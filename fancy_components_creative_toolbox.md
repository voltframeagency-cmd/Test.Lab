# Fancy Components Creative Developer Toolbox Reference Guide

A complete, structured developer guide for **Fancy Components** — a collection of fun, weird, and highly interactive React + Framer Motion components and microinteractions.

## Table of Contents

- [Getting Started](#getting-started)
  - [Introduction](#introduction)
  - [Installation](#installation)
  - [Changelog](#changelog)
- [Text](#text)
  - [Letter Swap Hover](#letter-swap-hover)
  - [Letter 3D Swap](#letter-3d-swap)
  - [Random Letter Swap Hover](#random-letter-swap-hover)
  - [Vertical Cut Reveal](#vertical-cut-reveal)
  - [Text Rotate](#text-rotate)
  - [Variable Font Hover By Letter](#variable-font-hover-by-letter)
  - [Variable Font Hover By Random Letter](#variable-font-hover-by-random-letter)
  - [Scroll And Swap](#scroll-and-swap)
  - [Text Cursor Proximity](#text-cursor-proximity)
  - [Variable Font & Cursor](#variable-font--cursor)
  - [Variable Font Cursor Proximity](#variable-font-cursor-proximity)
  - [Breathing Text](#breathing-text)
  - [Underline Animation](#underline-animation)
  - [Underline To Background](#underline-to-background)
  - [Basic Number Ticker](#basic-number-ticker)
  - [Typewriter](#typewriter)
  - [Scramble Hover](#scramble-hover)
  - [Text Highlighter](#text-highlighter)
  - [Scramble In](#scramble-in)
  - [Text Along Path](#text-along-path)
- [Carousel](#carousel)
  - [Box Carousel](#box-carousel)
- [Background](#background)
  - [Animated Gradient With SVG](#animated-gradient-with-svg)
  - [Pixel Trail](#pixel-trail)
- [Physics](#physics)
  - [Elastic Line](#elastic-line)
  - [Gravity](#gravity)
  - [Cursor Attractor & Gravity](#cursor-attractor--gravity)
- [Image](#image)
  - [Image Trail](#image-trail)
  - [Parallax Floating](#parallax-floating)
- [Filter](#filter)
  - [Gooey SVG Filter](#gooey-svg-filter)
  - [Pixelate SVG Filter](#pixelate-svg-filter)
- [Blocks](#blocks)
  - [Drag Elements](#drag-elements)
  - [Circling Elements](#circling-elements)
  - [Media Between Text](#media-between-text)
  - [CSS Box](#css-box)
  - [Screensaver](#screensaver)
  - [Sticky Footer](#sticky-footer)
  - [Float](#float)
  - [Stacking Cards](#stacking-cards)
  - [Simple Marquee](#simple-marquee)
  - [Marquee Along SVG Path](#marquee-along-svg-path)

---

## Getting Started <a name="getting-started"></a>

### Introduction <a name="introduction"></a>

Fancy Components is a collection of fun and weird, ready-to-use components and microinteractions built (mainly) with:

- React
- TypeScript
- Tailwind CSS
- Motion (Formerly Framer Motion)

## Why

The web UI landscape has become increasingly standardized. While this standardization has brought many benefits — improved accessibility, better user experiences, and easier development — it has also led to a certain homogeneity. The web, in many ways, has become a bit... boring.

The aim of this project is to somewhat inject some playfulness to the web, by collecting the weird, fun, and sometimes useless ui magic that you can see in the wild.

Now you might ask, then why create a copy-and-paste-able collection of components? The goal here is to re-create the most popular components from the award-winning sites, so you can either use them in your project by adding your spin on top of them, or just to provide the code as an inspiration for creating your own components.

## Getting Started

To start using the components in your project, check out the [Installation](/docs/installation) guide.

## Open Source & Free

Fancy Components is open source and free to use. You can use it for personal or commercial projects.

Contributions are welcome! If you have any ideas or suggestions, please feel free to open an issue or submit a pull request. A detailed contribution guide will be available soon.


---

### Installation <a name="installation"></a>

The components are built with React, TypeScript, Tailwind CSS, and mainly with Motion (formerly Framer Motion).
In order to use the components, you need to install the following dependencies:


```bash
# Install command:
tailwindcss@latest motion
```


Or follow the [Tailwind CSS installation guide](https://tailwindcss.com/docs/installation) to setup Tailwind CSS in your project.

Some components uses other libraries too, in each case, you can find the installation instructions in the component's documentation.

## Adding a component

You have two options to add a component to your project:

1. Using the CLI
2. Manually

### Using the CLI

If your project does not already have a `components.json`, initialize shadcn first:


```bash
# Install command:
npx shadcn@latest init
```


Then add the `@fancy` registry to the existing `registries` field in your `components.json`:


**File**: `components.json`
```json
{
  // ...
  "registries": {
    "@fancy": "https://fancycomponents.dev/r/{name}.json"
  }
}
```


To add a component using the CLI, you need to run the following command in your terminal:


```bash
# Install command:
npx shadcn add @fancy/component-name
```


This will add the component source code and its dependencies in your project. You can then import the component in your code and start using it. 

**Important note:** as of now, additional dependencies aren't installed automatically, they just get added to the `package.json` file. You need to run `npm install` or the equivalent command in your preferred package manager to install them.

The appropriate link is available in the "Installation" section of each component's documentation page.

### Manually

Go to any of the components you want to use and copy the source code(s) from the "Installation" section. Make sure to update your imports accordingly. 

## Notes

If you prefer javascript instead of typescript, you can easily convert the code with a tool like [this](https://transform.tools/typescript-to-javascript).


---

### Changelog <a name="changelog"></a>

## 2025 June

### New Component

- [Box Carousel](/docs/components/carousel/box-carousel)

### Updated components

Some components have been updated with better a11y support, improved documentation, and bug fixes, but there are no breaking changes.
This is an ongoing effort which will continue in the future as well.

## 2025 May

### New Components

- [Text Highlighter](/docs/components/text/text-highlighter)
- [Letter 3D Swap](/docs/components/text/letter-3d-swap)
- [Image Trail](/docs/components/image/image-trail)
- [CSS Box](/docs/components/blocks/css-box)

## 2025 Apr

### New Components

- [Simple Marquee](/docs/components/blocks/simple-marquee)
- [Marquee Along SVG Path](/docs/components/blocks/marquee-along-svg-path)

### Framer integration

Some of the components are now compatible with Framer, thanks to the work of [Framer University](https://x.com/learnframer) & [Achille](https://x.com/achilleernlt).

Available components for remixing:

- [Scramble In](/docs/components/text/scramble-in)
- [Pixel Trail](/docs/components/background/pixel-trail)
- [Elastic Line](/docs/components/physics/elastic-line)
- [Cursor Attractor & Gravity](/docs/components/physics/cursor-attractor-gravity)
- [Parallax Floating](/docs/components/image/parallax-floating)
- [Circling Elements](/docs/components/blocks/circling-elements)
- [Screensaver](/docs/components/blocks/screensaver)

## 2025 March

### Tailwind v4 Migration 🎉
We've upgraded to Tailwind v4! This major version brings several important changes:

**Breaking Changes:**
- Configuration is now stored directly in `global.css` instead of `tailwind.config.ts`
- The open-in-v0 feature currently doesn't support CSS variables - these need to be added manually
- CLI installation behavior has changed: CSS variables won't automatically merge with `global.css` when using layers, so you need to add them manually for now. 
- Syntax changes for Tailwind classes and colors (see official [migration guide](https://tailwindcss.com/docs/upgrade-guide))

**Affected Components:**
- Circling Elements
- Animated Gradient With SVG

**Documentation Updates:**
- Contribution guidelines have been updated to reflect v4 changes

For a complete overview of syntax changes and new features, please refer to the official [Tailwind v4 announcement](https://tailwindcss.com/blog/tailwindcss-v4#css-first-configuration).


---

## Text <a name="text"></a>

### Letter Swap Hover <a name="letter-swap-hover"></a>

### Demo Code (letter-swap-demo.tsx)
```tsx
import LetterSwapForward from "@/fancy/components/text/letter-swap-forward-anim"
import LetterSwapPingPong from "@/fancy/components/text/letter-swap-pingpong-anim"

export default function Preview() {
  return (
    <div >
      <div >
        <LetterSwapForward
          label="Hover me chief!"
          reverse={true}
          
        />
        <LetterSwapForward
          label="{awesome}"
          reverse={false}
          
        />
        <LetterSwapForward
          label="Good day!"
          staggerFrom={"center"}
          
        />
        <LetterSwapPingPong
          label="More text?"
          staggerFrom={"center"}
          reverse={false}
          
        />
        <LetterSwapPingPong label="oh, seriously?!" staggerFrom={"last"} />
      </div>
    </div>
  )
}
```


There are two types of animations available for this component:

1. Forward animation — plays the animation timeline once forward, when you hover over the text.
2. Ping Pong animation — plays the animation timeline in a ping pong fashion. It plays once forward when you hover over the text, and once in the opposite direction when you hover away from the text.

## Installation

### Only forward animation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/letter-swap-forward-anim
```






### Component Source Code (letter-swap-forward-anim.tsx)
```tsx
"use client"

import { useState } from "react"
import { AnimationOptions, motion, stagger, useAnimate } from "motion/react"

interface TextProps {
  label: string
  reverse?: boolean
  transition?: AnimationOptions
  staggerDuration?: number
  staggerFrom?: "first" | "last" | "center" | number
  className?: string
  onClick?: () => void
}

const LetterSwapForward = ({
  label,
  reverse = true,
  transition = {
    type: "spring",
    duration: 0.7,
  },
  staggerDuration = 0.03,
  staggerFrom = "first",
  className,
  onClick,
  ...props
}: TextProps) => {
  const [scope, animate] = useAnimate()
  const [blocked, setBlocked] = useState(false)

  const hoverStart = () => {
    if (blocked) return

    setBlocked(true)

    // Function to merge user transition with stagger and delay
    const mergeTransition = (baseTransition: AnimationOptions) => ({
      ...baseTransition,
      delay: stagger(staggerDuration, {
        from: staggerFrom,
      }),
    })

    animate(
      ".letter",
      { y: reverse ? "100%" : "-100%" },
      mergeTransition(transition)
    ).then(() => {
      animate(
        ".letter",
        {
          y: 0,
        },
        {
          duration: 0,
        }
      ).then(() => {
        setBlocked(false)
      })
    })

    animate(
      ".letter-secondary",
      {
        top: "0%",
      },
      mergeTransition(transition)
    ).then(() => {
      animate(
        ".letter-secondary",
        {
          top: reverse ? "-100%" : "100%",
        },
        {
          duration: 0,
        }
      )
    })
  }

  return (
    
      {label}

      {label.split("").map((letter: string, i: number) => {
        return (
          
            <motion.span className={`relative letter`} style={{ top: 0 }}>
              {letter}
            </motion.span>
            <motion.span
              
              style={{ top: reverse ? "-100%" : "100%" }}
            >
              {letter}
            </motion.span>
          
        )
      })}
    
  )
}

export default LetterSwapForward
```





### Ping Pong animation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/letter-swap-pingpong-anim
```





Install the following dependencies:


```bash
# Install command:
lodash
```


Lodash is used for debouncing here — so the animation doesn't break on rapid hover changes.

Then, copy the source code:


### Component Source Code (letter-swap-pingpong-anim.tsx)
```tsx
"use client"

import { useState } from "react"
import { debounce } from "lodash"
import { AnimationOptions, motion, stagger, useAnimate } from "motion/react"

interface TextProps {
  label: string
  reverse?: boolean
  transition?: AnimationOptions
  staggerDuration?: number
  staggerFrom?: "first" | "last" | "center" | number
  className?: string
  onClick?: () => void
}

const LetterSwapPingPong = ({
  label,
  reverse = true,
  transition = {
    type: "spring",
    duration: 0.7,
  },
  staggerDuration = 0.03,
  staggerFrom = "first",
  className,
  onClick,
  ...props
}: TextProps) => {
  const [scope, animate] = useAnimate()
  const [isHovered, setIsHovered] = useState(false)

  const mergeTransition = (baseTransition: AnimationOptions) => ({
    ...baseTransition,
    delay: stagger(staggerDuration, {
      from: staggerFrom,
    }),
  })

  const hoverStart = debounce(
    () => {
      if (isHovered) return
      setIsHovered(true)

      animate(
        ".letter",
        { y: reverse ? "100%" : "-100%" },
        mergeTransition(transition)
      )

      animate(
        ".letter-secondary",
        {
          top: "0%",
        },
        mergeTransition(transition)
      )
    },
    100,
    { leading: true, trailing: true }
  )

  const hoverEnd = debounce(
    () => {
      setIsHovered(false)

      animate(
        ".letter",
        {
          y: 0,
        },
        mergeTransition(transition)
      )

      animate(
        ".letter-secondary",
        {
          top: reverse ? "-100%" : "100%",
        },
        mergeTransition(transition)
      )
    },
    100,
    { leading: true, trailing: true }
  )

  return (
    <motion.span
      className={`flex justify-center items-center relative overflow-hidden  ${className} `}
      onHoverStart={hoverStart}
      onHoverEnd={hoverEnd}
      onClick={onClick}
      ref={scope}
      {...props}
    >
      {label}

      {label.split("").map((letter: string, i: number) => {
        return (
          
            <motion.span className={`relative letter`} style={{ top: 0 }}>
              {letter}
            </motion.span>
            <motion.span
              
              style={{ top: reverse ? "-100%" : "100%" }}
            >
              {letter}
            </motion.span>
          
        )
      })}
    </motion.span>
  )
}

export default LetterSwapPingPong
```





## Understanding the component

1. First, we duplicate the text we want to animate. We'll have two identical copies of the text.

2. We create a container `` element with these key properties:

   - `position: relative` - This establishes a positioning context
   - `overflow: hidden` - This ensures text outside the container boundaries is hidden

3. For each copy of the text:

   - We split it into individual letters
   - Each letter is wrapped in its own `` element, with `absolute` positioning
   - The letters from both copies are stacked vertically on top of each other (`top: 0`, and `top: 100%`)

4. When hovering:
   - The original letters slide upward out of view (hidden by overflow), by setting `top: 100%`
   - The duplicate letters slide up into the original position, by setting `top: 0`
   - This creates a smooth swapping effect

If `reverse` is enabled, the animation direction is flipped.

### Stagger

With the `staggerFrom` prop, you can control the index of the letter where the stagger animation starts.


### Demo Code (letter-swap-demo-stagger.tsx)
```tsx
import LetterSwapForward from "@/fancy/components/text/letter-swap-forward-anim"

export default function Preview() {
  return (
    <div >
      <LetterSwapForward label="First" staggerFrom={"first"} />
      <LetterSwapForward label="Center" staggerFrom={"center"}  />
      <LetterSwapForward label="Last" staggerFrom={"last"} />
    </div>
  )
}
```


### Line swap

By setting the `staggerDelay` prop to zero, you can create a line swap effect.


### Demo Code (letter-swap-demo-line.tsx)
```tsx
import LetterSwapForward from "@/fancy/components/text/letter-swap-forward-anim"

export default function Preview() {
  return (
    <div >
      <div >
        <LetterSwapForward label="oh, wow!" staggerDuration={0} />
        <LetterSwapForward label="nice!" staggerDuration={0} reverse={false} />
      </div>
    </div>
  )
}
```


## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| label* | `string` | - | The text to be displayed and animated |
| reverse | `boolean` | `true` | Direction of the animation (true: bottom to top, false: top to bottom) |
| transition | `AnimationOptions` | `{ type: "spring", duration: 0.7 }` | Animation configuration for each letter. Refer to motion docs for more details |
| staggerDuration | `number` | `0.03` | Delay between each letter's animation start |
| staggerFrom | `"first" | "last" | "center" | number` | `"first"` | Starting point of the stagger effect |
| className | `string` | - | Additional CSS classes for styling |
| onClick | `() => void` | - | Callback function for click events |


---

### Letter 3D Swap <a name="letter-3d-swap"></a>

<ComponentPreview name='letter-3d-swap-demo' />

## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/letter-3d-swap
```






### Component Source Code (letter-3d-swap.tsx)
```tsx
"use client"

import React, { ElementType, useCallback, useMemo, useState } from "react"
import {
  AnimationOptions,
  useAnimate,
  ValueAnimationTransition,
} from "motion/react"

import { cn } from "@/lib/utils"

// handy function to split text into characters with support for unicode and emojis
const splitIntoCharacters = (text: string): string[] => {
  if (typeof Intl !== "undefined" && "Segmenter" in Intl) {
    const segmenter = new Intl.Segmenter("en", { granularity: "grapheme" })
    return Array.from(segmenter.segment(text), ({ segment }) => segment)
  }
  // Fallback for browsers that don't support Intl.Segmenter
  return Array.from(text)
}

// handy function to extract text from children
const extractTextFromChildren = (children: React.ReactNode): string | undefined => {
  // Handle null/undefined
  if (children == null) return ""

  // Handle string
  if (typeof children === "string") return children

  // Handle number
  if (typeof children === "number") return String(children)

  // Handle arrays (including fragments)
  if (Array.isArray(children)) {
    return children.map(extractTextFromChildren).join("")
  }

  // Handle React elements
  if (React.isValidElement(children)) {
    const props = (children as React.ReactElement).props
    const childText = (props as any).children as React.ReactNode

    // Recursively extract text from children
    if (childText != null) {
      return extractTextFromChildren(childText)
    }

    return ""
  }
}

/**
 * Internal helper interface for representing a word in the text with its characters and spacing information
 */
interface WordObject {
  /**
   * Array of individual characters in the word
   */
  characters: string[]
  /**
   * Whether this word needs a space after it
   */
  needsSpace: boolean
}

interface Letter3DSwapProps {
  /**
   * The content to be displayed and animated
   */
  children: React.ReactNode

  /**
   * HTML Tag to render the component as
   */
  as?: ElementType
  /**
   * Class name for the main container element.
   */
  mainClassName?: string

  /**
   * Class name for the front face element.
   */
  frontFaceClassName?: string

  /**
   * Class name for the secondary face element.
   */
  secondFaceClassName?: string

  /**
   * Duration of stagger delay between elements in seconds.
   * @default 0.05
   */
  staggerDuration?: number

  /**
   * Direction to stagger animations from.
   * @default "first"
   */
  staggerFrom?: "first" | "last" | "center" | number | "random"

  /**
   * Animation transition configuration.
   * @default { type: "spring", damping: 25, stiffness: 300 }
   */
  transition?: ValueAnimationTransition | AnimationOptions

  /**
   * Direction of rotation
   * @default "right"
   */
  rotateDirection?: "top" | "right" | "bottom" | "left"
}

const Letter3DSwap = ({
  children,
  as = "p",
  mainClassName,
  frontFaceClassName,
  secondFaceClassName,
  staggerDuration = 0.05,
  staggerFrom = "first",
  transition = { type: "spring", damping: 30, stiffness: 300 },
  rotateDirection = "right",
  ...props
}: Letter3DSwapProps) => {
  const [isAnimating, setIsAnimating] = useState(false)
  const [isHovering, setIsHovering] = useState(false)
  const [scope, animate] = useAnimate()

  // Determine rotation transform based on direction
  const rotationTransform = (() => {
    switch (rotateDirection) {
      case "top":
        return "rotateX(90deg)"
      case "right":
        return "rotateY(90deg)"
      case "bottom":
        return "rotateX(-90deg)"
      case "left":
        return "rotateY(90deg)"
      default:
        return "rotateY(-90deg)"
    }
  })()

  // Convert children to string for processing with error handling
  const text = useMemo(() => {
    try {
      return extractTextFromChildren(children)
    } catch (error) {
      console.error(error)
      return ""
    }
  }, [children])

  // Splitting the text into animation segments
  const characters = useMemo(() => {
    const t = text?.split(" ") ?? []
    const result = t.map((word: string, i: number) => ({
      characters: splitIntoCharacters(word),
      needsSpace: i !== t.length - 1,
    }))
    return result
  }, [text])

  // Helper function to calculate stagger delay for each text segment
  const getStaggerDelay = useCallback(
    (index: number, totalChars: number) => {
      const total = totalChars
      if (staggerFrom === "first") return index * staggerDuration
      if (staggerFrom === "last") return (total - 1 - index) * staggerDuration
      if (staggerFrom === "center") {
        const center = Math.floor(total / 2)
        return Math.abs(center - index) * staggerDuration
      }
      if (staggerFrom === "random") {
        const randomIndex = Math.floor(Math.random() * total)
        return Math.abs(randomIndex - index) * staggerDuration
      }
      return Math.abs(staggerFrom - index) * staggerDuration
    },
    [staggerFrom, staggerDuration]
  )

  // Handle hover start - trigger the rotation
  const handleHoverStart = useCallback(async () => {
    if (isAnimating || isHovering) return

    setIsHovering(true)
    setIsAnimating(true)

    const totalChars = characters.reduce(
      (sum: number, word: WordObject) => sum + word.characters.length,
      0
    )

    // Create delays array based on staggerFrom
    const delays = Array.from({ length: totalChars }, (_, i) => {
      return getStaggerDelay(i, totalChars)
    })

    // Animate each character with its specific delay
    await animate(
      ".letter-3d-swap-char-box-item",
      { transform: rotationTransform },
      {
        ...transition,
        delay: (i: number) => delays[i],
      }
    )

    // Reset all boxes
    await animate(
      ".letter-3d-swap-char-box-item",
      { transform: "rotateX(0deg) rotateY(0deg)" },
      { duration: 0 }
    )

    setIsAnimating(false)
  }, [
    isAnimating,
    isHovering,
    characters,
    transition,
    getStaggerDelay,
    rotationTransform,
    animate,
  ])

  // Handle hover end
  const handleHoverEnd = useCallback(() => {
    setIsHovering(false)
  }, [])

  const ElementTag = as ?? "p"

  return (
    <ElementTag
      className={cn("flex flex-wrap relative", mainClassName)}
      onMouseEnter={handleHoverStart}
      onMouseLeave={handleHoverEnd}
      ref={scope}
      {...props}
    >
      {text}

      {characters.map(
        (wordObj: WordObject, wordIndex: number, array: WordObject[]) => {
          const previousCharsCount = array
            .slice(0, wordIndex)
            .reduce(
              (sum: number, word: WordObject) => sum + word.characters.length,
              0
            )

          return (
            
              {wordObj.characters.map((char: string, charIndex: number) => {
                const totalIndex = previousCharsCount + charIndex

                return (
                  <CharBox
                    key={totalIndex}
                    char={char}
                    frontFaceClassName={frontFaceClassName}
                    secondFaceClassName={secondFaceClassName}
                    rotateDirection={rotateDirection}
                  />
                )
              })}
              {wordObj.needsSpace &&  }
            
          )
        }
      )}
    </ElementTag>
  )
}

interface CharBoxProps {
  char: string
  frontFaceClassName?: string
  secondFaceClassName?: string
  rotateDirection: "top" | "right" | "bottom" | "left"
}

const CharBox = ({
  char,
  frontFaceClassName,
  secondFaceClassName,
  rotateDirection,
}: CharBoxProps) => {
  // Get the transform for the second face based on rotation direction
  const getSecondFaceTransform = () => {
    switch (rotateDirection) {
      case "top":
        return `rotateX(-90deg) translateZ(0.5lh)`
      case "right":
        return `rotateY(90deg) translateX(50%) rotateY(-90deg) translateX(-50%) rotateY(-90deg) translateX(50%)`
      case "bottom":
        return `rotateX(90deg) translateZ(0.5lh)`
      case "left":
        return `rotateY(90deg) translateX(50%) rotateY(-90deg) translateX(50%) rotateY(-90deg) translateX(50%)`
      default:
        return `rotateY(90deg) translateZ(1ch)`
    }
  }

  const secondFaceTransform = getSecondFaceTransform()

  return (
    
      {/* Front face */}
      
        {char}
      

      {/* Second face - positioned based on rotation direction */}
      
        {char}
      
    
  )
}

Letter3DSwap.displayName = "Letter3DSwap"

export default Letter3DSwap
```





## Usage

Just wrap your text with the component and set the `rotateDirection` prop to the direction you want the text to rotate, the rest will be taken care by the component.

## Understanding the component

### Splitting the text into characters

First, we split the text into `WorldObject` objects, each containing an array of characters and a boolean indicating whether there should be a space after the character. We use a handy function for this, which should respect emojis too.

**File**: `Splitting the text into characters`
```tsx
// handy function to split text into characters with support for unicode and emojis
const splitIntoCharacters = (text: string): string[] => {
  if (typeof Intl !== "undefined" && "Segmenter" in Intl) {
    const segmenter = new Intl.Segmenter("en", { granularity: "grapheme" })
    return Array.from(segmenter.segment(text), ({ segment }) => segment)
  }
  // Fallback for browsers that don't support Intl.Segmenter
  return Array.from(text)
}
```


This method also helps us to ensure that words stay together and properly spaced when the text wraps across multiple lines. Without this approach, simply splitting by characters would break words at line boundaries.


**File**: `Splitting the text into animation segments`
```tsx
// Splitting the text into animation segments
const characters = useMemo(() => {
    const t = text.split(" ")
    const result = t.map((word: string, i: number) => ({
      characters: splitIntoCharacters(word),
      needsSpace: i !== t.length - 1,
    }))
    return result
}, [text])
```


### 3D Transforms

When rendering each character, we create two instances of it - a front face and a second face. The second face is positioned relative to the first one and uses 3D CSS transforms to create the illusion that it's on a different face of a 3D box. The face it appears on depends on the `rotateDirection` prop:

- `"top"` - Character appears to flip upward from the top face
- `"right"` - Character appears to flip from the right side 
- `"bottom"` - Character appears to flip downward from the bottom face
- `"left"` - Character appears to flip from the left side


#### Top and bottom rotations

For top and bottom rotations, we create a 3D box effect through a series of transforms:

1. The front face is brought forward by translating it `0.5lh` on the Z axis (`lh` represents one line height)
2. For the second face, we:
   - Rotate it 90° (or -90°) on the X axis
   - Then translate it `0.5lh` forward in its local coordinate system to align with the edge of our virtual box
3. Finally, we translate the container back by `-0.5lh` to account for the initial translation of the front face

This creates the illusion of characters flipping between two faces of a 3D cube. The demo below shows how these transforms work together:

<ExplanationDemo name='letter-3d-swap-explanation-top-demo' />

#### Left and right rotations

For left/right rotations, we need to handle the box dimensions more carefully. Unlike top/bottom rotations where we can use line height (`lh`) as a fixed measurement, the width of each character varies. The side faces of our 3D box need to match the actual character width.

To achieve this, we use percentage-based translations on the X and Y axes, since these can automatically adapt to each character's width. The transform sequence works like this:

1. First face:
   - Rotate 90° on Y axis to face sideways
   - Translate 50% of character width to align with edge
   - Rotate -90° on Y axis to face forward again
   
2. Second face:
   - Apply the same transforms as the first face
   - Add additional transforms to position it correctly on the side

3. Lastly, we push back both faces to account for the initial translation

The demo below shows this transform sequence step by step:

<ExplanationDemo name='letter-3d-swap-explanation-left-demo' />

#### Why the initial translation?

The initial forward translation of our box (using `0.5lh` for `top`/`bottom` rotations, or the transform chain for `left`/`right` rotations) serves an important purpose. It ensures the rotation axis passes through the center of our virtual 3D box, rather than along its front face. This creates a more natural flipping motion, as the character rotates around its center point rather than pivoting from its front edge. Without this translation, the box rotation would appear to swing outward in an unnatural arc rather than flipping in place.

Of course, you can achieve the same result by applying (other) transforms in a different order, and even playing with the transform origins. I apologise if this seems overcomplicated, this is how it made sense to me :).

### Animation

Now that we have our virtual 3D box, the only thing left is to rotate each character box. For this, we use the `useAnimate` hook from [motion](https://motion.dev/docs/use-animate). This gives us a scope and an `animate` function to control the animation. We add `.letter-3d-swap-char-box-item` class name to each char box, so we can select and animate them with the `animate` function. After the animation is completed, we reset the transform to the original state.


**File**: `Animation`
```tsx
// Animate each character with its specific delay
await animate(
  ".letter-3d-swap-char-box-item",
  { transform: rotationTransform },
  {
    ...transition,
    delay: (i: number) => delays[i],
  }

// Reset all boxes
await animate(
  ".letter-3d-swap-char-box-item",
  { transform: "rotateX(0deg) rotateY(0deg)" },
  { duration: 0 }
)
```


The transform is just a 90/-90 degree rotation either on the X or Y axis, depending on the `rotateDirection` prop.

### Stagger

The delay is calculated based on the `staggerFrom` prop, which can be set to `first`, `last`, `center`, `random` or a number. If it's a number, it's used as the index of the character to stagger from. For example, if `staggerFrom` is set to `2`, the second character will be staggered from the third one. We have a handy function to calculate the correct delay for each character:


**File**: `Stagger delay calculation`
```tsx
// Helper function to calculate stagger delay for each text segment
const getStaggerDelay = useCallback(
  (index: number, totalChars: number) => {
    const total = totalChars
    if (staggerFrom === "first") return index * staggerDuration
    if (staggerFrom === "last") return (total - 1 - index) * staggerDuration
    if (staggerFrom === "center") {
      const center = Math.floor(total / 2)
      return Math.abs(center - index) * staggerDuration
    }
    if (staggerFrom === "random") {
      const randomIndex = Math.floor(Math.random() * total)
      return Math.abs(randomIndex - index) * staggerDuration
    }
    return Math.abs(staggerFrom - index) * staggerDuration
  },
  [staggerFrom, staggerDuration]
)
```


Check out the demo to see the possible values for `staggerFrom`.

<ComponentPreview name='letter-3d-swap-stagger-demo' />

## Resources

- [Intro to CSS 3D transforms](https://3dtransforms.desandro.com/) by David DeSandro

## Props

### Letter3DSwapProps


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The content to be displayed and animated |
| as | `ElementType` | `p` | HTML Tag to render the component as |
| mainClassName | `string` | - | Class name for the main container element |
| frontFaceClassName | `string` | - | Class name for the front face element |
| secondFaceClassName | `string` | - | Class name for the secondary face element |
| staggerDuration | `number` | `0.05` | Duration of stagger delay between elements in seconds |
| staggerFrom | `"first" | "last" | "center" | "random" | number` | `"first"` | Direction to stagger animations from |
| transition | `ValueAnimationTransition | AnimationOptions` | `{ type: "spring", damping: 25, stiffness: 300 }` | Animation transition configuration |
| rotateDirection | `"top" | "right" | "bottom" | "left"` | `"right"` | Direction of rotation |


---

### Random Letter Swap Hover <a name="random-letter-swap-hover"></a>

### Demo Code (random-letter-swap-demo.tsx)
```tsx
import RandomLetterSwapForward from "@/fancy/components/text/random-letter-swap-forward-anim"
import RandomLetterSwapPingPong from "@/fancy/components/text/random-letter-swap-pingpong-anim"

export default function Preview() {
  return (
    <div >
      <div >
        <RandomLetterSwapForward
          label="Right here!"
          reverse={true}
          
        />
        <RandomLetterSwapForward
          label="Right now!"
          reverse={false}
          
        />
        <RandomLetterSwapPingPong label="Right here!"  />
        <RandomLetterSwapPingPong
          label="Right now!"
          reverse={false}
          
        />
      </div>
    </div>
  )
}
```


There are two types of animations available for this component:

1. Forward animation — plays the animation timeline once forward.
2. Ping Pong animation — plays the animation timeline in a ping pong fashion.

## Installation

### Only forward animation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/random-letter-swap-forward-anim
```





Install the following dependencies:


```bash
# Install command:
lodash
```


Lodash is used for debouncing here — so the animation doesn't break on rapid hover changes.

Then, copy and paste the following code into your project:


### Component Source Code (random-letter-swap-forward-anim.tsx)
```tsx
"use client"

import { useState } from "react"
import { debounce } from "lodash"
import { AnimationOptions, motion, useAnimate } from "motion/react"

interface TextProps {
  label: string
  reverse?: boolean
  transition?: AnimationOptions
  staggerDuration?: number
  className?: string
  onClick?: () => void
}

const RandomLetterSwapForward = ({
  label,
  reverse = true,
  transition = {
    type: "spring",
    duration: 0.8,
  },
  staggerDuration = 0.02,
  className,
  onClick,
  ...props
}: TextProps) => {
  const [scope, animate] = useAnimate()
  const [blocked, setBlocked] = useState(false)

  const mergeTransition = (transition: AnimationOptions, i: number) => ({
    ...transition,
    delay: i * staggerDuration,
  })

  const shuffledIndices = Array.from(
    { length: label.length },
    (_, i) => i
  ).sort(() => Math.random() - 0.5)

  const hoverStart = debounce(
    () => {
      if (blocked) return
      setBlocked(true)

      for (let i = 0; i < label.length; i++) {
        const randomIndex = shuffledIndices[i]
        animate(
          ".letter-" + randomIndex,
          {
            y: reverse ? "100%" : "-100%",
          },
          mergeTransition(transition, i)
        ).then(() => {
          animate(
            ".letter-" + randomIndex,
            {
              y: 0,
            },
            {
              duration: 0,
            }
          )
        })

        animate(
          ".letter-secondary-" + randomIndex,
          {
            top: "0%",
          },
          mergeTransition(transition, i)
        )
          .then(() => {
            animate(
              ".letter-secondary-" + randomIndex,
              {
                top: reverse ? "-100%" : "100%",
              },
              {
                duration: 0,
              }
            )
          })
          .then(() => {
            if (i === label.length - 1) {
              setBlocked(false)
            }
          })
      }
    },
    100,
    { leading: true, trailing: true }
  )

  return (
    <motion.span
      className={`flex justify-center items-center relative overflow-hidden ${className}`}
      onHoverStart={hoverStart}
      onClick={onClick}
      ref={scope}
      {...props}
    >
      {label}

      {label.split("").map((letter: string, i: number) => {
        return (
          
            <motion.span
              className={`relative pb-2 letter-${i}`}
              style={{ top: 0 }}
            >
              {letter}
            </motion.span>
            <motion.span
              className={`absolute letter-secondary-${i}`}
              style={{ top: reverse ? "-100%" : "100%" }}
            >
              {letter}
            </motion.span>
          
        )
      })}
    </motion.span>
  )
}

export default RandomLetterSwapForward
```





### Ping Pong animation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/random-letter-swap-pingpong-anim
```





Install the following dependencies:


```bash
# Install command:
lodash
```


Lodash is used for debouncing here — so the animation doesn't break on rapid hover changes.

Then, copy and paste the following code into your project:


### Component Source Code (random-letter-swap-pingpong-anim.tsx)
```tsx
"use client"

import { useState } from "react"
import { debounce } from "lodash"
import { AnimationOptions, motion, useAnimate } from "motion/react"

interface TextProps {
  label: string
  reverse?: boolean
  transition?: AnimationOptions
  staggerDuration?: number
  className?: string
  onClick?: () => void
}

const RandomLetterSwapPingPong = ({
  label,
  reverse = true,
  transition = {
    type: "spring",
    duration: 0.8,
  },
  staggerDuration = 0.02,
  className,
  onClick,
  ...props
}: TextProps) => {
  const [scope, animate] = useAnimate()
  const [blocked, setBlocked] = useState(false)

  const mergeTransition = (transition: AnimationOptions, i: number) => ({
    ...transition,
    delay: i * staggerDuration,
  })

  const shuffledIndices = Array.from(
    { length: label.length },
    (_, i) => i
  ).sort(() => Math.random() - 0.5)

  const hoverStart = debounce(
    () => {
      if (blocked) return
      setBlocked(true)

      for (let i = 0; i < label.length; i++) {
        const randomIndex = shuffledIndices[i]
        animate(
          ".letter-" + randomIndex,
          {
            y: reverse ? "100%" : "-100%",
          },
          mergeTransition(transition, i)
        )

        animate(
          ".letter-secondary-" + randomIndex,
          {
            top: "0%",
          },
          mergeTransition(transition, i)
        )
      }
    },
    100,
    { leading: true, trailing: true }
  )

  const hoverEnd = debounce(
    () => {
      setBlocked(false)

      for (let i = 0; i < label.length; i++) {
        const randomIndex = shuffledIndices[i]
        animate(
          ".letter-" + randomIndex,
          {
            y: 0,
          },
          mergeTransition(transition, i)
        )

        animate(
          ".letter-secondary-" + randomIndex,
          {
            top: reverse ? "-100%" : "100%",
          },
          mergeTransition(transition, i)
        )
      }
    },
    100,
    { leading: true, trailing: true }
  )

  return (
    <motion.span
      className={`flex justify-center items-center relative overflow-hidden  ${className} `}
      onHoverStart={hoverStart}
      onHoverEnd={hoverEnd}
      onClick={onClick}
      ref={scope}
      {...props}
    >
      {label}

      {label.split("").map((letter: string, i: number) => {
        return (
          
            <motion.span
              className={`relative pb-2 letter-${i}`}
              style={{ top: 0 }}
            >
              {letter}
            </motion.span>
            <motion.span
              className={`absolute letter-secondary-${i}`}
              style={{ top: reverse ? "-100%" : "100%" }}
            >
              {letter}
            </motion.span>
          
        )
      })}
    </motion.span>
  )
}

export default RandomLetterSwapPingPong
```





## Understanding the component

The component works the same as the [Letter Swap Hover](/docs/components/text/letter-swap) component, but with a random letter swapping animation (and of course, a slightly different implementation of the animation). Please refer to that documentation for more details.

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| label* | `string` | - | The text to be displayed and animated |
| reverse | `boolean` | `true` | Direction of the animation (true: bottom to top, false: top to bottom) |
| transition | `AnimationOptions` | `{ type: "spring", duration: 0.7 }` | Animation configuration for each letter |
| staggerDuration | `number` | `0.03` | Delay between each letter's animation start |
| className | `string` | - | Additional CSS classes for styling |
| onClick | `() => void` | - | Callback function for click events |


---

### Vertical Cut Reveal <a name="vertical-cut-reveal"></a>

### Demo Code (vertical-cut-reveal-demo.tsx)
```tsx
import VerticalCutReveal from "@/fancy/components/text/vertical-cut-reveal"

export default function Preview() {
  return (
    <div >
      <VerticalCutReveal
        splitBy="characters"
        staggerDuration={0.025}
        staggerFrom="first"
        transition={{
          type: "spring",
          stiffness: 200,
          damping: 21,
        }}
      >
        {`HI 👋, FRIEND!`}
      </VerticalCutReveal>
      <VerticalCutReveal
        splitBy="characters"
        staggerDuration={0.025}
        staggerFrom="last"
        reverse={true}
        transition={{
          type: "spring",
          stiffness: 200,
          damping: 21,
          delay: 0.5,
        }}
      >
        {`🌤️ IT IS NICE ⇗ TO`}
      </VerticalCutReveal>
      <VerticalCutReveal
        splitBy="characters"
        staggerDuration={0.025}
        staggerFrom="center"
        transition={{
          type: "spring",
          stiffness: 200,
          damping: 21,
          delay: 1.1,
        }}
      >
        {`MEET 😊 YOU.`}
      </VerticalCutReveal>
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/vertical-cut-reveal
```






### Component Source Code (vertical-cut-reveal.tsx)
```tsx
"use client"

import { AnimationOptions, motion } from "motion/react"
import {
  forwardRef,
  useCallback,
  useEffect,
  useImperativeHandle,
  useMemo,
  useRef,
  useState,
} from "react"

import { cn } from "@/lib/utils"

interface TextProps {
  children: React.ReactNode
  reverse?: boolean
  transition?: AnimationOptions
  splitBy?: "words" | "characters" | "lines" | string
  staggerDuration?: number
  staggerFrom?: "first" | "last" | "center" | "random" | number
  containerClassName?: string
  wordLevelClassName?: string
  elementLevelClassName?: string
  onClick?: () => void
  onStart?: () => void
  onComplete?: () => void
  autoStart?: boolean // Whether to start the animation automatically
}

// Ref interface to allow external control of the animation
export interface VerticalCutRevealRef {
  startAnimation: () => void
  reset: () => void
}

interface WordObject {
  characters: string[]
  needsSpace: boolean
}

const VerticalCutReveal = forwardRef<VerticalCutRevealRef, TextProps>(
  (
    {
      children,
      reverse = false,
      transition = {
        type: "spring",
        stiffness: 190,
        damping: 22,
      },
      splitBy = "words",
      staggerDuration = 0.2,
      staggerFrom = "first",
      containerClassName,
      wordLevelClassName,
      elementLevelClassName,
      onClick,
      onStart,
      onComplete,
      autoStart = true,
      ...props
    },
    ref
  ) => {
    const containerRef = useRef<HTMLSpanElement>(null)
    const text =
      typeof children === "string" ? children : children?.toString() || ""
    const [isAnimating, setIsAnimating] = useState(false)

    // handy function to split text into characters with support for unicode and emojis
    const splitIntoCharacters = (text: string): string[] => {
      if (typeof Intl !== "undefined" && "Segmenter" in Intl) {
        const segmenter = new Intl.Segmenter("en", { granularity: "grapheme" })
        return Array.from(segmenter.segment(text), ({ segment }) => segment)
      }
      // Fallback for browsers that don't support Intl.Segmenter
      return Array.from(text)
    }

    // Split text based on splitBy parameter
    const elements = useMemo(() => {
      const words = text.split(" ")
      if (splitBy === "characters") {
        return words.map((word, i) => ({
          characters: splitIntoCharacters(word),
          needsSpace: i !== words.length - 1,
        }))
      }
      return splitBy === "words"
        ? text.split(" ")
        : splitBy === "lines"
          ? text.split("\n")
          : text.split(splitBy)
    }, [text, splitBy])

    // Calculate stagger delays based on staggerFrom
    const getStaggerDelay = useCallback(
      (index: number) => {
        const total =
          splitBy === "characters"
            ? elements.reduce(
                (acc, word) =>
                  acc +
                  (typeof word === "string"
                    ? 1
                    : word.characters.length + (word.needsSpace ? 1 : 0)),
                0
              )
            : elements.length
        if (staggerFrom === "first") return index * staggerDuration
        if (staggerFrom === "last") return (total - 1 - index) * staggerDuration
        if (staggerFrom === "center") {
          const center = Math.floor(total / 2)
          return Math.abs(center - index) * staggerDuration
        }
        if (staggerFrom === "random") {
          const randomIndex = Math.floor(Math.random() * total)
          return Math.abs(randomIndex - index) * staggerDuration
        }
        return Math.abs(staggerFrom - index) * staggerDuration
      },
      [elements.length, staggerFrom, staggerDuration]
    )

    const startAnimation = useCallback(() => {
      setIsAnimating(true)
      onStart?.()
    }, [onStart])

    // Expose the startAnimation function via ref
    useImperativeHandle(ref, () => ({
      startAnimation,
      reset: () => setIsAnimating(false),
    }))

    // Auto start animation
    useEffect(() => {
      if (autoStart) {
        startAnimation()
      }
    }, [autoStart])

    const variants = {
      hidden: { y: reverse ? "-100%" : "100%" },
      visible: (i: number) => ({
        y: 0,
        transition: {
          ...transition,
          delay: ((transition?.delay as number) || 0) + getStaggerDelay(i),
        },
      }),
    }

    return (
      
        {text}

        {(splitBy === "characters"
          ? (elements as WordObject[])
          : (elements as string[]).map((el, i) => ({
              characters: [el],
              needsSpace: i !== elements.length - 1,
            }))
        ).map((wordObj, wordIndex, array) => {
          const previousCharsCount = array
            .slice(0, wordIndex)
            .reduce((sum, word) => sum + word.characters.length, 0)

          return (
            
              {wordObj.characters.map((char, charIndex) => (
                
                  <motion.span
                    custom={previousCharsCount + charIndex}
                    initial="hidden"
                    animate={isAnimating ? "visible" : "hidden"}
                    variants={variants}
                    onAnimationComplete={
                      wordIndex === elements.length - 1 &&
                      charIndex === wordObj.characters.length - 1
                        ? onComplete
                        : undefined
                    }
                    
                  >
                    {char}
                  </motion.span>
                
              ))}
              {wordObj.needsSpace &&  }
            
          )
        })}
      
    )
  }
)

VerticalCutReveal.displayName = "VerticalCutReveal"
export default VerticalCutReveal
```





## Understanding the component

1. First, the text is split into smaller pieces based on the `splitBy` prop:

   - `words`: Splits into individual words (e.g., "Hello world" → ["Hello", "world"])
   - `characters`: Splits into individual characters (e.g., "Hi" → ["H", "i"])
   - `lines`: Splits by newline characters (`\n`)
   - `string`: Splits by any custom string delimiter

2. Each piece of text is wrapped in two `` elements:

   - An outer `` that acts as a container with its position to `relative` and its overflow to `overflow-hidden`
   - An inner `` that holds the actual text, initially positioned off-screen using `y: 100` (or `-100` if `reverse` is true)

3. When the animation starts:
   - The inner `` elements smoothly transition from their off-screen position (`y: 100`) to their final position (`y: 0`)
   - This creates a "cutting" or "revealing" effect as each piece of text slides into view
   - The animation can be staggered from different directions (first, last, center, or random) using the `staggerFrom` prop

A key implementation detail is that the component always maintains word boundaries, even when splitting by characters. There are two reason for this:

1. When dealing with multi-line text, each line maintains its own reveal animation starting point. This means if you have text that spans multiple lines, each line will animate independently from its own baseline, rather than all elements animating from a single point (like the bottom of the entire paragraph).
2. When using `characters` mode, characters from the same word stay together in a word container. This prevents unwanted line breaks in the middle of words - if a word needs to wrap to the next line, it will wrap as a complete unit rather than having some characters on one line and others on the next line. This maintains proper text flow and readability while still allowing character-by-character animation within each word.

## Examples

### splitBy variations

With the `splitBy` prop, you can control how the text is split into smaller pieces. It can be either `words`, `characters`, `lines`, or a custom `string` delimiter.


### Demo Code (vertical-cut-reveal-line-demo.tsx)
```tsx
import VerticalCutReveal from "@/fancy/components/text/vertical-cut-reveal"

export default function Preview() {
  return (
    <div >
      <div >
        <VerticalCutReveal
          splitBy="lines"
          staggerDuration={0.2}
          staggerFrom="first"
          transition={{
            type: "spring",
            stiffness: 250,
            damping: 30,
            delay: 0.2,
          }}
          containerClassName="text-[#00000] leading-relaxed"
        >
          {"→ We're on a mission\nto make the 🌐 web \nsuper fun again! ☺"}
        </VerticalCutReveal>
      </div>
    </div>
  )
}
```



### Demo Code (vertical-cut-reveal-word-demo.tsx)
```tsx
import VerticalCutReveal from "@/fancy/components/text/vertical-cut-reveal"

export default function Preview() {
  return (
    <div >
      <div >
        <VerticalCutReveal
          splitBy="words"
          staggerDuration={0.1}
          staggerFrom="first"
          reverse={true}
          transition={{
            type: "spring",
            stiffness: 250,
            damping: 30,
            delay: 0,
          }}
        >
          {`super cool & awesome example text`}
        </VerticalCutReveal>
      </div>
    </div>
  )
}
```


### staggerFrom variations

With the `staggerFrom` prop, you can control the starting index of the animation. It can be either `first`, `last`, `center`, a `number` (custom index).


### Demo Code (vertical-cut-reveal-stagger-demo.tsx)
```tsx
import VerticalCutReveal from "@/fancy/components/text/vertical-cut-reveal"

export default function Preview() {
  return (
    <div >
      <div >
        <VerticalCutReveal
          splitBy="characters"
          staggerDuration={0.05}
          staggerFrom="first"
          transition={{
            type: "spring",
            stiffness: 200,
            damping: 21,
            delay: 0,
          }}
        >
          {`THIS STAGGERS FROM FIRST`}
        </VerticalCutReveal>
        <VerticalCutReveal
          splitBy="characters"
          staggerDuration={0.05}
          staggerFrom="last"
          reverse={true}
          transition={{
            type: "spring",
            stiffness: 200,
            damping: 21,
            delay: 1,
          }}
        >
          {`THIS STAGGERS FROM LAST`}
        </VerticalCutReveal>
        <VerticalCutReveal
          splitBy="characters"
          staggerDuration={0.05}
          staggerFrom="center"
          transition={{
            type: "spring",
            stiffness: 200,
            damping: 21,
            delay: 2.3,
          }}
        >
          {`THIS STAGGERS FROM CENTER`}
        </VerticalCutReveal>
        <VerticalCutReveal
          splitBy="characters"
          staggerDuration={0.05}
          staggerFrom={5}
          transition={{
            type: "spring",
            stiffness: 200,
            damping: 21,
            delay: 3.2,
          }}
        >
          {`THIS ONE FROM THE 5TH CHARACTER`}
        </VerticalCutReveal>
      </div>
    </div>
  )
}
```


Or you can use the `random` option, which will animate the elements in a random order. You can see the multiline text in action here too:


### Demo Code (vertical-cut-reveal-letter-random-demo.tsx)
```tsx
import VerticalCutReveal from "@/fancy/components/text/vertical-cut-reveal"

export default function Preview() {
  return (
    <div >
      <VerticalCutReveal
        splitBy="characters"
        staggerDuration={0.002}
        staggerFrom="random"
        transition={{
          type: "spring",
          stiffness: 200,
          damping: 35,
          delay: 0.1,
        }}
        containerClassName="text-[#00000] leading-snug"
      >
        {`“When a small, unassuming object exceeds our expectations, we are not only surprised but pleased. Our usual reaction is something like, "That little thing did all that?" Simplicity is about the unexpected pleasure derived from what is likely to be insignificant and would otherwise go unnoticed. The smaller the object, the more forgiving we can be when it misbehaves.”
        ― John Maeda,`}
      </VerticalCutReveal>
    </div>
  )
}
```


### No auto start

If you don't want the animation to start automatically, you can set the `autoStart` prop to `false`. In this case, you can call the `startAnimation` method exposed via a ref to start the animation manually. Here is an example demonstrating how to do this when the component is inside the viewport (with the `useInView` hook from framer motion):


### Demo Code (vertical-cut-reveal-scroll-demo.tsx)
```tsx
"use client"

import { useEffect, useRef } from "react"
import { useInView } from "motion/react"

import VerticalCutReveal, {
  VerticalCutRevealRef,
} from "@/fancy/components/text/vertical-cut-reveal"

export default function Preview() {
  const ref = useRef(null)
  const textRef = useRef<VerticalCutRevealRef>(null)
  const isInView = useInView(ref, { once: false })

  useEffect(() => {
    if (isInView) {
      textRef.current?.startAnimation()
    } else {
      textRef.current?.reset()
    }
  }, [isInView])

  return (
    <div >
      <div >
        Scroll down champ ↓
      </div>
      <div >
        <div ref={ref}>
          <VerticalCutReveal
            splitBy="characters"
            staggerDuration={0.02}
            staggerFrom="first"
            transition={{
              type: "spring",
              stiffness: 200,
              damping: 35,
              delay: 0.1,
            }}
            containerClassName="text-[#00000] leading-snug"
            ref={textRef}
            autoStart={false}
          >
            {`howdy! 👋`}
          </VerticalCutReveal>
        </div>
      </div>
    </div>
  )
}
```


## Notes

Since each element is "cutted" because of the `overflow-hidden` property, with some fonts and font-families (eg italic), parts of the letter may be cutoff. That's why you can use the `containerClassName` prop to style the container element, the `worldLeterLevelClassName` prop to style word level container, and the `elementLevelClassName` prop to style the individual split elements. You can add padding for example to accomodate more space for the text.

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `string` | - | The text to be displayed and animated |
| reverse | `boolean` | `true` | Direction of the animation (true: bottom to top, false: top to bottom) |
| transition | `AnimationOptions` | `{ type: "spring", damping: 30, stiffness: 300 }` | Animation configuration for each element. Refer to motion docs for more details |
| splitBy | `"words" | "characters" | "lines" | string` | `"words"` | The split method for the text |
| staggerDuration | `number` | `0.2` | Delay between each element's animation start |
| staggerFrom | `"first" | "last" | "center" | "random" | number` | `"first"` | Starting index of the animation |
| containerClassName | `string` | - | Additional CSS classes for styling the container |
| wordLevelClassName | `string` | - | Additional CSS classes for styling the word level container |
| elementLevelClassName | `string` | - | Additional CSS classes for styling the individual elements |
| onClick | `() => void` | - | Callback function for click events |
| onStart | `() => void` | - | Callback function for when the animation starts |
| onComplete | `() => void` | - | Callback function for when the animation completes |
| autoStart | `boolean` | `true` | Whether to start the animation automatically |


---

### Text Rotate <a name="text-rotate"></a>

### Demo Code (text-rotate-demo.tsx)
```tsx
"use client"

import { LayoutGroup, motion } from "motion/react"

import TextRotate from "@/fancy/components/text/text-rotate"

export default function Preview() {
  return (
    <div >
      <LayoutGroup>
        <motion.p  layout>
          <motion.span
            
            layout
            transition={{ type: "spring", damping: 30, stiffness: 400 }}
          >
            Make it{" "}
          </motion.span>
          <TextRotate
            texts={[
              "work!",
              "fancy ✽",
              "right",
              "fast",
              "fun",
              "rock",
              "🕶️🕶️🕶️",
            ]}
            mainClassName="text-white px-2 sm:px-2 md:px-3 bg-primary-red overflow-hidden py-0.5 sm:py-1 md:py-2 justify-center rounded-lg"
            staggerFrom={"last"}
            initial={{ y: "100%" }}
            animate={{ y: 0 }}
            exit={{ y: "-120%" }}
            staggerDuration={0.025}
            splitLevelClassName="overflow-hidden pb-0.5 sm:pb-1 md:pb-1"
            transition={{ type: "spring", damping: 30, stiffness: 400 }}
            rotationInterval={2000}
          />
        </motion.p>
      </LayoutGroup>
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/text-rotate
```


  



### Component Source Code (text-rotate.tsx)
```tsx
"use client"

import {
  ElementType,
  forwardRef,
  useCallback,
  useEffect,
  useImperativeHandle,
  useMemo,
  useState,
} from "react"
import {
  AnimatePresence,
  AnimatePresenceProps,
  motion,
  MotionProps,
  Transition,
} from "motion/react"

import { cn } from "@/lib/utils"

// handy function to split text into characters with support for unicode and emojis
const splitIntoCharacters = (text: string): string[] => {
  if (typeof Intl !== "undefined" && "Segmenter" in Intl) {
    const segmenter = new Intl.Segmenter("en", { granularity: "grapheme" })
    return Array.from(segmenter.segment(text), ({ segment }) => segment)
  }
  // Fallback for browsers that don't support Intl.Segmenter
  return Array.from(text)
}

interface TextRotateProps {
  /**
   * Array of text strings to rotate through.
   * Required prop with no default value.
   */
  texts: string[]

  /**
   * render as HTML Tag
   */
  as?: ElementType

  /**
   * Time in milliseconds between text rotations.
   * @default 2000
   */
  rotationInterval?: number

  /**
   * Initial animation state or array of states.
   * @default { y: "100%", opacity: 0 }
   */
  initial?: MotionProps["initial"] | MotionProps["initial"][]

  /**
   * Animation state to animate to or array of states.
   * @default { y: 0, opacity: 1 }
   */
  animate?: MotionProps["animate"] | MotionProps["animate"][]

  /**
   * Animation state when exiting or array of states.
   * @default { y: "-120%", opacity: 0 }
   */
  exit?: MotionProps["exit"] | MotionProps["exit"][]

  /**
   * AnimatePresence mode
   * @default "wait"
   */
  animatePresenceMode?: AnimatePresenceProps["mode"]

  /**
   * Whether to run initial animation on first render.
   * @default false
   */
  animatePresenceInitial?: boolean

  /**
   * Duration of stagger delay between elements in seconds.
   * @default 0
   */
  staggerDuration?: number

  /**
   * Direction to stagger animations from.
   * @default "first"
   */
  staggerFrom?: "first" | "last" | "center" | number | "random"

  /**
   * Animation transition configuration.
   * @default { type: "spring", damping: 25, stiffness: 300 }
   */
  transition?: Transition

  /**
   * Whether to loop through texts continuously.
   * @default true
   */
  loop?: boolean

  /**
   * Whether to auto-rotate texts.
   * @default true
   */
  auto?: boolean

  /**
   * How to split the text for animation.
   * @default "characters"
   */
  splitBy?: "words" | "characters" | "lines" | string

  /**
   * Callback function triggered when rotating to next text.
   * @default undefined
   */
  onNext?: (index: number) => void

  /**
   * Class name for the main container element.
   * @default undefined
   */
  mainClassName?: string

  /**
   * Class name for the split level wrapper elements.
   * @default undefined
   */
  splitLevelClassName?: string

  /**
   * Class name for individual animated elements.
   * @default undefined
   */
  elementLevelClassName?: string
}

/**
 * Interface for the ref object exposed by TextRotate component.
 * Provides methods to control text rotation programmatically.
 * This allows external components to trigger text changes
 * without relying on the automatic rotation.
 */
export interface TextRotateRef {
  /**
   * Advance to next text in sequence.
   * If at the end, will loop to beginning if loop prop is true.
   */
  next: () => void

  /**
   * Go back to previous text in sequence.
   * If at the start, will loop to end if loop prop is true.
   */
  previous: () => void

  /**
   * Jump to specific text by index.
   * Will clamp index between 0 and texts.length - 1.
   */
  jumpTo: (index: number) => void

  /**
   * Reset back to first text.
   * Equivalent to jumpTo(0).
   */
  reset: () => void
}

/**
 * Internal interface for representing words when splitting text by characters.
 * Used to maintain proper word spacing and line breaks while allowing
 * character-by-character animation. This prevents words from breaking
 * across lines during animation.
 */
interface WordObject {
  /**
   * Array of individual characters in the word.
   * Uses Intl.Segmenter when available for proper Unicode handling.
   */
  characters: string[]

  /**
   * Whether this word needs a space after it.
   * True for all words except the last one in a sequence.
   */
  needsSpace: boolean
}

const TextRotate = forwardRef<TextRotateRef, TextRotateProps>(
  (
    {
      texts,
      as = "p",
      transition = { type: "spring", damping: 25, stiffness: 300 },
      initial = { y: "100%", opacity: 0 },
      animate = { y: 0, opacity: 1 },
      exit = { y: "-120%", opacity: 0 },
      animatePresenceMode = "wait",
      animatePresenceInitial = false,
      rotationInterval = 2000,
      staggerDuration = 0,
      staggerFrom = "first",
      loop = true,
      auto = true,
      splitBy = "characters",
      onNext,
      mainClassName,
      splitLevelClassName,
      elementLevelClassName,
      ...props
    },
    ref
  ) => {
    const [currentTextIndex, setCurrentTextIndex] = useState(0)

    // Splitting the text into animation segments
    const elements = useMemo(() => {
      const currentText = texts[currentTextIndex]
      if (splitBy === "characters") {
        const text = currentText.split(" ")
        return text.map((word, i) => ({
          characters: splitIntoCharacters(word),
          needsSpace: i !== text.length - 1,
        }))
      }
      return splitBy === "words"
        ? currentText.split(" ")
        : splitBy === "lines"
          ? currentText.split("\n")
          : currentText.split(splitBy)
    }, [texts, currentTextIndex, splitBy])

    // Helper function to calculate stagger delay for each text segment
    const getStaggerDelay = useCallback(
      (index: number, totalChars: number) => {
        const total = totalChars
        if (staggerFrom === "first") return index * staggerDuration
        if (staggerFrom === "last") return (total - 1 - index) * staggerDuration
        if (staggerFrom === "center") {
          const center = Math.floor(total / 2)
          return Math.abs(center - index) * staggerDuration
        }
        if (staggerFrom === "random") {
          const randomIndex = Math.floor(Math.random() * total)
          return Math.abs(randomIndex - index) * staggerDuration
        }
        return Math.abs(staggerFrom - index) * staggerDuration
      },
      [staggerFrom, staggerDuration]
    )

    // Helper function to handle index changes and trigger callback
    const handleIndexChange = useCallback(
      (newIndex: number) => {
        setCurrentTextIndex(newIndex)
        onNext?.(newIndex)
      },
      [onNext]
    )

    // Go to next text
    const next = useCallback(() => {
      const nextIndex =
        currentTextIndex === texts.length - 1
          ? loop
            ? 0
            : currentTextIndex
          : currentTextIndex + 1

      if (nextIndex !== currentTextIndex) {
        handleIndexChange(nextIndex)
      }
    }, [currentTextIndex, texts.length, loop, handleIndexChange])

    // Go back to previous text
    const previous = useCallback(() => {
      const prevIndex =
        currentTextIndex === 0
          ? loop
            ? texts.length - 1
            : currentTextIndex
          : currentTextIndex - 1

      if (prevIndex !== currentTextIndex) {
        handleIndexChange(prevIndex)
      }
    }, [currentTextIndex, texts.length, loop, handleIndexChange])

    // Jump to specific text by index
    const jumpTo = useCallback(
      (index: number) => {
        const validIndex = Math.max(0, Math.min(index, texts.length - 1))
        if (validIndex !== currentTextIndex) {
          handleIndexChange(validIndex)
        }
      },
      [texts.length, currentTextIndex, handleIndexChange]
    )

    // Reset back to first text
    const reset = useCallback(() => {
      if (currentTextIndex !== 0) {
        handleIndexChange(0)
      }
    }, [currentTextIndex, handleIndexChange])

    // Get animation props for each text segment. If array is provided, states will be mapped to text segments cyclically.
    const getAnimationProps = useCallback(
      (index: number) => {
        const getProp = (
          prop:
            | MotionProps["initial"]
            | MotionProps["initial"][]
            | MotionProps["animate"]
            | MotionProps["animate"][]
            | MotionProps["exit"]
            | MotionProps["exit"][]
        ) => {
          if (Array.isArray(prop)) {
            return prop[index % prop.length]
          }
          return prop
        }

        return {
          initial: getProp(initial) as MotionProps["initial"],
          animate: getProp(animate) as MotionProps["animate"],
          exit: getProp(exit) as MotionProps["exit"],
        }
      },
      [initial, animate, exit]
    )

    // Expose all navigation functions via ref
    useImperativeHandle(
      ref,
      () => ({
        next,
        previous,
        jumpTo,
        reset,
      }),
      [next, previous, jumpTo, reset]
    )

    // Auto-rotate text
    useEffect(() => {
      if (!auto) return
      const intervalId = setInterval(next, rotationInterval)
      return () => clearInterval(intervalId)
    }, [next, rotationInterval, auto])

    // Custom motion component to render the text as a custom HTML tag provided via prop
    const MotionComponent = useMemo(() => motion.create(as ?? "p"), [as])

    return (
      <MotionComponent
        className={cn("flex flex-wrap whitespace-pre-wrap", mainClassName)}
        transition={transition}
        layout
        {...props}
      >
        {texts[currentTextIndex]}

        <AnimatePresence
          mode={animatePresenceMode}
          initial={animatePresenceInitial}
        >
          <motion.span
            key={currentTextIndex}
            className={cn(
              "flex flex-wrap",
              splitBy === "lines" && "flex-col w-full"
            )}
            aria-hidden
            layout
          >
            {(splitBy === "characters"
              ? (elements as WordObject[])
              : (elements as string[]).map((el, i) => ({
                  characters: [el],
                  needsSpace: i !== elements.length - 1,
                }))
            ).map((wordObj, wordIndex, array) => {
              const previousCharsCount = array
                .slice(0, wordIndex)
                .reduce((sum, word) => sum + word.characters.length, 0)

              return (
                
                  {wordObj.characters.map((char, charIndex) => {
                    const totalIndex = previousCharsCount + charIndex
                    const animationProps = getAnimationProps(totalIndex)
                    return (
                      
                        <motion.span
                          {...animationProps}
                          key={charIndex}
                          transition={{
                            ...transition,
                            delay: getStaggerDelay(
                              previousCharsCount + charIndex,
                              array.reduce(
                                (sum, word) => sum + word.characters.length,
                                0
                              )
                            ),
                          }}
                          className={"inline-block"}
                        >
                          {char}
                        </motion.span>
                      
                    )
                  })}
                  {wordObj.needsSpace && (
                     
                  )}
                
              )
            })}
          </motion.span>
        </AnimatePresence>
      </MotionComponent>
    )
  }
)

TextRotate.displayName = "TextRotate"

export default TextRotate
```





## Understanding the component

1. For the animation, we switch the actual rendered text from the `texts` array. Either automatically, if the `auto` prop is set to `true`, or we can do it manually, by calling the `next()` or `previous()` methods exposed via a ref.

2. For animating out the previous text, and animating in the next, we use the `AnimatePresence` component from `motion/react`. The `initial`, `animate` and `exit` props can be used to define the three states of the text. Refer to the [motion documentation](https://motion.dev/docs/react/docs/react-animate-presence) for more details.

3. The current text is split into smaller pieces based on the `splitBy` prop, which will determine how the text will be animated:

   - `words`: Splits into individual words (e.g., "Hello world" → ["Hello", "world"])
   - `characters`: Splits into individual characters (e.g., "Hi" → ["H", "i"])
   - `lines`: Splits by newline characters (`\n`)
   - `string`: Splits by any custom string delimiter

4. Each piece of text is wrapped in two `` elements: An outer `` that acts as a container for a word or line of text and an inner `` that holds the actual text. There are two reasons for this:

    1. When dealing with multi-line text, each line maintains its own reveal animation starting point. This means if you have text that spans multiple lines, each line will animate independently from its own baseline, rather than all elements animating from a single point (like the bottom of the entire paragraph).
    2. When using `characters` mode, characters from the same word stay together in a word container. This prevents unwanted line breaks in the middle of words - if a word needs to wrap to the next line, it will wrap as a complete unit rather than having some characters on one line and others on the next line. This maintains proper text flow and readability while still allowing character-by-character animation within each word.


## Examples

### Custom animation variations

You can customize the `animate`, `exit`, and `initial` props to create custom animation variations. For example, you can use the `filter` property to blur the text during the animation.


### Demo Code (text-rotate-custom-animation-demo.tsx)
```tsx
"use client"

import TextRotate from "@/fancy/components/text/text-rotate"

export default function Preview() {
  return (
    <div >
      <TextRotate
        texts={[
          "The problem isn't how to make the world more technological. It's about how to make the world more humane again.",
          "When you use other people's software you live in somebody else's dream.",
        ]}
        mainClassName=" md:leading-10 flex whitespace-pre text-lg sm:text-xl md:text-5xl max-w-xl text-center"
        staggerFrom={"random"}
        animatePresenceMode="wait"
        splitBy="characters"
        initial={[
          { filter: "blur(20px)", opacity: 0 },
        ]}
        animate={[
          { filter: "blur(0px)", opacity: 1 },
        ]}
        exit={[
          { filter: "blur(20px)", opacity: 0 },
        ]}
        loop
        staggerDuration={0.01}
        splitLevelClassName=""
        elementLevelClassName="md:py-[4px]"
        transition={{ ease: [0.909, 0.151, 0.153, 0.86], duration: 1 }}
        rotationInterval={4000}
      />
    </div>
  )
}
```


### SplitBy variations

With the `splitBy` prop, you can control how the text is split into smaller pieces. It can be either `words`, `characters`, `lines`, or a custom `string` delimiter. In case of `lines`, you are responsible for adding the `\n` delimiter yourself.

The following example demonstrates the `words` (the quote) and `characters` (the author) mode. It should respect multiline texts.


### Demo Code (text-rotate-multiline-demo.tsx)
```tsx
"use client"

import { LayoutGroup, motion } from "motion/react"

import TextRotate from "@/fancy/components/text/text-rotate"

export default function Preview() {
  return (
    <div >
      <LayoutGroup>
        <TextRotate
          texts={[
            "A typeface family is an accomplishment on the order of a novel, a feature film screenplay, a computer language design and implementation, a major musical composition, a monumental sculpture, or other artistic or technical endeavors that consume a year or more of intensive creative effort.",
            "Typography is two-dimensional architecture, based on experience and imagination, and guided by rules and readability. And this is the purpose of typography: The arrangement of design elements within a given structure should allow the reader to easily focus on the message, without slowing down the speed of his reading.",
          ]}
          staggerFrom={"first"}
          staggerDuration={0.01}
          initial={{ opacity: 0, x: 10 }}
          animate={{ opacity: 1, x: 0 }}
          exit={{ opacity: 0, x: -10 }}
          transition={{ type: "spring", damping: 30, stiffness: 400 }}
          rotationInterval={4000}
          splitBy="words"
        />
        <motion.div
          
          layout
        />
        <TextRotate
          texts={["Charles Bigelow", "Hermann Zapf"]}
          staggerFrom={"first"}
          staggerDuration={0.025}
          initial={{ opacity: 0, x: 10 }}
          animate={{ opacity: 1, x: 0 }}
          exit={{ opacity: 0, x: -10 }}
          transition={{ type: "spring", damping: 30, stiffness: 400 }}
          rotationInterval={4000}
          splitBy="characters"
        />
      </LayoutGroup>
    </div>
  )
}
```


### Stagger

With the `staggerFrom` prop, you can control the index of the letter/word/line where the stagger animation starts. Possible values are `"first"`, `"center"`, `"last"`, `"random"`, or a number.


### Demo Code (text-rotate-stagger-demo.tsx)
```tsx
"use client"

import { exampleImages } from "@/utils/demo-images"

import TextRotate from "@/fancy/components/text/text-rotate"

export default function Preview() {
  return (
    <div >
      <div >
        <img
          src={exampleImages[0].url}
          alt="city"
          
        />
      </div>
      <div >
        <div >
          <TextRotate
            texts={["New York", "Los Angeles", "Chicago", "Miami"]}
            mainClassName="justify-center"
            staggerFrom="first"
            initial={{ y: "100%" }}
            animate={{ y: 0 }}
            exit={{ y: "-120%" }}
            staggerDuration={0.04}
            splitLevelClassName="overflow-hidden"
            transition={{ type: "spring", damping: 30, stiffness: 400 }}
            rotationInterval={2500}
          />

          <TextRotate
            texts={["São Paulo", "Rio de Janeiro", "Salvador", "Brasília"]}
            mainClassName="justify-center"
            staggerFrom="center"
            initial={{ y: "100%" }}
            animate={{ y: 0 }}
            exit={{ y: "-120%" }}
            staggerDuration={0.04}
            splitLevelClassName="overflow-hidden"
            transition={{ type: "spring", damping: 30, stiffness: 400 }}
            rotationInterval={2500}
          />

          <TextRotate
            texts={["Tokyo", "Osaka", "Kyoto", "Sapporo"]}
            mainClassName="justify-center"
            staggerFrom="last"
            initial={{ y: "100%" }}
            animate={{ y: 0 }}
            exit={{ y: "-120%" }}
            staggerDuration={0.04}
            splitLevelClassName="overflow-hidden"
            transition={{ type: "spring", damping: 30, stiffness: 400 }}
            rotationInterval={2500}
          />

          <TextRotate
            texts={["Mumbai", "Delhi", "Bangalore", "Chennai"]}
            mainClassName="justify-center"
            staggerFrom="random"
            initial={{ y: "100%" }}
            animate={{ y: 0 }}
            exit={{ y: "-120%" }}
            staggerDuration={0.04}
            splitLevelClassName="overflow-hidden"
            transition={{ type: "spring", damping: 30, stiffness: 400 }}
            rotationInterval={2500}
          />
        </div>
      </div>
    </div>
  )
}
```


### Manual control

If the `auto` prop is set to `false`, you can manually control the animation by calling the `next()` or `previous()` methods exposed via a ref.


### Demo Code (text-rotate-step-demo.tsx)
```tsx
"use client"

import { useRef } from "react"
import { MoveLeft, MoveRight } from "lucide-react"
import { LayoutGroup, motion } from "motion/react"

import TextRotate, { TextRotateRef } from "@/fancy/components/text/text-rotate"

export default function Preview() {
  const textRotateRef = useRef<TextRotateRef>(null)

  return (
    <div >
      <LayoutGroup>
        <motion.p  layout>
          <TextRotate
            ref={textRotateRef}
            texts={[
              "this is the first text",
              "this is the 2nd",
              "we're at third!",
              "4th! keep going",
              "5th.",
              "this is the end.",
            ]}
            mainClassName="text-lg sm:text-2xl md:text-4xl justify-center flex"
            staggerFrom={"first"}
            animatePresenceMode="sync"
            loop={true}
            auto={false}
            staggerDuration={0.0}
            initial={{ opacity: 0, y: 0 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: 0 }}
            transition={{ type: "spring", damping: 30, stiffness: 400 }}
            rotationInterval={3000}
            splitBy="words"
          />
        </motion.p>
      </LayoutGroup>

      <div >
        <button
          onClick={() => textRotateRef.current?.previous()}
          
        >
          <MoveLeft  />
        </button>
        <button
          onClick={() => textRotateRef.current?.next()}
          
        >
          <MoveRight  />
        </button>
      </div>
    </div>
  )
}
```


This can be handy for a lot of use cases, eg. a scroll-triggered animation.


### Demo Code (text-rotate-scroll-step-demo.tsx)
```tsx
"use client"

import { useEffect, useRef } from "react"
import { exampleImages } from "@/utils/demo-images"
import { useInView } from "motion/react"

import TextRotate, { TextRotateRef } from "@/fancy/components/text/text-rotate"

function Item({
  index,
  image,
  link,
  onInView,
}: {
  index: number
  image: string
  link: string
  onInView: (inView: boolean) => void
}) {
  const ref = useRef<HTMLDivElement>(null)
  const isInView = useInView(ref, {
    margin: "-45% 0px -45% 0px",
  })

  useEffect(() => {
    onInView(isInView)
  }, [isInView, onInView])

  return (
    <section
      ref={ref}
      key={index + 1}
      
    >
      <div >
        <a href={link} target="_blank" rel="noreferrer">
          <img
            src={image}
            alt={`Example ${index + 2}`}
            
          />
        </a>
      </div>
    </section>
  )
}

export default function Preview() {
  const textRotateRef = useRef<TextRotateRef>(null)

  const handleInView = (index: number, inView: boolean) => {
    console.log(index, inView)
    if (inView && textRotateRef.current) {
      textRotateRef.current.jumpTo(index)
    }
  }

  return (
    <div >
      <div >
        <div >
          <TextRotate
            ref={textRotateRef}
            texts={[...exampleImages.map((image) => image.author)]}
            mainClassName="text-sm sm:text-3xl md:text-4xl w-full justify-center flex pt-2"
            splitLevelClassName="overflow-hidden pb-2"
            staggerFrom={"first"}
            animatePresenceMode="wait"
            loop={false}
            auto={false}
            staggerDuration={0.005}
            initial={{ opacity: 0, y: 50 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -50 }}
            transition={{ type: "spring", duration: 0.6, bounce: 0 }}
          />
        </div>
      </div>
      <div >
        {exampleImages.slice(1).map((image, index) => (
          <Item
            key={index}
            index={index}
            image={image.url}
            link={image.link}
            onInView={(inView) => handleInView(index, inView)}
          />
        ))}
      </div>
    </div>
  )
}
```


### Different animation per segment

For the `animate`, `exit`, and `initial` props, you can either pass one single object, or an array of objects. This allows you to map different animations to each segment of the text. If there are more segments than animations, we will cycle through the animations.


### Demo Code (text-rotate-mapping-demo.tsx)
```tsx
"use client"

import TextRotate from "@/fancy/components/text/text-rotate"

export default function Preview() {
  return (
    <div >
      <TextRotate
        texts={[
          "The problem isn't how to make the world more technological. It's about how to make the world more humane again.",
          "The problem isn't how to make the world more technological. It's about how to make the world more humane again.",
        ]}
        mainClassName="overflow-hidden md:leading-10 flex whitespace-pre text-lg sm:text-xl md:text-5xl max-w-xl text-center"
        staggerFrom={"random"}
        animatePresenceMode="wait"
        splitBy="characters"
        initial={[{ x: "120%" }, { y: "120%" }, { x: "-120%" }, { y: "-120%" }]}
        animate={[{ x: 0 }, { y: 0 }, { x: 0 }, { y: 0 }]}
        exit={[{ x: "-120%" }, { y: "-120%" }, { x: "120%" }, { y: "120%" }]}
        loop
        staggerDuration={0.01}
        splitLevelClassName="overflow-hidden"
        elementLevelClassName="overflow-hidden md:py-[4px]"
        transition={{ ease: [0.909, 0.151, 0.153, 0.86], duration: 1 }}
        rotationInterval={4000}
      />
    </div>
  )
}
```


## Notes

If you're using `auto` mode, make sure that the `rotationInterval` prop is set to a value that's greater than the duration of initial/exit animations, otherwise we will switch to a new text before the animation is complete. 

## Props

### TextRotateProps


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| texts* | `string[]` | - | The text to be displayed and animated |
| as | `ElementType` | `p` | Render as HTML Tag |
| initial | `MotionProps["initial"] | MotionProps["initial"][]` | `{ y: "100%", opacity: 0 }` | Initial animation state or array of states. If array is provided, states will be mapped to text segments cyclically. |
| animate | `MotionProps["animate"] | MotionProps["animate"][]` | `{ y: 0, opacity: 1 }` | Animation state to animate to or array of states. If array is provided, states will be mapped to text segments cyclically. |
| exit | `MotionProps["exit"] | MotionProps["exit"][]` | `{ y: "-120%", opacity: 0 }` | Animation state when exiting or array of states. If array is provided, states will be mapped to text segments cyclically. |
| animatePresenceMode | `AnimatePresenceProps["mode"]` | `"wait"` | The mode for the AnimatePresence component. Refer to motion docs for more details |
| animatePresenceInitial | `boolean` | `false` | Whether to animate in the initial state for AnimatePresence. Refer to motion docs for more details |
| rotationInterval | `number` | `2000` | The interval in milliseconds between each rotation |
| transition | `ValueAnimationTransition` | `{ type: "spring", damping: 25, stiffness: 300 }` | Animation configuration for each letter. Refer to motion docs for more details |
| staggerDuration | `number` | `0` | Delay between each letter's animation start |
| staggerFrom | `"first" | "last" | "center" | "random" | number` | `"first"` | Starting index of the stagger effect |
| loop | `boolean` | `true` | Whether to loop through the texts |
| auto | `boolean` | `true` | Whether to start the animation automatically |
| splitBy | `"words" | "characters" | "lines" | string` | `"words"` | The split method for the text |
| onNext | `(index: number) => void` | - | Callback function for when the next text is rendered |
| mainClassName | `string` | - | Additional CSS classes for styling the container |
| splitLevelClassName | `string` | - | Additional CSS classes for styling the individual words or lines |
| elementLevelClassName | `string` | - | Additional CSS classes for styling the individual characters/words/lines |


### TextRotateRef


| Method | Description |
| --- | --- |
| next() | Goes to the next text |
| previous() | Goes back to the previous text |
| jumpTo(index: number) | Jumps to a specific text index |
| reset() | Resets the animation to the initial state |


---

### Variable Font Hover By Letter <a name="variable-font-hover-by-letter"></a>

### Demo Code (variable-font-hover-by-letter-demo.tsx)
```tsx
import VariableFontHoverByLetter from "@/fancy/components/text/variable-font-hover-by-letter"

export default function Preview() {
  return (
    <div >
      <div >
        <div >
          <h2>OPEN ROLES ✽</h2>
          <ul >
            <VariableFontHoverByLetter
              label="DESIGN ENGINEER (US)"
              staggerDuration={0.03}
              fromFontVariationSettings="'wght' 400, 'slnt' 0"
              toFontVariationSettings="'wght' 900, 'slnt' -10"
            />
            <VariableFontHoverByLetter
              label="PRODUCT DESIGNER (US/UK)"
              staggerDuration={0.0}
              transition={{ duration: 1, type: "spring" }}
              fromFontVariationSettings="'wght' 400, 'slnt' -10"
              toFontVariationSettings="'wght' 900, 'slnt' -10"
            />
            <VariableFontHoverByLetter
              label="ENGINEERING MANAGER (US)"
              fromFontVariationSettings="'wght' 400, 'slnt' 0"
              toFontVariationSettings="'wght' 900, 'slnt' -10"
              staggerFrom={"last"}
            />
            <VariableFontHoverByLetter
              label="SALES ENGINEER (US)"
              staggerFrom={"center"}
              fromFontVariationSettings="'wght' 400, 'slnt' 0"
              toFontVariationSettings="'wght' 900, 'slnt' -10"
            />
          </ul>
        </div>
      </div>
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/variable-font-hover-by-letter
```






Install the following dependencies:


```bash
# Install command:
lodash
```


lodash is used for debouncing here — so the animation doesn't break on rapid hover changes.

Then, copy and paste the following code into your project:


### Component Source Code (variable-font-hover-by-letter.tsx)
```tsx
"use client"

import { useState } from "react"
import { debounce } from "lodash"
import { AnimationOptions, motion, stagger, useAnimate } from "motion/react"

interface TextProps {
  label: string
  fromFontVariationSettings: string
  toFontVariationSettings: string
  transition?: AnimationOptions
  staggerDuration?: number
  staggerFrom?: "first" | "last" | "center" | number
  className?: string
  onClick?: () => void
}

const VariableFontHoverByLetter = ({
  label,
  fromFontVariationSettings = "'wght' 400, 'slnt' 0",
  toFontVariationSettings = "'wght' 900, 'slnt' -10",
  transition = {
    type: "spring",
    duration: 0.7,
  },
  staggerDuration = 0.03,
  staggerFrom = "first",
  className,
  onClick,
  ...props
}: TextProps) => {
  const [scope, animate] = useAnimate()
  const [isHovered, setIsHovered] = useState(false)

  const mergeTransition = (baseTransition: AnimationOptions) => ({
    ...baseTransition,
    delay: stagger(staggerDuration, {
      from: staggerFrom,
    }),
  })

  const hoverStart = debounce(
    () => {
      if (isHovered) return
      setIsHovered(true)

      animate(
        ".letter",
        { fontVariationSettings: toFontVariationSettings },
        mergeTransition(transition)
      )
    },
    100,
    { leading: true, trailing: true }
  )

  const hoverEnd = debounce(
    () => {
      setIsHovered(false)

      animate(
        ".letter",
        { fontVariationSettings: fromFontVariationSettings },
        mergeTransition(transition)
      )
    },
    100,
    { leading: true, trailing: true }
  )

  return (
    <motion.span
      className={`${className}`}
      onHoverStart={hoverStart}
      onHoverEnd={hoverEnd}
      onClick={onClick}
      ref={scope}
      {...props}
    >
      {label}

      {label.split("").map((letter: string, i: number) => {
        return (
          <motion.span
            key={i}
            
            aria-hidden="true"
          >
            {letter}
          </motion.span>
        )
      })}
    </motion.span>
  )
}

export default VariableFontHoverByLetter
```





## Understanding Variable Fonts

This component is designed to work exclusively with variable fonts. Variable fonts are a modern font technology that allows a single font file to contain multiple variations of a typeface — these variations can be adjusted along different axes, such as weight, width, or slant, etc.

Each axis in a variable font has a minimum and maximum value, and you can interpolate between these values to create custom styles. Common axes include:

- Weight (`wght`): Controls the thickness of the letterforms (usually ranges from 100 to 900)
- Width (`wdth`): Controls the width of the letterforms
- Slant (`slnt`): Changes the angle of the letterforms
- Italic (`ital`): Also controls the slant of the letterforms
- Optical Size (`opsz`): Controls the size of the letterforms

These 5 axes are actually standardized, so when a font have them, they will have the names above. But, a font can have limitless custom axes too, with completely arbitrary names.

In this component, the `fromFontVariationSettings` and `toFontVariationSettings` props define the starting and ending states of the font variation. For example, using the [Overused Grotesk](https://github.com/RandomMaerks/Overused-Grotesk) font (which we use in this demo), we can modify the 'slnt' (slant) and 'wght' (weight) axes:

- The `slnt` axis ranges from 0 (up) to -10 (upright)
- The `wght` axis ranges from 100 (thin) to 900 (black)

An example prop therefore would be:

<CodeSnippet>
```jsx
`fromFontVariationSettings="'wght' 100, 'slnt' 0"`
```
</CodeSnippet>

**Always check the font's documentation to see what are the available axes and their ranges.**

Important to mention that older browser versions and various environments don't support variable fonts, so make sure to check compatibility.

## Resources

I highly recommend to go down the rabbit hole, it's super fun :)

- [MDN Web Docs for Variable fonts](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_fonts/Variable_fonts_guide)
- [Super cool article by ABC Dinamo](https://abcdinamo.com/news/using-variable-fonts-on-the-web)

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| label* | `string` | - | The text to be displayed and animated |
| fromFontVariationSettings* | `string` | - | Initial font variation settings |
| toFontVariationSettings* | `string` | - | Target font variation settings on hover |
| className | `string` | - | Additional CSS classes for styling |
| transition | `AnimationOptions` | `{ type: "spring", duration: 0.7 }` | Transition settings for the animation. Refer to motion docs for more details |
| staggerDuration | `number` | `0.03` | Delay between each letter's animation start |
| staggerFrom | `"first" | "last" | "center" | number` | `"first"` | Starting point of the stagger effect. Number is the index of the letter where the stagger animation starts |
| onClick | `() => void` | - | Callback function for click events |


---

### Variable Font Hover By Random Letter <a name="variable-font-hover-by-random-letter"></a>

### Demo Code (variable-font-hover-by-random-letter-demo.tsx)
```tsx
import VariableFontHoverByRandomLetter from "@/fancy/components/text/variable-font-hover-by-random-letter"

export default function Preview() {
  return (
    <div >
      <div >
        <VariableFontHoverByRandomLetter
          label="Let's Go!"
          staggerDuration={0.03}
          
          fromFontVariationSettings="'wght' 400, 'slnt' 0"
          toFontVariationSettings="'wght' 900, 'slnt' 0"
        />
      </div>
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/variable-font-hover-by-random-letter
```


  



### Component Source Code (variable-font-hover-by-random-letter.tsx)
```tsx
"use client"

import { useMemo } from "react"
import { motion, Transition } from "motion/react"

// Function to shuffle an array
function shuffleArray(array: number[]) {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
    ;[array[i], array[j]] = [array[j], array[i]]
  }
}

interface TextProps {
  label: string
  fromFontVariationSettings: string
  toFontVariationSettings: string
  transition?: Transition
  staggerDuration?: number
  className?: string
  onClick?: () => void
}

const VariableFontHoverByRandomLetter = ({
  label,
  fromFontVariationSettings = "'wght' 400, 'slnt' 0",
  toFontVariationSettings = "'wght' 900, 'slnt' -10",
  transition = {
    type: "spring",
    duration: 0.7,
  },
  staggerDuration = 0.03,
  className,
  onClick,
  ...props
}: TextProps) => {
  const shuffledIndices = useMemo(() => {
    const indices = Array.from({ length: label.length }, (_, i) => i)
    shuffleArray(indices)
    return indices
  }, [label])

  const letterVariants = {
    hover: (index: number) => ({
      fontVariationSettings: toFontVariationSettings,
      transition: {
        ...transition,
        delay: staggerDuration * index,
      },
    }),
    initial: (index: number) => ({
      fontVariationSettings: fromFontVariationSettings,
      transition: {
        ...transition,
        delay: staggerDuration * index,
      },
    }),
  }

  return (
    <motion.span
      className={`${className}`}
      onClick={onClick}
      whileHover="hover"
      initial="initial"
      {...props}
    >
      {label}

      {label.split("").map((letter: string, i: number) => {
        const index = shuffledIndices[i]
        return (
          <motion.span
            key={i}
            
            aria-hidden="true"
            variants={letterVariants}
            custom={index}
          >
            {letter}
          </motion.span>
        )
      })}
    </motion.span>
  )
}

export default VariableFontHoverByRandomLetter
```





## Understanding Variable Fonts

This component is designed to work exclusively with variable fonts. Variable fonts are a modern font technology that allows a single font file to contain multiple variations of a typeface — these variations can be adjusted along different axes, such as weight, width, or slant, etc.

Each axis in a variable font has a minimum and maximum value, and you can interpolate between these values to create custom styles. Common axes include:

- Weight (`wght`): Controls the thickness of the letterforms (usually ranges from 100 to 900)
- Width (`wdth`): Controls the width of the letterforms
- Slant (`slnt`): Changes the angle of the letterforms
- Italic (`ital`): Also controls the slant of the letterforms
- Optical Size (`opsz`): Controls the size of the letterforms

These 5 axes are actually standardized, so when a font have them, they will have the names above. But, a font can have limitless custom axes too, with completely arbitrary names.

In this component, the `fromFontVariationSettings` and `toFontVariationSettings` props define the starting and ending states of the font variation. For example, using the [Overused Grotesk](https://github.com/RandomMaerks/Overused-Grotesk) font (which we use in this demo), we can modify the 'slnt' (slant) and 'wght' (weight) axes:

- The `slnt` axis ranges from 0 (up) to -10 (upright)
- The `wght` axis ranges from 100 (thin) to 900 (black)

An example prop therefore would be:


**File**: `Font variation example prop`
```jsx
  fromFontVariationSettings="'wght' 100, 'slnt' 0"
```


**Always check the font's documentation to see what are the available axes and their ranges.**

Important to mention that older browsers versions and various environemnts don't support variable fonts, so make sure to check compatibility.

## Resources

I highly recommend to go down the rabbit hole, it's super fun :)

- [MDN Web Docs for Variable fonts](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_fonts/Variable_fonts_guide)
- [Super cool article by ABC Dinamo](https://abcdinamo.com/news/using-variable-fonts-on-the-web)

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| label* | `string` | - | The text to be displayed and animated |
| fromFontVariationSettings* | `string` | - | Initial font variation settings |
| toFontVariationSettings* | `string` | - | Target font variation settings on hover |
| className | `string` | - | Additional CSS classes for styling |
| transition | `Transition` | `{ type: "spring", duration: 0.7 }` | Transition settings for the animation. Refer to motion docs for more details |
| staggerDuration | `number` | `0.03` | Delay between each letter's animation start |
| onClick | `() => void` | - | Callback function for click events |


---

### Scroll And Swap <a name="scroll-and-swap"></a>

### Demo Code (scroll-and-swap-text-demo.tsx)
```tsx
"use client"

import { useEffect, useRef } from "react"
import Lenis from "lenis"

import ScrollAndSwapText from "@/fancy/components/text/scroll-and-swap-text"

// Generate an array of imaginary names
const names = [
  "Alexandra Rodriguez",
  "Benjamin Chen",
  "Catherine Williams",
  "David Martinez",
  "Elena Petrov",
  "Francesco Rossi",
  "Gabriela Santos",
  "Henrik Larsson",
  "Isabella Thompson",
  "James Anderson",
  "Katarina Novak",
  "Leonardo Silva",
  "Maria Gonzalez",
  "Nikolai Volkov",
  "Olivia Johnson",
  "Pablo Hernandez",
  "Qiana Washington",
  "Ricardo Lopez",
  "Sophia Kim",
  "Thomas Mueller",
  "Ursula Schmidt",
  "Viktor Petersen",
  "Wen Li",
  "Xavier Dubois",
  "Yasmin Hassan",
  "Zachary Brown",
  "Amelia Davis",
  "Bruno Costa",
  "Clara Johansson",
  "Diego Morales",
  "Evelyn Taylor",
  "Felix Wagner",
  "Grace Wilson",
  "Hugo Andersen",
  "Iris Nakamura",
  "Julian Beck",
  "Kira Popovic",
  "Lucas Garcia",
  "Maya Patel",
  "Nathan Clark",
  "Ophelia Martin",
  "Pietro Romano",
  "Quinn O'Brien",
  "Rosa Fernandez",
  "Sebastian Lee",
  "Tara Mitchell",
  "Ulrich Weber",
  "Valentina Rosso",
  "William Jones",
  "Xiomara Reyes",
  "Yuki Tanaka",
  "Zara Ahmed",
  "Andre Leclerc",
  "Beatrice Hall",
  "Carlos Mendoza",
  "Delphine Moreau",
  "Emilio Bianchi",
  "Fiona Murphy",
  "Giovanni Conti",
  "Helena Svensson",
  "Ivan Dimitrov",
  "Jasmine Green",
  "Kai Nielsen",
  "Luna Torres",
  "Marco Esposito",
  "Nadia Kozlov",
  "Oscar Lindberg",
  "Penelope White",
  "Quincy Adams",
  "Rafael Vargas",
  "Stella Jackson",
  "Theo Van Der Berg",
  "Uma Sharma",
  "Vincenzo Ferrari",
  "Willow Parker",
  "Ximena Castillo",
  "Yolanda King",
  "Zander Cooper",
  "Aria Blackwood",
  "Bastien Dubois",
  "Camille Laurent",
  "Dante Ricci",
  "Estelle Moreau",
  "Fabio Santos",
  "Gemma Wright",
  "Hector Vega",
  "Ingrid Hansen",
  "Javier Ruiz",
  "Kaia Storm",
  "Liam O'Connor",
  "Mila Petrov",
  "Noah Fischer",
  "Octavia Bell",
  "Phoenix Rivera",
  "Quentin Gray",
  "Ruby Anderson",
  "Sage Thompson",
  "Tobias Klein",
  "Unity Cross",
  "Vera Kozlova",
  "Wade Turner",
  "Xara Moon",
  "York Sterling",
  "Zoe Martinez",
  "Atlas Kane",
  "Brielle Fox",
  "Caspian Reed",
  "Dara Singh",
  "Eden Blake",
  "Falcon Knight",
  "Gaia Stone",
  "Harbor Wells",
  "Indigo Vale",
  "Juno Pierce",
  "Knox Rivers",
]

export default function Preview() {
  const containerRef = useRef<HTMLDivElement>(null)

  useEffect(() => {
    if (!containerRef.current) return

    const lenis = new Lenis({
      autoRaf: true,
      wrapper: containerRef.current,
      duration: 3,
      orientation: "vertical",
      gestureOrientation: "vertical",
      smoothWheel: true,
      touchMultiplier: 2,
    })

    return () => {
      lenis.destroy()
    }
  }, [])

  return (
    <div
      
      ref={containerRef}
    >
      <div >
        <p >
          SCROLL SLOWLY
        </p>
        <div >
          {names.map((name, index) => (
            <ScrollAndSwapText
              key={index}
              offset={[`0 0.2`, `0 0.8`]}
              
              containerRef={containerRef}
            >
              {name}
            </ScrollAndSwapText>
          ))}
        </div>
      </div>
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/scroll-and-swap-text
```






### Component Source Code (scroll-and-swap-text.tsx)
```tsx
"use client"

import React, { ElementType, useMemo, useRef } from "react"
import { motion, useScroll, useTransform, useSpring } from "motion/react"
import { cn } from "@/lib/utils"

// handy function to extract text from children
const extractTextFromChildren = (children: React.ReactNode): string | undefined => {
  // Handle null/undefined
  if (children == null) return ""

  // Handle string
  if (typeof children === "string") return children

  // Handle number
  if (typeof children === "number") return String(children)

  // Handle arrays (including fragments)
  if (Array.isArray(children)) {
    return children.map(extractTextFromChildren).join("")
  }

  // Handle React elements
  if (React.isValidElement(children)) {
    const props = (children as React.ReactElement).props
    const childText = (props as any).children as React.ReactNode

    // Recursively extract text from children
    if (childText != null) {
      return extractTextFromChildren(childText)
    }

    return ""
  }
}

interface ScrollAndSwapTextProps {
  /**
   * The content to be displayed and animated
   */
  children: React.ReactNode

  /**
   * HTML Tag to render the component as
   * @default "span"
   */
  as?: ElementType

  /**
   * Reference to the container element for scroll tracking
   */
  containerRef: React.RefObject<HTMLElement | null>

  /**
   * Offset configuration for when the animation should start and end relative to the scroll container. Check motion documentation for more details.
   * @default ["0 0", "0 1"]
   */
  offset?: [string, string]

  /**
   * Additional CSS classes for styling the component
   */
  className?: string

  /**
   * Spring animation configuration for smoothing the scroll-based animation
   * @default { stiffness: 200, damping: 30 }
   */
  springConfig?: {
    stiffness?: number
    damping?: number
    mass?: number
  }
}

/**
 * ScrollAndSwapText creates a scroll-triggered text animation where text slides vertically
 * based on scroll progress.
 */
const ScrollAndSwapText = ({
  children,
  as = "span",
  offset = ["0 0", "0 1"],
  className,
  containerRef,
  springConfig = { stiffness: 200, damping: 30 },
  ...props
}: ScrollAndSwapTextProps) => {
  const ref = useRef<HTMLElement>(null)

  // Convert children to string for processing with error handling
  const text = useMemo(() => {
    try {
      return extractTextFromChildren(children)
    } catch (error) {
      console.error(error)
      return ""
    }
  }, [children])

  // Track scroll progress within the specified container and target element
  const { scrollYProgress } = useScroll({
    container: containerRef,
    target: ref,
    offset: offset as any, // framer motion doesnt export the type, so we have to cast it, sorry :/
  })

  // Apply spring physics to smooth the scroll-based animation
  const springScrollYProgress = useSpring(scrollYProgress, springConfig)

  // Transform scroll progress into vertical translation values
  // Original text moves from 0% to -100% (slides up and out)
  const top = useTransform(springScrollYProgress, [0, 1], ["0%", "-100%"])
  // Replacement text moves from 100% to 0% (slides up from below)
  const bottom = useTransform(springScrollYProgress, [0, 1], ["100%", "0%"])

  const ElementTag = as

  return (
    <ElementTag
      className={cn("flex overflow-hidden relative items-center justify-center p-0", className)}
      ref={ref}
      {...props}
    >

      
        {text}
      
      
      <motion.span  style={{ top: top }}>
        {text}
      </motion.span>
      
      <motion.span
        
        style={{ top: bottom }}
        aria-hidden="true"
      >
        {text}
      </motion.span>
    </ElementTag>
  )
}

ScrollAndSwapText.displayName = "ScrollAndSwapText"

export default ScrollAndSwapText
```





## Understanding the component

The trick here is similar to the [Letter Swap Hover](/docs/components/text/letter-swap-hover) component—duplicate the text, then wrapping the them in a container with `relative` position, then stack the elements vertically. We use `useScroll` hook from motion to track the scroll position of the container, and use the `scrollYProgress` value to offset the vertical position of the elements (by setting the `y` property of the element).

## Notes

- In order to achieve a nice effect, you likely have to play with the container (where to track the scroll) and its offset. Please refer to motion's [documentation](https://www.framer.com/motion/use-scroll/) for more details.

- Make sure that the container has a non-static position, like `relative`, `fixed`, or `absolute` to ensure scroll offset is calculated correctly.

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The content to be displayed and animated |
| as | `ElementType` | `"span"` | HTML Tag to render the component as |
| containerRef* | `React.RefObject` | - | Reference to the container element for scroll tracking |
| offset | `[string, string]` | `["0 0", "0 1"]` | Offset configuration for when the animation should start and end relative to the scroll container |
| className | `string` | - | Additional CSS classes for styling the component |
| springConfig | `{ stiffness?: number, damping?: number, mass?: number }` | `{ stiffness: 200, damping: 30 }` | Spring animation configuration for smoothing the scroll-based animation |


---

### Text Cursor Proximity <a name="text-cursor-proximity"></a>

### Demo Code (text-cursor-proximity-demo.tsx)
```tsx
"use client"

import { useRef } from "react"

import TextCursorProximity from "@/fancy/components/text/text-cursor-proximity"

const styles = {
  title: {
    filter: {
      from: "blur(0px)",
      to: "blur(8px)",
    }
  },
  details: {
    filter: {
      from: "blur(0px)", 
      to: "blur(4px)",
    }
  }
}

export default function Preview() {
  const containerRef = useRef<HTMLDivElement>(null)

  return (
    <div
      
      ref={containerRef}
    >
      <div >
        <div >
          <TextCursorProximity
            
            styles={styles.title}
            falloff="gaussian"
            radius={100}
            containerRef={containerRef}
          >
            DIGITAL
          </TextCursorProximity>
          <TextCursorProximity
            
            styles={styles.title}
            falloff="gaussian"
            radius={100}
            containerRef={containerRef}
          >
            WORKSHOP
          </TextCursorProximity>
        </div>

        <div >
          <div >
            <TextCursorProximity
              
              styles={styles.details}
              falloff="exponential"
              radius={70}
              containerRef={containerRef}
            >
              LONDON, UK ⟡ 18:30 GMT
            </TextCursorProximity>

            <TextCursorProximity
              
              styles={styles.details}
              falloff="exponential"
              radius={70}
              containerRef={containerRef}
            >
              123 DIGITAL STREET, EC1A 1BB ⟶
            </TextCursorProximity>

            <TextCursorProximity
              
              styles={styles.details}
              falloff="exponential"
              radius={70}
              containerRef={containerRef}
            >
              +44 20 7123 4567 ⟨⟩ INFO@DIGITAL.WORK
            </TextCursorProximity>

            <TextCursorProximity
              
              styles={styles.details}
              falloff="exponential"
              radius={70}
              containerRef={containerRef}
            >
              @DIGITALWORKSHOP * DIGITAL.WORK®
            </TextCursorProximity>

            <TextCursorProximity
              
              styles={styles.details}
              falloff="exponential"
              radius={70}
              containerRef={containerRef}
            >
              RSVP REQUIRED ⌲ LIMITED SEATS
            </TextCursorProximity>
          </div>
        </div>
      </div>
    </div>
  )
}
```


## Installation 




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/text-cursor-proximity
```






Create a hook for querying the cursor/mouse position.


*Component Source: use-mouse-position-ref (Source file not found)*


This hook actually returns a ref to the position (instead of a state), so we can avoid re-renders when the cursor moves.

Then, copy and paste the following code into your project:


### Component Source Code (text-cursor-proximity.tsx)
```tsx
"use client"

import React, { CSSProperties, ElementType, forwardRef, useRef, useMemo } from "react"
import {
  motion,
  useAnimationFrame,
  useMotionValue,
  useTransform,
} from "motion/react"

import { useMousePositionRef } from "@/hooks/use-mouse-position-ref"
import { cn } from "@/lib/utils"

// Helper type that makes all properties of CSSProperties accept number | string
type CSSPropertiesWithValues = {
  [K in keyof CSSProperties]: string | number
}

interface StyleValue<T extends keyof CSSPropertiesWithValues> {
  from: CSSPropertiesWithValues[T]
  to: CSSPropertiesWithValues[T]
}

interface TextProps extends React.HTMLAttributes<HTMLSpanElement> {
  /**
   * The content to be displayed and animated
   */
  children: React.ReactNode

  /**
   * HTML Tag to render the component as
   * @default span
   */
  as?: ElementType

  /**
   * Object containing style properties to animate
   * Each property should have 'from' and 'to' values
   */
  styles: Partial<{
    [K in keyof CSSPropertiesWithValues]: StyleValue<K>
  }>

  /**
   * Reference to the container element for mouse position calculations
   */
  containerRef: React.RefObject<HTMLDivElement | null>

  /**
   * Radius of the proximity effect in pixels
   * @default 50
   */
  radius?: number

  /**
   * Type of falloff function to use for the proximity effect
   * @default "linear"
   */
  falloff?: "linear" | "exponential" | "gaussian"
}

const TextCursorProximity = forwardRef<HTMLSpanElement, TextProps>(
  (
    {
      children,
      as,
      styles,
      containerRef,
      radius = 50,
      falloff = "linear",
      className,
      ...props
    },
    ref
  ) => {
    const MotionComponent = useMemo(() => motion.create(as ?? "span"), [as])
    const letterRefs = useRef<(HTMLSpanElement | null)[]>([])
    const mousePositionRef = useMousePositionRef(containerRef)

    // Convert children to string for letter processing
    const text = React.Children.toArray(children).join("")

    // Create a motion value for each letter's proximity
    const letterProximities = useRef(
      Array(text.replace(/\s/g, "").length)
        .fill(0)
        .map(() => useMotionValue(0))
    )

    const calculateDistance = (
      x1: number,
      y1: number,
      x2: number,
      y2: number
    ): number => {
      return Math.sqrt(Math.pow(x2 - x1, 2) + Math.pow(y2 - y1, 2))
    }

    const calculateFalloff = (distance: number): number => {
      const normalizedDistance = Math.min(Math.max(1 - distance / radius, 0), 1)

      switch (falloff) {
        case "exponential":
          return Math.pow(normalizedDistance, 2)
        case "gaussian":
          return Math.exp(-Math.pow(distance / (radius / 2), 2) / 2)
        case "linear":
        default:
          return normalizedDistance
      }
    }

    useAnimationFrame(() => {
      if (!containerRef.current) return
      const containerRect = containerRef.current.getBoundingClientRect()

      letterRefs.current.forEach((letterRef, index) => {
        if (!letterRef) return

        const rect = letterRef.getBoundingClientRect()
        const letterCenterX = rect.left + rect.width / 2 - containerRect.left
        const letterCenterY = rect.top + rect.height / 2 - containerRect.top

        const distance = calculateDistance(
          mousePositionRef.current.x,
          mousePositionRef.current.y,
          letterCenterX,
          letterCenterY
        )

        const proximity = calculateFalloff(distance)
        letterProximities.current[index].set(proximity)
      })
    })

    const words = text.split(" ")
    let letterIndex = 0

    return (
      <MotionComponent
        ref={ref}
        className={cn("", className)}
        {...props}
      >
        {words.map((word, wordIndex) => (
          
            {word.split("").map((letter) => {
              const currentLetterIndex = letterIndex++
              const proximity = letterProximities.current[currentLetterIndex]

              // Create transformed values for each style property
              const transformedStyles = Object.entries(styles).reduce(
                (acc, [key, value]) => {
                  acc[key] = useTransform(
                    proximity,
                    [0, 1],
                    [value.from, value.to]
                  )
                  return acc
                },
                {} as Record<string, any>
              )

              return (
                <motion.span
                  key={currentLetterIndex}
                  ref={(el: HTMLSpanElement | null) => {
                    letterRefs.current[currentLetterIndex] = el
                  }}
                  
                  aria-hidden="true"
                  style={transformedStyles}
                >
                  {letter}
                </motion.span>
              )
            })}
            {wordIndex < words.length - 1 && (
              &nbsp;
            )}
          
        ))}
        {text}
      </MotionComponent>
    )
  }
)

TextCursorProximity.displayName = "TextCursorProximity"
export default TextCursorProximity
```





## Understanding the component

The `TextCursorProximity` splits its text into letters that respond to cursor movement by adjusting their CSS properties based on the distance between the letter and cursor position.

1. Splitting text into individual letters
2. Tracking cursor position relative to each letter
3. Smoothly transitioning CSS values with motion's `useTransform` hook
4. Supporting multiple falloff patterns for the effect

### How it works

The component calculates the distance between the cursor and each letter in real-time. When the cursor comes within the specified `radius` of a letter, that letter's CSS properties (like scale, color, etc.) smoothly interpolate between two states. For this, we use the `motion` library's `useTransform` hook, which maps the CSS properties from the `styles.*.from` state to the `styles.*.to` state based on the proximity value (which ranges from 0 to 1).

- Default state: (defined in `styles.*.from`)
- Target state (defined in `styles.*.to`)

You can interpolate any value that [motion supports](https://motion.dev/docs/react-animation#animatable-values) (which is actually any CSS value, even those that can't be animated by the browser, like `mask-image`).

The closer the cursor gets to a letter, the closer that letter moves toward its target state.

## Examples

### Falloff

With the `falloff` prop, you can control the type of falloff. It can be either `linear`, `exponential`, or `gaussian`. The following demo showcases the `exponential` one. The effects are best observed on a larger block of text.


### Demo Code (text-cursor-proximity-falloff-demo.tsx)
```tsx
"use client"

import { useRef } from "react"

import TextCursorProximity from "@/fancy/components/text/text-cursor-proximity"

export default function Preview() {
  const containerRef = useRef<HTMLDivElement>(null)

  return (
    <div
      
      ref={containerRef}
    >
      {/* this is the important stuff */}
      <div >
        <TextCursorProximity
          
          styles={{
            opacity: { from: 0.1, to: 1 },
          }}
          falloff="linear"
          radius={80}
          containerRef={containerRef}
        >
          Just as every problem is novel and different from others. so the grid
          must be conceived afresh every time so as to meet requirements. This
          means that the designer must approach each new problem with an open
          mind and must seek to solve it by analysing it objectively. The
          difficulties of the task are due to the enormous differences in the
          demands made on the designer by the various assignments he receives. A
          small newspaper advertisement does not present the difficulties of
          designing, say, a daily paper with 10 and more columns. a great
          variety of subjects, and an additional advertising section. Such a
          task calls not only for designing talent but also organizing ability
          since the many constantly changing items of information have to be
          arranged in a logical order and their priorities reflected in
          appropriate typography.
        </TextCursorProximity>
      </div>
    </div>
  )
}
```


## Notes

It seems like interpolating on large number of letters simultaneously can be a bit slow, even when we're avoiding re-renders with state updates. If you're experiencing performance issues, try to limit the length of the text you're animating.

## Credits

Ported to Framer by [Framer University](https://framer.university/)

## Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The content to be displayed and animated |
| as | `ElementType` | `"span"` | The HTML element to render the component as |
| styles* | `Partial` | - | CSS properties to animate and their from/to values |
| containerRef* | `React.RefObject` | - | Reference to the container for mouse tracking |
| radius | `number` | `50` | The radius of the proximity effect in pixels |
| falloff | `"linear" | "exponential" | "gaussian"` | `"linear"` | The falloff pattern for the proximity effect |
| className | `string` | - | Additional CSS classes for styling |


---

### Variable Font & Cursor <a name="variable-font--cursor"></a>

### Demo Code (variable-font-and-cursor-demo.tsx)
```tsx
"use client"

import { useRef } from "react"

import { useMousePosition } from "@/hooks/use-mouse-position"
import VariableFontAndCursor from "@/fancy/components/text/variable-font-and-cursor"

export default function Preview() {
  const containerRef = useRef<HTMLDivElement>(null)

  const { x, y } = useMousePosition(containerRef)

  return (
    <div
      
      ref={containerRef}
    >
      {/* this is the important stuff */}
      <div >
        <VariableFontAndCursor
          
          fontVariationMapping={{
            y: { name: "wght", min: 100, max: 900 },
            x: { name: "slnt", min: 0, max: -10 },
          }}
          containerRef={containerRef}
        >
          fancy!
        </VariableFontAndCursor>
      </div>

      {/* this is just fluff for the demo */}

      <div >
        
          x: {Math.round(x)}
        
        
          y: {Math.round(y)}
        
      </div>

      <div
        
        style={{
          left: `${x}px`,
        }}
      />
      <div
        
        style={{
          top: `${y}px`,
        }}
      />
      <div
        
        style={{
          top: `${y}px`,
          left: `${x}px`,
        }}
      />
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/variable-font-and-cursor
```





Create this hook to query the cursor position's ref:


*Component Source: use-mouse-position-ref (Source file not found)*


Then, copy and paste the following code into your project:


### Component Source Code (variable-font-and-cursor.tsx)
```tsx
"use client"

import React, { ElementType, useCallback, useRef } from "react"
import { motion, useAnimationFrame } from "motion/react"

import { cn } from "@/lib/utils"
import { useMousePositionRef } from "@/hooks/use-mouse-position-ref"

/**
 * Interface for defining a single font variation axis.
 * Each axis represents a dimension of variation in a variable font. You should check the font variation settings of the font you are using to see the available axes.
 */
interface FontVariationAxis {
  /**
   * The name of the font variation axis (e.g., "wght" for weight, "slnt" for slant).
   * This corresponds to the OpenType variation axis tags, but can be arbitrary. Make sure to check the font variation settings of the font you are using to see the available axes.
   */
  name: string

  /**
   * The minimum value for this axis.
   * Applied when the cursor is at the left edge (for x-axis) or top edge (for y-axis).
   */
  min: number

  /**
   * The maximum value for this axis.
   * Applied when the cursor is at the right edge (for x-axis) or bottom edge (for y-axis).
   */
  max: number
}

/**
 * Interface for mapping cursor position to font variation settings.
 * Allows independent control of two font variation axes based on cursor movement.
 */
interface FontVariationMapping {
  /**
   * Font variation axis controlled by horizontal cursor movement.
   */
  x: FontVariationAxis

  /**
   * Font variation axis controlled by vertical cursor movement.
   */
  y: FontVariationAxis
}

/**
 * Props for the VariableFontAndCursor component.
 */
interface TextProps extends React.HTMLAttributes<HTMLElement> {
  /**
   * The text content to display and animate.
   * Required prop with no default value.
   */
  children: React.ReactNode

  /**
   * HTML Tag to render the component as.
   * @default "span"
   */
  as?: ElementType

  /**
   * Mapping configuration that defines how cursor position affects font variation settings.
   * Maps x and y cursor positions to specific font variation axes and value ranges.
   * Required prop with no default value.
   */
  fontVariationMapping: FontVariationMapping

  /**
   * Reference to the container element for mouse tracking.
   * The cursor position will be calculated relative to this container's bounds.
   * Required prop with no default value.
   */
  containerRef: React.RefObject<HTMLDivElement | null>
}

const VariableFontAndCursor = ({
  children,
  as = "span",
  fontVariationMapping,
  className,
  containerRef,
  ...props
}: TextProps) => {
  // Hook to track mouse position relative to the specified container
  const mousePositionRef = useMousePositionRef(containerRef)

  // Ref for the visible text span to apply font variation settings
  const spanRef = useRef<HTMLSpanElement>(null)

  /**
   * Calculates font variation settings based on cursor position within the container.
   *
   * This function maps the cursor's x and y coordinates to font variation values
   * by interpolating between the minimum and maximum values defined in the mapping.
   * The position is normalized to a 0-1 range based on the container dimensions.
   *
   * @param xPosition - Horizontal cursor position relative to container
   * @param yPosition - Vertical cursor position relative to container
   * @returns CSS font-variation-settings string with calculated values
   */
  const interpolateFontVariationSettings = useCallback(
    (xPosition: number, yPosition: number) => {
      const container = containerRef.current
      if (!container) return "0 0" // Return default values if container is null

      // Get container dimensions for normalization
      const containerWidth = container.clientWidth
      const containerHeight = container.clientHeight

      // Normalize cursor position to 0-1 range, clamped to container bounds
      const xProgress = Math.min(Math.max(xPosition / containerWidth, 0), 1)
      const yProgress = Math.min(Math.max(yPosition / containerHeight, 0), 1)

      // Interpolate between min and max values for each axis
      const xValue =
        fontVariationMapping.x.min +
        (fontVariationMapping.x.max - fontVariationMapping.x.min) * xProgress
      const yValue =
        fontVariationMapping.y.min +
        (fontVariationMapping.y.max - fontVariationMapping.y.min) * yProgress

      // Return CSS font-variation-settings string
      return `'${fontVariationMapping.x.name}' ${xValue}, '${fontVariationMapping.y.name}' ${yValue}`
    },
    [fontVariationMapping, containerRef]
  )

  // Use animation frame to smoothly update font variations on every frame
  // This ensures smooth transitions as the cursor moves
  useAnimationFrame(() => {
    const settings = interpolateFontVariationSettings(
      mousePositionRef.current.x,
      mousePositionRef.current.y
    )
    if (spanRef.current) {
      spanRef.current.style.fontVariationSettings = settings
    }
  })

  // Custom motion component to render as the specified HTML tag
  const MotionComponent = motion.create(as)

  return (
    <MotionComponent
      className={cn(className)}
      data-text={children}
      ref={spanRef}
      {...props}
    >
      {children}
    </MotionComponent>
  )
}

export default VariableFontAndCursor
```





## Usage

The `VariableFontAndCursor` component allows you to create text that responds to cursor movement by adjusting its font variation settings. This component works with variable fonts and can track cursor movement either within a specific container or across the entire viewport.

It's important to note that the container used for tracking mouse position is not part of the component itself. To track mouse movement within a specific area, you need to create a container element, assign it a ref, and pass that ref to the component using the `containerRef` prop. You can use the window object as a reference to the entire viewport.

### Font Variation Mapping

The `fontVariationMapping` prop allows you to define how cursor position maps to font variation settings. It has the following structure:


**File**: `Font variation mapping settings`
```tsx
interface FontVariationMapping {
  x: { name: string; min: number; max: number };
  y: { name: string; min: number; max: number };
}
```


- `x`: Defines the font variation axis controlled by horizontal cursor movement.
- `y`: Defines the font variation axis controlled by vertical cursor movement.
- `name`: The name of the font variation axis (e.g., "wght" for weight, "slnt" for slant, see next section for more details).
- `min`: The minimum value for the axis, applied when the cursor is at the left/top.
- `max`: The maximum value for the axis, applied when the cursor is at the right/bottom.

The component interpolates between `min` and `max` based on the cursor position within the tracking area.

### Understanding Variable Fonts

For more information about variable fonts and how they work, please refer to the [Variable Font Hover By Letter](/docs/components/text/variable-font-hover-by-letter#understanding-variable-fonts) documentation.

## Notes

Make sure the main container has enough space to hold the text at its full weight to avoid layout shifts. For example, you can use negative margins, or an invisible pseudo element.

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The text content to display and animate |
| as | `ElementType` | `"span"` | HTML tag to render the component as |
| fontVariationMapping* | `FontVariationMapping` | - | Mapping of cursor position to font variation settings |
| containerRef* | `React.RefObject` | - | Reference to the container for mouse tracking |
| className | `string` | - | Additional CSS classes for styling |


## Interfaces

### FontVariationMapping


| Property | Type | Description |
| --- | --- | --- |
| x | `FontVariationAxis` | Font variation settings for horizontal cursor movement |
| y | `FontVariationAxis` | Font variation settings for vertical cursor movement |


### FontVariationAxis


| Property | Type | Description |
| --- | --- | --- |
| name | `string` | Name of the font variation axis (e.g., "wght", "slnt") |
| min | `number` | Minimum value for the axis |
| max | `number` | Maximum value for the axis |


---

### Variable Font Cursor Proximity <a name="variable-font-cursor-proximity"></a>

### Demo Code (variable-font-cursor-proximity-demo.tsx)
```tsx
"use client"

import { useRef } from "react"

import { cn } from "@/lib/utils"
import VariableFontCursorProximity from "@/fancy/components/text/variable-font-cursor-proximity"

const texts = ["Overstimulated", "Underutilized", "Familiar", "Extraordinary"]

export default function Preview() {
  const containerRef = useRef<HTMLDivElement>(null)

  return (
    <div
      
      ref={containerRef}
    >
      <div >
        {texts.map((text, i) => (
          <VariableFontCursorProximity
            key={i}
            className={cn("text-4xl md:text-6xl lg:text-7xl leading-none")}
            fromFontVariationSettings="'wght' 400, 'slnt' 0"
            toFontVariationSettings="'wght' 900, 'slnt' -10"
            radius={200}
            containerRef={containerRef}
          >
            {text}
          </VariableFontCursorProximity>
        ))}
      </div>
    </div>
  )
}
```


A generalized version of this component (where you can control any CSS property) is available in the [Text Cursor Proximity](/docs/components/text/text-cursor-proximity) component.

## Installation 




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/variable-font-cursor-proximity
```






Create a hook for querying the cursor/mouse position.


*Component Source: use-mouse-position-ref (Source file not found)*


This hook actually returns a ref to the position (instead of a state), so we can avoid re-renders when the cursor moves.

Then, copy and paste the component source code into your project:


### Component Source Code (variable-font-cursor-proximity.tsx)
```tsx
"use client"

import React, { ElementType, forwardRef, useMemo, useRef } from "react"
import { motion, useAnimationFrame } from "motion/react"

import { cn } from "@/lib/utils"
import { useMousePositionRef } from "@/hooks/use-mouse-position-ref"

/**
 * Props for the VariableFontCursorProximity component.
 */
interface TextProps extends React.HTMLAttributes<HTMLElement> {
  /**
   * The text content to display and animate.
   * Each letter will respond individually to cursor proximity.
   * Required prop with no default value.
   */
  children: React.ReactNode

  /**
   * HTML Tag to render the component as.
   * @default "span"
   */
  as?: ElementType

  /**
   * Default font variation settings applied when cursor is outside the radius.
   * Should be a CSS font-variation-settings string (e.g., "'wght' 400, 'slnt' 0").
   * You should check the font variation settings of the font you are using to see the available axes.
   * Required prop with no default value.
   */
  fromFontVariationSettings: string

  /**
   * Target font variation settings applied when cursor is at the center of a letter.
   * Should be a CSS font-variation-settings string (e.g., "'wght' 900, 'slnt' 15").
   * Make sure to check the font variation settings of the font you are using to see the available axes.
   * Required prop with no default value.
   */
  toFontVariationSettings: string

  /**
   * Reference to the container element for mouse tracking.
   * The cursor position will be calculated relative to this container's bounds.
   * Required prop with no default value.
   */
  containerRef: React.RefObject<HTMLDivElement | null>

  /**
   * The radius in pixels within which letters respond to cursor proximity.
   * Letters outside this radius will use the default font variation settings.
   * @default 50
   */
  radius?: number

  /**
   * The falloff function that determines how the effect diminishes with distance.
   * - "linear": Linear interpolation (straight line falloff)
   * - "exponential": Quadratic falloff (more dramatic near cursor)
   * - "gaussian": Bell curve falloff (smooth, natural feeling)
   * @default "linear"
   */
  falloff?: "linear" | "exponential" | "gaussian"
}

const VariableFontCursorProximity = forwardRef<HTMLElement, TextProps>(
  (
    {
      children,
      as = "span",
      fromFontVariationSettings,
      toFontVariationSettings,
      containerRef,
      radius = 50,
      falloff = "linear",
      className,
      ...props
    },
    ref
  ) => {
    // Refs to store references to each individual letter element
    const letterRefs = useRef<(HTMLSpanElement | null)[]>([])

    // Cache for interpolated font settings to avoid recalculation
    const interpolatedSettingsRef = useRef<string[]>([])

    // Hook to track mouse position relative to the specified container
    const mousePositionRef = useMousePositionRef(containerRef)

    /**
     * Parse and prepare font variation settings for interpolation.
     *
     * Converts CSS font-variation-settings strings into structured data that can be
     * efficiently interpolated. Each axis is parsed with its from/to values for
     * smooth transitions during proximity-based animation.
     *
     * Expected format: "'wght' 400, 'slnt' 0" -> Map of axis names to values
     */
    const parsedSettings = useMemo(() => {
      // Parse the 'from' font variation settings string
      const fromSettings = new Map(
        fromFontVariationSettings
          .split(",")
          .map((s) => s.trim())
          .map((s) => {
            const [name, value] = s.split(" ")
            return [name.replace(/['"]/g, ""), parseFloat(value)]
          })
      )

      // Parse the 'to' font variation settings string
      const toSettings = new Map(
        toFontVariationSettings
          .split(",")
          .map((s) => s.trim())
          .map((s) => {
            const [name, value] = s.split(" ")
            return [name.replace(/['"]/g, ""), parseFloat(value)]
          })
      )

      // Create structured data for each axis with from/to values
      return Array.from(fromSettings.entries()).map(([axis, fromValue]) => ({
        axis,
        fromValue,
        toValue: toSettings.get(axis) ?? fromValue,
      }))
    }, [fromFontVariationSettings, toFontVariationSettings])

    /**
     * Calculate Euclidean distance between two points.
     *
     * Used to determine the distance between the cursor position and each letter's center.
     * This distance is then used to calculate the proximity effect strength.
     *
     * @param x1 - X coordinate of first point (cursor)
     * @param y1 - Y coordinate of first point (cursor)
     * @param x2 - X coordinate of second point (letter center)
     * @param y2 - Y coordinate of second point (letter center)
     * @returns Distance in pixels between the two points
     */
    const calculateDistance = (
      x1: number,
      y1: number,
      x2: number,
      y2: number
    ): number => {
      return Math.sqrt(Math.pow(x2 - x1, 2) + Math.pow(y2 - y1, 2))
    }

    /**
     * Calculate the falloff value based on distance and selected falloff type.
     *
     * This function determines how strongly the proximity effect affects each letter
     * based on its distance from the cursor. Different falloff types create different
     * visual effects and feelings of interaction.
     *
     * @param distance - Distance in pixels from cursor to letter center
     * @returns Falloff value between 0 (no effect) and 1 (full effect)
     */
    const calculateFalloff = (distance: number): number => {
      // Normalize distance to 0-1 range within the radius
      const normalizedDistance = Math.min(Math.max(1 - distance / radius, 0), 1)

      switch (falloff) {
        case "exponential":
          // Quadratic falloff - more dramatic effect near cursor
          return Math.pow(normalizedDistance, 2)
        case "gaussian":
          // Bell curve falloff - smooth, natural feeling
          return Math.exp(-Math.pow(distance / (radius / 2), 2) / 2)
        case "linear":
        default:
          // Linear falloff - consistent rate of change
          return normalizedDistance
      }
    }

    // Use animation frame to smoothly update font variations for all letters
    // This ensures smooth transitions as the cursor moves across the text
    useAnimationFrame(() => {
      if (!containerRef.current) return
      const containerRect = containerRef.current.getBoundingClientRect()

      // Process each letter individually for proximity-based animation
      letterRefs.current.forEach((letterRef, index) => {
        if (!letterRef) return

        // Calculate letter's center position relative to container
        const rect = letterRef.getBoundingClientRect()
        const letterCenterX = rect.left + rect.width / 2 - containerRect.left
        const letterCenterY = rect.top + rect.height / 2 - containerRect.top

        // Calculate distance from cursor to this letter's center
        const distance = calculateDistance(
          mousePositionRef.current.x,
          mousePositionRef.current.y,
          letterCenterX,
          letterCenterY
        )

        // If letter is outside the effect radius, reset to default settings
        if (distance >= radius) {
          if (
            letterRef.style.fontVariationSettings !== fromFontVariationSettings
          ) {
            letterRef.style.fontVariationSettings = fromFontVariationSettings
          }
          return
        }

        // Calculate falloff strength based on distance and falloff type
        const falloffValue = calculateFalloff(distance)

        // Interpolate between from and to settings for each axis
        const newSettings = parsedSettings
          .map(({ axis, fromValue, toValue }) => {
            const interpolatedValue =
              fromValue + (toValue - fromValue) * falloffValue
            return `'${axis}' ${interpolatedValue}`
          })
          .join(", ")

        // Cache and apply the interpolated settings
        interpolatedSettingsRef.current[index] = newSettings
        letterRef.style.fontVariationSettings = newSettings
      })
    })

    // Split text into words and track letter indices across all words
    const words = String(children).split(" ")
    let letterIndex = 0
    const ElementTag = as

    return (
      <ElementTag
        ref={ref}
        className={cn(
          className,
        )}
        {...props}
        data-text={children}
      >
        {words.map((word, wordIndex) => (
          
            {word.split("").map((letter) => {
              const currentLetterIndex = letterIndex++
              return (
                <motion.span
                  key={currentLetterIndex}
                  ref={(el: HTMLSpanElement | null) => {
                    letterRefs.current[currentLetterIndex] = el
                  }}
                  
                  aria-hidden="true"
                  style={{
                    fontVariationSettings:
                      interpolatedSettingsRef.current[currentLetterIndex],
                  }}
                >
                  {letter}
                </motion.span>
              )
            })}
            {wordIndex < words.length - 1 && (
              &nbsp;
            )}
          
        ))}
        {children}
      </ElementTag>
    )
  }
)

VariableFontCursorProximity.displayName = "VariableFontCursorProximity"
export default VariableFontCursorProximity
```





## Understanding the component

The `VariableFontCursorProximity` splits its text into letters, that respond to cursor movement by adjusting their font variation settings, based on the distance between the letter and cursor distance. This component works only with variable fonts.

1. Splitting text into individual letters
2. Tracking cursor position relative to each letter
3. Smoothly transitioning font variations based on proximity
4. Supporting multiple falloff patterns for the effect

This component requires the use of variable fonts to function properly. Otherwise it will not work.

### How it works

The component calculates the distance between the cursor and each letter in real-time. When the cursor comes within the specified `radius` of a letter, that letter's font variations (like weight, slant, etc.) smoothly interpolate between two states:

- The default state (`fromFontVariationSettings`)
- The target state (`toFontVariationSettings`)

The closer the cursor gets to a letter, the closer that letter moves toward its target state.

### Understanding Variable Fonts

For more information about variable fonts and how they work, please refer to the [Variable Font Hover By Letter](/docs/components/text/variable-font-hover-by-letter#understanding-variable-fonts) documentation.

## Examples

### Falloff

With the `falloff` prop, you can control the type of falloff. It can be either `linear`, `exponential`, or `gaussian`. The following demo showcases the `exponential` one. The effects are best observed on a larger block of text.


### Demo Code (variable-font-cursor-proximity-falloff-demo.tsx)
```tsx
"use client"

import { useRef } from "react"

import VariableFontCursorProximity from "@/fancy/components/text/variable-font-cursor-proximity"

export default function Preview() {
  const containerRef = useRef<HTMLDivElement>(null)

  return (
    <div
      
      ref={containerRef}
    >
      <div >
        <VariableFontCursorProximity
          
          fromFontVariationSettings="'wght' 400, 'slnt' 0"
          toFontVariationSettings="'wght' 900, 'slnt' -10"
          falloff="exponential"
          radius={70}
          containerRef={containerRef}
        >
          {`Modern typography is based primarily on the theories and principles of design evolved in the 20's and 30's of our century. It was Mallarmé and Rimbaud in the 19th century and Apollinaire in the early 20th century who paved the way to a new understanding of the possibilities inherent in typography and who, released from conventional prejudices and fetters, created through their experiments the basis for the pioneer achievements of the theoreticians and practitioners that followed. Walter Dexel, El Lissitzky, Kurt Schwitters, Jan Tschichold, Paul Renner, Moholy-Nagy, Joost Schmidt etc. breathed new life into an unduly rigid typography. In his book "Die neue Typografie" (1928) J. Tschichold formulated the rules of an up-to-date and objective typography which met the needs of the age.`}
        </VariableFontCursorProximity>
      </div>
    </div>
  )
}
```


## Notes

- Interpolating on large number of letters simultaneously can be a bit slow, even when we're avoiding re-renders with state updates. If you're experienceing performance issues, try to limit the length of the text you're animating.

- Make sure the main container has enough space to hold the text at its full weight to avoid layout shifts. For example, you can use negative margins like in the 2nd example.

## Credits

Ported to Framer by [Framer University](https://framer.university/)

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| label* | `string` | - | The text to be displayed and animated |
| fromFontVariationSettings* | `string` | - | Default font variation settings |
| toFontVariationSettings* | `string` | - | Target font variation settings |
| containerRef* | `React.RefObject` | - | Reference to the container for mouse tracking |
| radius | `number` | `50` | The radius of the proximity effect |
| falloff | `"linear" | "exponential" | "gaussian"` | `"linear"` | The falloff type of the proximity |
| className | `string` | - | Additional CSS classes for styling |


---

### Breathing Text <a name="breathing-text"></a>

### Demo Code (breathing-text-demo.tsx)
```tsx
import BreathingText from "@/fancy/components/text/breathing-text"

export default function Preview() {
  return (
    <div >
      <div >
        <BreathingText
          staggerDuration={0.08}
          fromFontVariationSettings="'wght' 100, 'slnt' 0"
          toFontVariationSettings="'wght' 800, 'slnt' -10"
        >
          overused grotesk
        </BreathingText>
      </div>
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/breathing-text
```






### Component Source Code (breathing-text.tsx)
```tsx
"use client"

import { ElementType } from "react"
import { motion, Transition, Variants } from "motion/react"

import { cn } from "@/lib/utils"

interface TextProps extends React.HTMLAttributes<HTMLElement> {
  /**
   * The content to be displayed and animated
   */
  children: React.ReactNode

  /**
   * HTML Tag to render the component as
   */
  as?: ElementType

  /**
   * Initial font variation settings
   */
  fromFontVariationSettings: string

  /**
   * Target font variation settings to animate to
   */
  toFontVariationSettings: string

  /**
   * Animation transition configuration
   * @default { duration: 1.5, ease: "easeInOut" }
   */
  transition?: Transition

  /**
   * Duration of stagger delay between elements in seconds
   * @default 0.1
   */
  staggerDuration?: number

  /**
   * Direction to stagger animations from
   * @default "first"
   */
  staggerFrom?: "first" | "last" | "center" | number

  /**
   * Delay between animation repeats in seconds
   * @default 0.1
   */
  repeatDelay?: number
}

const BreathingText = ({
  children,
  as = "span",
  fromFontVariationSettings,
  toFontVariationSettings,
  transition = {
    duration: 1.5,
    ease: "easeInOut",
  },
  staggerDuration = 0.1,
  staggerFrom = "first",
  repeatDelay = 0.1,
  className,
  ...props
}: TextProps) => {
  const letterVariants: Variants = {
    initial: { fontVariationSettings: fromFontVariationSettings },
    animate: (i) => ({
      fontVariationSettings: toFontVariationSettings,
      transition: {
        ...transition,
        repeat: Infinity,
        repeatType: "mirror",
        delay: i * staggerDuration,
        repeatDelay: repeatDelay,
      },
    }),
  }

  const getCustomIndex = (index: number, total: number) => {
    if (typeof staggerFrom === "number") {
      return Math.abs(index - staggerFrom)
    }
    switch (staggerFrom) {
      case "first":
        return index
      case "last":
        return total - 1 - index
      case "center":
      default:
        return Math.abs(index - Math.floor(total / 2))
    }
  }

  const letters = String(children).split("")
  const ElementTag = as

  return (
    <ElementTag
      className={cn(
        className,
        // an after pseudo element is used to create a container large enough to hold the text with full weight. Helps avoid layout shifts
        "relative after:absolute after:content-[attr(data-text)] after:font-black after:pointer-none after:overflow-hidden after:select-none after:invisible after:h-0"
      )}
      {...props}
      data-text={children}
    >
      {letters.map((letter: string, i: number) => (
        <motion.span
          key={i}
          
          aria-hidden="true"
          variants={letterVariants}
          initial="initial"
          animate="animate"
          custom={getCustomIndex(i, letters.length)}
        >
          {letter}
        </motion.span>
      ))}
      {children}
    </ElementTag>
  )
}

export default BreathingText
```





## Understanding Variable Fonts

This component is designed to work exclusively with variable fonts. Please refer to the [Variable Font Hover By Letter](/docs/components/text/variable-font-hover-by-letter#understanding-variable-fonts) documentation for more details.

## Notes

Since the animation is continous, keep the performance in check when using this component.

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The content to be displayed and animated |
| as | `ElementType` | `"span"` | HTML Tag to render the component as |
| fromFontVariationSettings* | `string` | - | Initial font variation settings |
| toFontVariationSettings* | `string` | - | Target font variation settings to animate to |
| transition | `Transition` | `{ duration: 1.5, ease: "easeInOut" }` | Animation transition configuration |
| staggerDuration | `number` | `0.1` | Duration of stagger delay between elements in seconds |
| staggerFrom | `"first" | "last" | "center" | number` | `"first"` | Direction to stagger animations from |
| repeatDelay | `number` | `0.1` | Delay between animation repeats in seconds |
| className | `string` | - | Class name for styling |


---

### Underline Animation <a name="underline-animation"></a>

### Demo Code (underline-demo.tsx)
```tsx
import Link from "next/link"

import CenterUnderline from "@/fancy/components/text/underline-center"
import ComesInGoesOutUnderline from "@/fancy/components/text/underline-comes-in-goes-out"
import GoesOutComesInUnderline from "@/fancy/components/text/underline-goes-out-comes-in"

export default function UnderlineDemo() {
  return (
    <div >
      <div >
        <div>Contact</div>
        <ul >
          <Link  href="#">
            <CenterUnderline>LINKEDIN</CenterUnderline>
          </Link>
          <Link  href="#">
            <ComesInGoesOutUnderline direction="right">
              INSTAGRAM
            </ComesInGoesOutUnderline>
          </Link>
          <Link  href="#">
            <ComesInGoesOutUnderline direction="left">
              X (TWITTER)
            </ComesInGoesOutUnderline>
          </Link>

          <div >
            <ul >
              <Link  href="#">
                <GoesOutComesInUnderline direction="left">
                  FANCY@FANCY.DEV
                </GoesOutComesInUnderline>
              </Link>
              <Link  href="#">
                <GoesOutComesInUnderline direction="right">
                  HELLO@FANCY.DEV
                </GoesOutComesInUnderline>
              </Link>
            </ul>
          </div>
        </ul>
      </div>
    </div>
  )
}
```


## Installation

### From center




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/underline-center
```


  



### Component Source Code (underline-center.tsx)
```tsx
"use client"

import { ElementType, useEffect, useRef, useMemo } from "react"
import { motion, ValueAnimationTransition } from "motion/react"
import { cn } from "@/lib/utils"

interface UnderlineProps {
  /**
   * The content to be displayed and animated
   */
  children: React.ReactNode

  /**
   * HTML Tag to render the component as
   * @default span
   */
  as?: ElementType

  /**
   * Optional class name for styling
   */
  className?: string

  /**
   * Animation transition configuration
   * @default { duration: 0.25, ease: "easeInOut" }
   */
  transition?: ValueAnimationTransition

  /**
   * Height of the underline as a ratio of font size
   * @default 0.1
   */
  underlineHeightRatio?: number

  /**
   * Padding of the underline as a ratio of font size
   * @default 0.01
   */
  underlinePaddingRatio?: number
}

const CenterUnderline = ({
  children,
  as,
  className,
  transition = { duration: 0.25, ease: "easeInOut" },
  underlineHeightRatio = 0.1,
  underlinePaddingRatio = 0.01,
  ...props
}: UnderlineProps) => {
  const textRef = useRef<HTMLSpanElement>(null)
  const MotionComponent = useMemo(() => motion.create(as ?? "span"), [as])

  useEffect(() => {
    const updateUnderlineStyles = () => {
      if (textRef.current) {
        const fontSize = parseFloat(getComputedStyle(textRef.current).fontSize)
        const underlineHeight = fontSize * underlineHeightRatio
        const underlinePadding = fontSize * underlinePaddingRatio
        textRef.current.style.setProperty(
          "--underline-height",
          `${underlineHeight}px`
        )
        textRef.current.style.setProperty(
          "--underline-padding",
          `${underlinePadding}px`
        )
      }
    }

    updateUnderlineStyles()
    window.addEventListener("resize", updateUnderlineStyles)

    return () => window.removeEventListener("resize", updateUnderlineStyles)
  }, [underlineHeightRatio, underlinePaddingRatio])

  const underlineVariants = {
    hidden: {
      width: 0,
      originX: 0.5,
    },
    visible: {
      width: "100%",
      transition: transition,
    },
  }

  return (
    <MotionComponent
      className={cn("relative inline-block cursor-pointer", className)}
      whileHover="visible"
      ref={textRef}
      {...props}
    >
      {children}
      <motion.div
        
        style={{
          height: "var(--underline-height)",
          bottom: "calc(-1 * var(--underline-padding))",
        }}
        variants={underlineVariants}
        aria-hidden="true"
      />
    </MotionComponent>
  )
}

CenterUnderline.displayName = "CenterUnderline"

export default CenterUnderline
```





### From side to side (comes in, goes out)




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/underline-comes-in-goes-out
```


  



### Component Source Code (underline-comes-in-goes-out.tsx)
```tsx
"use client"

import { ElementType, useEffect, useRef, useState, useMemo } from "react"
import cn from "clsx"
import {
  motion,
  useAnimationControls,
  ValueAnimationTransition,
} from "motion/react"

interface ComesInGoesOutUnderlineProps {
  /**
   * The content to be displayed and animated
   */
  children: React.ReactNode

  /**
   * HTML Tag to render the component as
   * @default span
   */
  as?: ElementType

  /**
   * Direction of the animation
   * @default "left"
   */
  direction?: "left" | "right"

  /**
   * Optional class name for styling
   */
  className?: string

  /**
   * Height of the underline as a ratio of font size
   * @default 0.1
   */
  underlineHeightRatio?: number

  /**
   * Padding of the underline as a ratio of font size
   * @default 0.01
   */
  underlinePaddingRatio?: number

  /**
   * Animation transition configuration
   * @default { duration: 0.4, ease: "easeInOut" }
   */
  transition?: ValueAnimationTransition
}

const ComesInGoesOutUnderline = ({
  children,
  as,
  direction = "left",
  className,
  underlineHeightRatio = 0.1,
  underlinePaddingRatio = 0.01,
  transition = {
    duration: 0.4,
    ease: "easeInOut",
  },
  ...props
}: ComesInGoesOutUnderlineProps) => {
  const controls = useAnimationControls()
  const [blocked, setBlocked] = useState(false)
  const textRef = useRef<HTMLSpanElement>(null)
  const MotionComponent = useMemo(() => motion.create(as ?? "span"), [as])

  useEffect(() => {
    const updateUnderlineStyles = () => {
      if (textRef.current) {
        const fontSize = parseFloat(getComputedStyle(textRef.current).fontSize)
        const underlineHeight = fontSize * underlineHeightRatio
        const underlinePadding = fontSize * underlinePaddingRatio
        textRef.current.style.setProperty(
          "--underline-height",
          `${underlineHeight}px`
        )
        textRef.current.style.setProperty(
          "--underline-padding",
          `${underlinePadding}px`
        )
      }
    }

    updateUnderlineStyles()
    window.addEventListener("resize", updateUnderlineStyles)

    return () => window.removeEventListener("resize", updateUnderlineStyles)
  }, [underlineHeightRatio, underlinePaddingRatio])

  const animate = async () => {
    if (blocked) return

    setBlocked(true)

    await controls.start({
      width: "100%",
      transition,
      transitionEnd: {
        left: direction === "left" ? "auto" : 0,
        right: direction === "left" ? 0 : "auto",
      },
    })

    await controls.start({
      width: 0,
      transition,
      transitionEnd: {
        left: direction === "left" ? 0 : "",
        right: direction === "left" ? "" : 0,
      },
    })

    setBlocked(false)
  }

  return (
    <MotionComponent
      className={cn("relative inline-block cursor-pointer", className)}
      onHoverStart={animate}
      ref={textRef}
      {...props}
    >
      {children}
      <motion.span
        className={cn("absolute bg-current w-0", {
          "left-0": direction === "left",
          "right-0": direction === "right",
        })}
        style={{
          height: "var(--underline-height)",
          bottom: "calc(1 * var(--underline-padding))",
        }}
        animate={controls}
        aria-hidden="true"
      />
    </MotionComponent>
  )
}

ComesInGoesOutUnderline.displayName = "ComesInGoesOutUnderline"

export default ComesInGoesOutUnderline
```





### From side to side (goes out, comes in)




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/underline-goes-out-comes-in
```


  



### Component Source Code (underline-goes-out-comes-in.tsx)
```tsx
"use client"

import { ElementType, useEffect, useRef, useState, useMemo } from "react"
import cn from "clsx"
import {
  motion,
  useAnimationControls,
  ValueAnimationTransition,
} from "motion/react"

interface GoesOutComesInUnderlineProps {
  /**
   * The content to be displayed and animated
   */
  children: React.ReactNode

  /**
   * HTML Tag to render the component as
   * @default span
   */
  as?: ElementType

  /**
   * Direction of the animation
   * @default "left"
   */
  direction?: "left" | "right"

  /**
   * Optional class name for styling
   */
  className?: string

  /**
   * Height of the underline as a ratio of font size
   * @default 0.1
   */
  underlineHeightRatio?: number

  /**
   * Padding of the underline as a ratio of font size
   * @default 0.01
   */
  underlinePaddingRatio?: number

  /**
   * Animation transition configuration
   * @default { duration: 0.5, ease: "easeOut" }
   */
  transition?: ValueAnimationTransition
}

const GoesOutComesInUnderline = ({
  children,
  as,
  direction = "left",
  className,
  underlineHeightRatio = 0.1,
  underlinePaddingRatio = 0.01,
  transition = {
    duration: 0.5,
    ease: "easeOut",
  },
  ...props
}: GoesOutComesInUnderlineProps) => {
  const controls = useAnimationControls()
  const [blocked, setBlocked] = useState(false)
  const textRef = useRef<HTMLSpanElement>(null)
  const MotionComponent = useMemo(() => motion.create(as ?? "span"), [as])

  useEffect(() => {
    const updateUnderlineStyles = () => {
      if (textRef.current) {
        const fontSize = parseFloat(getComputedStyle(textRef.current).fontSize)
        const underlineHeight = fontSize * underlineHeightRatio
        const underlinePadding = fontSize * underlinePaddingRatio
        textRef.current.style.setProperty(
          "--underline-height",
          `${underlineHeight}px`
        )
        textRef.current.style.setProperty(
          "--underline-padding",
          `${underlinePadding}px`
        )
      }
    }

    updateUnderlineStyles()
    window.addEventListener("resize", updateUnderlineStyles)

    return () => window.removeEventListener("resize", updateUnderlineStyles)
  }, [underlineHeightRatio, underlinePaddingRatio])

  const animate = async () => {
    if (blocked) return

    setBlocked(true)

    await controls.start({
      width: 0,
      transition,
      transitionEnd: {
        left: direction === "left" ? "auto" : 0,
        right: direction === "left" ? 0 : "auto",
      },
    })

    await controls.start({
      width: "100%",
      transition,
      transitionEnd: {
        left: direction === "left" ? 0 : "",
        right: direction === "left" ? "" : 0,
      },
    })

    setBlocked(false)
  }

  return (
    <MotionComponent
      className={cn("relative inline-block cursor-pointer", className)}
      onHoverStart={animate}
      ref={textRef}
      {...props}
    >
      {children}
      <motion.span
        className={cn("absolute bg-current", {
          "left-0": direction === "left",
          "right-0": direction === "right",
        })}
        style={{
          height: "var(--underline-height)",
          bottom: "calc(-1 * var(--underline-padding))",
          width: "100%",
        }}
        animate={controls}
        aria-hidden="true"
      />
    </MotionComponent>
  )
}

GoesOutComesInUnderline.displayName = "GoesOutComesInUnderline"

export default GoesOutComesInUnderline
```





## Understanding the component

These underline animations work differently from typical CSS underline animations. Instead of animating the CSS `text-decoration-line: underline` property, they create a separate `div` element positioned absolutely below the text. This div acts as the underline and its dimensions are calculated relative to the font size:

- The height is controlled by `underlineHeightRatio` (defaults to 10% of font size)
- The padding below text is controlled by `underlinePaddingRatio` (defaults to 1% of font size)

The three variants work as follows:

- **Center**: The underline grows outward from the center point
- **Comes In Goes Out**: The underline enters from one side, then exits from the other side
- **Goes Out Comes In**: The underline exits from one side, then re-enters from the opposite side

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The content to be displayed and underlined |
| as | `ElementType` | `"span"` | The HTML element to render the component as |
| direction          (only for side-to-side variants) | `"left" | "right"` | `"left"` | The direction of the underline animation |
| className | `string` | `undefined` | Additional CSS classes for styling |
| onClick | `() => void` | `undefined` | Callback function when the text is clicked |
| underlineHeightRatio | `number` | `0.1` | Height of the underline as a ratio of the font size |
| underlinePaddingRatio | `number` | `0.01` | Padding below the text as a ratio of the font size |
| transition | `ValueAnimationTransition` | Varies by variant | Animation configuration, refer to motion docs for more details |


---

### Underline To Background <a name="underline-to-background"></a>

### Demo Code (underline-to-background-demo.tsx)
```tsx
"use client"

import { motion } from "motion/react"

import UnderlineToBackground from "@/fancy/components/text/underline-to-background"

export default function UnderlineToBackgroundDemo() {
  const fadeInVariants = {
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: { duration: 0.5, staggerChildren: 0.1 },
    },
  }

  const wordVariants = {
    hidden: { opacity: 0 },
    visible: { opacity: 1 },
  }

  const words = "Weekly goodies delivered straight to your inbox —".split(" ")

  return (
    <div >
      <motion.h2
        
        initial="hidden"
        animate="visible"
        variants={fadeInVariants}
      >
        {words.map((word, index) => (
          <motion.span
            key={index}
            variants={wordVariants}
            
          >
            {word}
          </motion.span>
        ))}
        <motion.span variants={wordVariants} >
          <UnderlineToBackground
            targetTextColor="#f0f0f0"
            
          >
            subscribe
          </UnderlineToBackground>
        </motion.span>
      </motion.h2>
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/underline-to-background
```





### Component Source Code (underline-to-background.tsx)
```tsx
"use client"

import { ElementType, useEffect, useMemo, useRef } from "react"
import { motion, ValueAnimationTransition } from "motion/react"

import { cn } from "@/lib/utils"

interface UnderlineProps {
  /**
   * The content to be displayed and animated
   */
  children: React.ReactNode

  /**
   * HTML Tag to render the component as
   * @default span
   */
  as?: ElementType

  /**
   * Optional class name for styling
   */
  className?: string

  /**
   * Animation transition configuration
   * @default { type: "spring", damping: 30, stiffness: 300 }
   */
  transition?: ValueAnimationTransition

  /**
   * The color that the text will animate to on hover
   */
  targetTextColor: string

  /**
   * Height of the underline as a ratio of font size
   * @default 0.1
   */
  underlineHeightRatio?: number

  /**
   * Padding of the underline as a ratio of font size
   * @default 0.01
   */
  underlinePaddingRatio?: number
}

const UnderlineToBackground = ({
  children,
  as,
  className,
  transition = { type: "spring", damping: 30, stiffness: 300 },
  underlineHeightRatio = 0.1, // Default to 10% of font size
  underlinePaddingRatio = 0.01, // Default to 1% of font size
  targetTextColor = "#fef",
  ...props
}: UnderlineProps) => {
  const textRef = useRef<HTMLSpanElement>(null)

  // Create custom motion component based on the 'as' prop
  const MotionComponent = useMemo(() => motion.create(as ?? "span"), [as])

  // Update CSS custom properties based on font size
  useEffect(() => {
    const updateUnderlineStyles = () => {
      if (textRef.current) {
        const fontSize = parseFloat(getComputedStyle(textRef.current).fontSize)
        const underlineHeight = fontSize * underlineHeightRatio
        const underlinePadding = fontSize * underlinePaddingRatio
        textRef.current.style.setProperty(
          "--underline-height",
          `${underlineHeight}px`
        )
        textRef.current.style.setProperty(
          "--underline-padding",
          `${underlinePadding}px`
        )
      }
    }

    updateUnderlineStyles()
    window.addEventListener("resize", updateUnderlineStyles)

    return () => window.removeEventListener("resize", updateUnderlineStyles)
  }, [underlineHeightRatio, underlinePaddingRatio])

  // Animation variants for the underline background
  const underlineVariants = {
    initial: {
      height: "var(--underline-height)",
    },
    target: {
      height: "100%",
      transition: transition,
    },
  }

  // Animation variants for the text color
  const textVariants = {
    initial: {
      color: "currentColor",
    },
    target: {
      color: targetTextColor,
      transition: transition,
    },
  }

  return (
    <MotionComponent
      className={cn("relative inline-block cursor-pointer", className)}
      whileHover="target"
      ref={textRef}
      {...props}
    >
      <motion.div
        
        style={{
          height: "var(--underline-height)",
          bottom: "calc(-1 * var(--underline-padding))",
        }}
        variants={underlineVariants}
        aria-hidden="true"
      />
      <motion.span variants={textVariants} >
        {children}
      </motion.span>
    </MotionComponent>
  )
}

UnderlineToBackground.displayName = "UnderlineToBackground"

export default UnderlineToBackground
```





## Understanding the component

The component creates a separate `div` element positioned absolutely below the text (instead of the regular underline elements with CSS pseudo-elements). It animates the height from a thin line (controlled by `underlineHeightRatio`) to cover the full text height, effectively becoming a background. Simultaneously, it transitions the text color to maintain contrast against the expanding background.

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The content to be displayed and animated |
| as | `ElementType` | `"span"` | HTML Tag to render the component as |
| className | `string` | `undefined` | Optional class name for styling |
| transition | `ValueAnimationTransition` | `{ type: "spring", damping: 30, stiffness: 300 }` | Animation transition configuration |
| targetTextColor* | `string` | `"#fef"` | The color that the text will animate to on hover |
| underlineHeightRatio | `number` | `0.1` | Height of the underline as a ratio of font size |
| underlinePaddingRatio | `number` | `0.01` | Padding of the underline as a ratio of font size |


---

### Basic Number Ticker <a name="basic-number-ticker"></a>

### Demo Code (basic-number-ticker-demo.tsx)
```tsx
import NumberTicker from "@/fancy/components/text/basic-number-ticker"

const NumberTickerDemo = () => {
  return (
    <div >
      <p >
        <NumberTicker
          from={0}
          target={100}
          autoStart={true}
          transition={{ duration: 3.5, type: "tween", ease: "easeInOut" }}
          onComplete={() => console.log("complete")}
          onStart={() => console.log("start")}
        />
        %
      </p>
    </div>
  )
}

export default NumberTickerDemo
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/basic-number-ticker
```






### Component Source Code (basic-number-ticker.tsx)
```tsx
"use client"

import {
  forwardRef,
  useCallback,
  useEffect,
  useImperativeHandle,
  useState,
} from "react"
import {
  animate,
  AnimationPlaybackControls,
  motion,
  useMotionValue,
  useTransform,
  ValueAnimationTransition,
} from "motion/react"

import { cn } from "@/lib/utils"

interface NumberTickerProps {
  from: number // Starting value of the animation
  target: number // End value of the animation
  transition?: ValueAnimationTransition // Animation configuration, refer to motion docs for more details
  className?: string // additionl CSS classes for styling
  onStart?: () => void // Callback function when animation starts
  onComplete?: () => void // Callback function when animation completes
  autoStart?: boolean // Whether to start the animation automatically
}

// Ref interface to allow external control of the animation
export interface NumberTickerRef {
  startAnimation: () => void
}

const NumberTicker = forwardRef<NumberTickerRef, NumberTickerProps>(
  (
    {
      from = 0,
      target = 100,
      transition = {
        duration: 3,
        type: "tween",
        ease: "easeInOut",
      },
      className,
      onStart,
      onComplete,
      autoStart = true,
      ...props
    },
    ref
  ) => {
    const count = useMotionValue(from)
    const rounded = useTransform(count, (latest) => Math.round(latest))
    const [controls, setControls] = useState<AnimationPlaybackControls | null>(
      null
    )

    // Function to start the animation
    const startAnimation = useCallback(() => {
      if (controls) controls.stop()
      onStart?.()

      count.set(from)

      const newControls = animate(count, target, {
        ...transition,
        onComplete: () => {
          onComplete?.()
        },
      })
      setControls(newControls)
    }, [])

    // Expose the startAnimation function via ref
    useImperativeHandle(ref, () => ({
      startAnimation,
    }))

    useEffect(() => {
      if (autoStart) {
        startAnimation()
      }
      return () => controls?.stop()
    }, [autoStart])

    return (
      <motion.span className={cn(className)} {...props}>
        {rounded}
      </motion.span>
    )
  }
)

NumberTicker.displayName = "NumberTicker"

export default NumberTicker

// Usage example:
// To start the animation from outside the component:
// 1. Create a ref:
//    const tickerRef = useRef<NumberTickerRef>(null);
// 2. Pass the ref to the NumberTicker component:
//    <NumberTicker ref={tickerRef} from={0} target={100} autoStart={false} />
// 3. Call the startAnimation function:
//    tickerRef.current?.startAnimation();
```





## Examples

### Controlling the animation from outside the component

To start the animation from outside the component, you can use the `startAnimation` function that is exposed via the ref. In this example, the animation (re)start when the component enters the viewport.


### Demo Code (fancy-basic-number-ticker-demo.tsx)
```tsx
"use client"

import { useEffect, useRef } from "react"
import {
  Activity,
  ArrowDownRight,
  DollarSign,
  LucideIcon,
  TrendingUp,
  Zap,
} from "lucide-react"
import { motion, useInView } from "motion/react"

import NumberTicker, {
  NumberTickerRef,
} from "@/fancy/components/text/basic-number-ticker"

const cards = [
  {
    title: "Revenue",
    icon: DollarSign,
    from: 0,
    target: 1250321,
    prefix: "$",
    suffix: "",
    gradient: "from-gray-100 to-blue-400",
    size: "large",
  },
  {
    title: "Conversion Rate",
    icon: TrendingUp,
    from: 0,
    target: 12.5,
    prefix: "",
    suffix: "%",
    gradient: "from-gray-100 to-purple-200",
    size: "small",
  },
  {
    title: "Bounce Rate",
    icon: ArrowDownRight,
    from: 100,
    target: 35.8,
    prefix: "",
    suffix: "%",
    gradient: "from-gray-100 to-orange-200",
    size: "small",
  },
  {
    title: "Avg. Session Duration",
    icon: Zap,
    from: 0,
    target: 245,
    prefix: "",
    suffix: "s",
    gradient: "from-gray-100 to-purple-200",
    size: "small",
  },
  {
    title: "New Users",
    icon: TrendingUp,
    from: 0,
    target: 15420,
    prefix: "",
    suffix: "",
    gradient: "from-gray-100 to-orange-200",
    size: "small",
  },
  {
    title: "Active Users",
    icon: Activity,
    from: 0,
    target: 8750,
    prefix: "",
    suffix: "",
    gradient: "from-gray-100 to-blue-200",
    size: "small",
  },
]

interface CardProps {
  title: string
  icon: LucideIcon
  from: number
  target: number
  prefix: string
  suffix: string
  gradient: string
  size: string
}

const Card = ({ card, index }: { card: CardProps; index: number }) => {
  const cardRef = useRef<HTMLDivElement>(null)
  const tickerRef = useRef<NumberTickerRef>(null)
  const inView = useInView(cardRef, { once: false })

  useEffect(() => {
    if (inView) {
      tickerRef.current?.startAnimation()
    }
  }, [inView])

  return (
    <motion.div
      ref={cardRef}
      initial={{ opacity: 0, y: 10 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.5, delay: index * 0.1 }}
      className={`p-6 bg-linear-to-b from-50% to-130% flex justify-between flex-col text-foreground dark:text-muted ${
        card.gradient
      } ${card.size === "large" ? "col-span-2 row-span-2" : ""}`}
    >
      <div >
        <h3 >{card.title}</h3>
        <card.icon className={`h-4 w-4`} />
      </div>
      <div
        className={`${card.size === "large" ? "text-2xl md:text-5xl" : "text-xl md:text-3xl"}`}
      >
        {card.prefix}
        <NumberTicker
          ref={tickerRef}
          from={card.from}
          target={card.target}
          transition={{
            duration: 3,
            ease: "easeInOut",
            type: "tween",
            delay: index * 0.2,
          }}
          
          autoStart={false}
        />
        {card.suffix}
      </div>
    </motion.div>
  )
}

export default function FancyNumberTickerDemo() {
  return (
    <div >
      <div >
        {cards.map((card, index) => (
          <Card key={index} card={card} index={index} />
        ))}
      </div>
    </div>
  )
}
```


## Props


| from* | `number` | `0` | Starting value of the animation |
| --- | --- | --- | --- |
| target* | `number` | `100` | End value of the animation |
| transition | `ValueAnimationTransition` | `{ duration: 3, type: "tween", ease: "easeInOut" }` | Animation configuration, refer to motion docs for more details |
| className | `string` | `undefined` | Additional CSS classes for styling |
| onStart | `() => void` | `undefined` | Callback function when animation starts |
| onComplete | `() => void` | `undefined` | Callback function when animation completes |
| autoStart | `boolean` | `true` | Whether to start the animation automatically |


---

### Typewriter <a name="typewriter"></a>

### Demo Code (typewriter-demo.tsx)
```tsx
import Typewriter from "@/fancy/components/text/typewriter"

export default function Preview() {
  return (
    <div >
      <p >
        {"We're born 🌞 to "}
        <Typewriter
          text={[
            "experience",
            "dance",
            "love",
            "be alive",
            "create things that make the world a better place",
          ]}
          speed={70}
          
          waitTime={1500}
          deleteSpeed={40}
          cursorChar={"_"}
        />
      </p>
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/typewriter
```






### Component Source Code (typewriter.tsx)
```tsx
"use client"

import { ElementType, useEffect, useState } from "react"
import { motion, Variants } from "motion/react"

import { cn } from "@/lib/utils"

interface TypewriterProps {
  /**
   * Text or array of texts to type out
   */
  text: string | string[]

  /**
   * HTML Tag to render the component as
   * @default div
   */
  as?: ElementType

  /**
   * Speed of typing in milliseconds
   * @default 50
   */
  speed?: number

  /**
   * Initial delay before typing starts
   * @default 0
   */
  initialDelay?: number

  /**
   * Time to wait between typing and deleting
   * @default 2000
   */
  waitTime?: number

  /**
   * Speed of deleting characters
   * @default 30
   */
  deleteSpeed?: number

  /**
   * Whether to loop through texts array
   * @default true
   */
  loop?: boolean

  /**
   * Optional class name for styling
   */
  className?: string

  /**
   * Whether to show the cursor
   * @default true
   */
  showCursor?: boolean

  /**
   * Hide cursor while typing
   * @default false
   */
  hideCursorOnType?: boolean

  /**
   * Character or React node to use as cursor
   * @default "|"
   */
  cursorChar?: string | React.ReactNode

  /**
   * Animation variants for cursor
   */
  cursorAnimationVariants?: {
    initial: Variants["initial"]
    animate: Variants["animate"]
  }

  /**
   * Optional class name for cursor styling
   */
  cursorClassName?: string
}

const Typewriter = ({
  text,
  as: Tag = "div",
  speed = 50,
  initialDelay = 0,
  waitTime = 2000,
  deleteSpeed = 30,
  loop = true,
  className,
  showCursor = true,
  hideCursorOnType = false,
  cursorChar = "|",
  cursorClassName = "ml-1",
  cursorAnimationVariants = {
    initial: { opacity: 0 },
    animate: {
      opacity: 1,
      transition: {
        duration: 0.01,
        repeat: Infinity,
        repeatDelay: 0.4,
        repeatType: "reverse",
      },
    },
  },
  ...props
}: TypewriterProps & React.HTMLAttributes<HTMLElement>) => {
  const [displayText, setDisplayText] = useState("")
  const [currentIndex, setCurrentIndex] = useState(0)
  const [isDeleting, setIsDeleting] = useState(false)
  const [currentTextIndex, setCurrentTextIndex] = useState(0)

  const texts = Array.isArray(text) ? text : [text]

  useEffect(() => {
    let timeout: NodeJS.Timeout

    const currentText = texts[currentTextIndex]

    const startTyping = () => {
      if (isDeleting) {
        if (displayText === "") {
          setIsDeleting(false)
          if (currentTextIndex === texts.length - 1 && !loop) {
            return
          }
          setCurrentTextIndex((prev) => (prev + 1) % texts.length)
          setCurrentIndex(0)
          timeout = setTimeout(() => {}, waitTime)
        } else {
          timeout = setTimeout(() => {
            setDisplayText((prev) => prev.slice(0, -1))
          }, deleteSpeed)
        }
      } else {
        if (currentIndex < currentText.length) {
          timeout = setTimeout(() => {
            setDisplayText((prev) => prev + currentText[currentIndex])
            setCurrentIndex((prev) => prev + 1)
          }, speed)
        } else if (texts.length > 1) {
          timeout = setTimeout(() => {
            setIsDeleting(true)
          }, waitTime)
        }
      }
    }

    // Apply initial delay only at the start
    if (currentIndex === 0 && !isDeleting && displayText === "") {
      timeout = setTimeout(startTyping, initialDelay)
    } else {
      startTyping()
    }

    return () => clearTimeout(timeout)
  }, [
    currentIndex,
    displayText,
    isDeleting,
    speed,
    deleteSpeed,
    waitTime,
    texts,
    currentTextIndex,
    loop,
  ])

  return (
    <Tag className={cn("inline whitespace-pre-wrap tracking-tight", className)} {...props}>
      {displayText}
      {showCursor && (
        <motion.span
          variants={cursorAnimationVariants}
          className={cn(
            cursorClassName,
            hideCursorOnType &&
              (currentIndex < texts[currentTextIndex].length || isDeleting)
              ? "hidden"
              : ""
          )}
          initial="initial"
          animate="animate"
        >
          {cursorChar}
        </motion.span>
      )}
    </Tag>
  )
}

export default Typewriter
```





## Usage

As a text, either a string or an array of strings can be passed to the component. The component will automatically split the text into an array of characters, and animate each letter one by one. If you pass an array of strings, the component will animate one text, delete it, then continue animating the next one. The component will loop through the texts if you set the `loop` prop to `true`.

The cursor at the end of the text is optional. You can use a character or even a svg node if you like. There is a prop to customize the cursor animation, where you have to define the two motion variants as `initial` and `animate`.

Ideally, the component should respect multiple lines. If you experience otherwise, please let me know.:)

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| text* | `string | string[]` | - | Text or array of texts to type out |
| as | `ElementType` | `"div"` | HTML Tag to render the component as |
| speed | `number` | `50` | Speed of typing in milliseconds |
| initialDelay | `number` | `0` | Initial delay before typing starts |
| waitTime | `number` | `2000` | Time to wait between typing and deleting |
| deleteSpeed | `number` | `30` | Speed of deleting characters |
| loop | `boolean` | `true` | Whether to loop through texts array |
| className | `string` | - | Optional class name for styling |
| showCursor | `boolean` | `true` | Whether to show the cursor |
| hideCursorOnType | `boolean` | `false` | Hide cursor while typing |
| cursorChar | `string | React.ReactNode` | `"|"` | Character or React node to use as cursor |
| cursorAnimationVariants | `{ initial: Variants["initial"]; animate: Variants["animate"] }` | See description | Animation variants for cursor. Default: `{ initial: { opacity: 0 }, animate: { opacity: 1, transition: { duration: 0.01, repeat: Infinity, repeatDelay: 0.4, repeatType: "reverse" } } }` |
| cursorClassName | `string` | `"ml-1"` | Optional class name for cursor styling |


---

### Scramble Hover <a name="scramble-hover"></a>

### Demo Code (scramble-hover-demo.tsx)
```tsx
"use client"

import { motion } from "motion/react"

import ScrambleHover from "@/fancy/components/text/scramble-hover"

export default function Preview() {
  const models = [
    "Llama 3.1 405B Instruct Turbo",
    "Llama 3.2 3B Instruct Turbo",
    "Gemma 2 27B",
    "Mistral 7B Instruct v0.3",
    "Mixtral 8x7B Instruct",
    "DeepSeek LLM Chat 67B",
    "Qwen 2.5 72B Instruct Turbo",
    "WizardLM 2 8x22B",
    "Nous Hermes 2 Mixtral",
    "StripedHyena Nous 7B",
    "DBRX Instruct",
    "MythoMax L2 13B",
    "SOLAR 10.7B Instruct",
    "Gemma 2B Instruct",
  ]

  return (
    <div >
      {models.map((model, index) => (
        <motion.div
          layout
          key={model}
          animate={{ opacity: [0, 1, 1], y: [10, 10, 0] }}
          transition={{
            duration: 0.1,
            ease: "circInOut",
            delay: index * 0.05 + 0.5,
            times: [0, 0.2, 1],
          }}
        >
          <ScrambleHover
            text={model}
            scrambleSpeed={50}
            maxIterations={8}
            useOriginalCharsOnly={true}
            
          />
        </motion.div>
      ))}
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/scramble-hover
```






### Component Source Code (scramble-hover.tsx)
```tsx
"use client"

import { useEffect, useState } from "react"
import { motion } from "motion/react"

import { cn } from "@/lib/utils"

interface ScrambleHoverProps {
  text: string
  scrambleSpeed?: number
  maxIterations?: number
  sequential?: boolean
  revealDirection?: "start" | "end" | "center"
  useOriginalCharsOnly?: boolean
  characters?: string
  className?: string
  scrambledClassName?: string
}

const ScrambleHover: React.FC<ScrambleHoverProps> = ({
  text,
  scrambleSpeed = 50,
  maxIterations = 10,
  useOriginalCharsOnly = false,
  characters = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz!@#$%^&*()_+",
  className,
  scrambledClassName,
  sequential = false,
  revealDirection = "start",
  ...props
}) => {
  const [displayText, setDisplayText] = useState(text)
  const [isHovering, setIsHovering] = useState(false)
  const [isScrambling, setIsScrambling] = useState(false)
  const [revealedIndices, setRevealedIndices] = useState(new Set<number>())

  useEffect(() => {
    let interval: NodeJS.Timeout
    let currentIteration = 0

    const getNextIndex = () => {
      const textLength = text.length
      switch (revealDirection) {
        case "start":
          return revealedIndices.size
        case "end":
          return textLength - 1 - revealedIndices.size
        case "center":
          const middle = Math.floor(textLength / 2)
          const offset = Math.floor(revealedIndices.size / 2)
          const nextIndex =
            revealedIndices.size % 2 === 0
              ? middle + offset
              : middle - offset - 1

          if (
            nextIndex >= 0 &&
            nextIndex < textLength &&
            !revealedIndices.has(nextIndex)
          ) {
            return nextIndex
          }

          for (let i = 0; i < textLength; i++) {
            if (!revealedIndices.has(i)) return i
          }
          return 0
        default:
          return revealedIndices.size
      }
    }

    const shuffleText = (text: string) => {
      if (useOriginalCharsOnly) {
        const positions = text.split("").map((char, i) => ({
          char,
          isSpace: char === " ",
          index: i,
          isRevealed: revealedIndices.has(i),
        }))

        const nonSpaceChars = positions
          .filter((p) => !p.isSpace && !p.isRevealed)
          .map((p) => p.char)

        // Shuffle remaining non-revealed, non-space characters
        for (let i = nonSpaceChars.length - 1; i > 0; i--) {
          const j = Math.floor(Math.random() * (i + 1))
          ;[nonSpaceChars[i], nonSpaceChars[j]] = [
            nonSpaceChars[j],
            nonSpaceChars[i],
          ]
        }

        let charIndex = 0
        return positions
          .map((p) => {
            if (p.isSpace) return " "
            if (p.isRevealed) return text[p.index]
            return nonSpaceChars[charIndex++]
          })
          .join("")
      } else {
        return text
          .split("")
          .map((char, i) => {
            if (char === " ") return " "
            if (revealedIndices.has(i)) return text[i]
            return availableChars[
              Math.floor(Math.random() * availableChars.length)
            ]
          })
          .join("")
      }
    }

    const availableChars = useOriginalCharsOnly
      ? Array.from(new Set(text.split(""))).filter((char) => char !== " ")
      : characters.split("")

    if (isHovering) {
      setIsScrambling(true)
      interval = setInterval(() => {
        if (sequential) {
          if (revealedIndices.size < text.length) {
            const nextIndex = getNextIndex()
            revealedIndices.add(nextIndex)
            setDisplayText(shuffleText(text))
          } else {
            clearInterval(interval)
            setIsScrambling(false)
          }
        } else {
          setDisplayText(shuffleText(text))
          currentIteration++
          if (currentIteration >= maxIterations) {
            clearInterval(interval)
            setIsScrambling(false)
            setDisplayText(text)
          }
        }
      }, scrambleSpeed)
    } else {
      setDisplayText(text)
      revealedIndices.clear()
    }

    return () => {
      if (interval) clearInterval(interval)
    }
  }, [
    isHovering,
    text,
    characters,
    scrambleSpeed,
    useOriginalCharsOnly,
    sequential,
    revealDirection,
    maxIterations,
  ])

  return (
    <motion.span
      onHoverStart={() => setIsHovering(true)}
      onHoverEnd={() => setIsHovering(false)}
      className={cn("inline-block whitespace-pre-wrap", className)}
      {...props}
    >
      {displayText}
      
        {displayText.split("").map((char, index) => (
          
            {char}
          
        ))}
      
    </motion.span>
  )
}

export default ScrambleHover
```





## Usage

For the scrambling effect, you can use either the original characters, or another set of characters specified in the `characters` prop.


### Demo Code (scramble-hover-new-chars-demo.tsx)
```tsx
import ScrambleHover from "@/fancy/components/text/scramble-hover"

export default function Preview() {
  return (
    <div >
      <ScrambleHover
        text={"original characters"}
        scrambleSpeed={50}
        maxIterations={8}
        useOriginalCharsOnly={true}
        
      />
      <ScrambleHover
        text={"new characters"}
        scrambleSpeed={50}
        maxIterations={8}
        useOriginalCharsOnly={false}
        
        characters="abcdefghijklmnopqrstuvwxyz!@#$%^&*()_+-=[]{}|;':\,./<>?"
      />
    </div>
  )
}
```


You can also apply a different styling on the scrambled text by passing a string to the `scrambleClassName` prop. This allows for example to use a different font family or color, like in the following example. Please not that if the `scrambledClassName` is applied, it's not going to be merged with the `className` prop, so you have to style the original text and the scrambled text separately.


### Demo Code (scramble-hover-diff-class-demo.tsx)
```tsx
import ScrambleHover from "@/fancy/components/text/scramble-hover"

export default function Preview() {
  return (
    <div >
      <ScrambleHover
        text={"special symbols"}
        scrambleSpeed={50}
        maxIterations={8}
        useOriginalCharsOnly={false}
        
        characters="čüỳĦØ↋⒬¢⏧⏛⏄⎄*¿"
        scrambledClassName="font-notoSansSymbols text-3xl cursor-pointer"
      />
    </div>
  )
}
```


With the `sequential` prop, you can scramble the text in a sequential manner, starting from the `start`, the `end`, or the `center` of the text. In that case, the `maxIterations` prop is ignored.
In my experience this works best with a monospaced font, but feel free to experiment.


### Demo Code (scramble-hover-sequential-demo.tsx)
```tsx
import ScrambleHover from "@/fancy/components/text/scramble-hover"

export default function Preview() {
  return (
    <div >
      <div >
        <ScrambleHover
          text={"from the start"}
          scrambleSpeed={40}
          sequential={true}
          revealDirection="start"
          useOriginalCharsOnly={false}
          
          characters="abcdefghijklmnopqrstuvwxyz!@#$%^&*()_+-=[]{}|;':\,./<>?"
        />
      </div>
      <div >
        <ScrambleHover
          text={"from the center"}
          scrambleSpeed={40}
          sequential={true}
          revealDirection="center"
          useOriginalCharsOnly={false}
          
          characters="abcdefghijklmnopqrstuvwxyz!@#$%^&*()_+-=[]{}|;':\,./<>?"
        />
      </div>
      <div >
        <ScrambleHover
          text={"from the end"}
          scrambleSpeed={40}
          sequential={true}
          revealDirection="end"
          useOriginalCharsOnly={false}
          
          characters="abcdefghijklmnopqrstuvwxyz!@#$%^&*()_+-=[]{}|;':\,./<>?"
        />
      </div>
    </div>
  )
}
```


## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| text* | `string` | - | The text to be displayed and scrambled |
| scrambleSpeed | `number` | `50` | Speed of the scrambling animation in milliseconds |
| maxIterations | `number` | `10` | Maximum number of iterations for the scrambling animation |
| sequential | `boolean` | `false` | Whether to scramble the text sequentially |
| revealDirection | `"start" | "end" | "center"` | `"start"` | The direction to reveal the scrambled text |
| useOriginalCharsOnly | `boolean` | `true` | Whether to use only the original characters or the whole string |
| className | `string` | `undefined` | Additional CSS classes for styling |
| characters | `string` | `"ABCDEFGHIJKLMNO PQRSTUVWXYZ abcdefghijklmno pqrstuvwxyz !@#$%^&*()_+"` | Characters to use for scrambling, if `useOriginalCharsOnly` is `false` |
| scrambledClassName | `string` | `undefined` | Additional CSS classes for styling the scrambled text |


---

### Text Highlighter <a name="text-highlighter"></a>

### Demo Code (text-highlighter-demo.tsx)
```tsx
"use client"

import { useEffect, useRef } from "react"
import Lenis from "lenis"

import { TextHighlighter } from "@/fancy/components/text/text-highlighter"
import { Transition } from "motion"

export default function TextHighlighterDemo() {
  const containerRef = useRef<HTMLDivElement | null>(null)

  const transition = { type: "spring", duration: 1, delay: 0.4, bounce: 0 }
  const highlightClass = "rounded-[0.3em] px-px"
  const highlightColor = "#F2AD91"
  const inViewOptions = { once: true, initial: true, amount: 0.1 }

  useEffect(() => {
    if (!containerRef.current) return

    const lenis = new Lenis({
      autoRaf: true,
      wrapper: containerRef.current,
      duration: 1.2,
      orientation: "vertical",
      gestureOrientation: "vertical",
      smoothWheel: true,
      touchMultiplier: 2,
    })

    return () => {
      lenis.destroy()
    }
  }, [])

  return (
    <div >
      <div  />

      <div
        
        ref={containerRef}
      >
        <div >
          <h1 >
            Typeface alphabets
          </h1>

          <div >
            <p >
              The present-day designer has a host of printing types at his
              disposal.{" "}
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                Since Gutenberg first invented movable type in 1436-55
              </TextHighlighter>{" "}
              hundreds of different types have been designed and cast in lead.{" "}
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                The most recent technical developments
              </TextHighlighter>{" "}
              with computer and photo-typesetting have once again brought new
              faces or variations of old ones on the market.
            </p>

            <p>
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                The choice is up to the designer
              </TextHighlighter>{" "}
              It is left to his feeling for form to use{" "}
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                good or poor typefaces
              </TextHighlighter>{" "}
              for his design work. In view of the limited space available, we
              shall refer here to only a few of the outstanding designs of the
              past and the 20th century which have appeared most frequently in
              publications.
            </p>

            <p>
              Knowledge of the quality of a typeface is of the greatest
              importance for the{" "}
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                functional, aesthetic and psychological effect
              </TextHighlighter>{" "}
              of printed matter. Again, the typographic design, i. e. the
              correct spaces between letters and words and the length and
              spacing of lines conducive to easy reading, does much to enhance
              the impression created.{" "}
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                Today the field is dominated mainly by computer and
                photo-typesetting
              </TextHighlighter>{" "}
              A typical characteristic of these forms of composition is the too
              narrow setting of the letters which makes reading difficult. The
              designer will be well advised to demand the normal spacing between
              letters when ordering photo-typesetting.
            </p>

            <p>
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                By studying the classic designs
              </TextHighlighter>{" "}
              of{" "}
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                Garamond, Casion, Bodoni, Walbaum
              </TextHighlighter>{" "}
              and others, the designer can learn what the timeless criteria are
              which produce a refined and artistic typeface that makes for ease
              of reading.
            </p>

            <p>
              The lead type designs of{" "}
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                Berthold, Helvetica, Folio, Univers
              </TextHighlighter>{" "}
              etc. produce pleasant and easily legible type areas. The
              typographic rules that apply to the roman typefaces are also valid
              for the sans serifs.
            </p>

            <p>
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                The creators of these type designs
              </TextHighlighter>{" "}
              were extremely intelligent artists with high creative powers. This
              is shown by the fact that for more than four centuries innumerable
              type designers have sought to create new type alphabets but very
              few of these have gained acceptance.{" "}
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                An alphabet of Garamond
              </TextHighlighter>{" "}
              for example, is an artistic achievement of the first order. Each
              letter has its own unmistakable face, whether lower or upper case,
              and displays the highest quality of form and originality. Each
              letter has its own personality and makes a marked impact.
            </p>

            <p>
              Every designer who is concerned with typography should take the
              trouble when creating graphic designs to{" "}
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                sketch words and sentences by hand
              </TextHighlighter>{" "}
              Many designers take advantage of the Letraset process, which can
              undoubtedly produce a clean draft design that is almost ready for
              press. But a feeling for good letter forms and an attractive
              typeface can be acquired only by constant and careful practice in
              sketching letters.
            </p>

            <p>
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                How the forms of letters can create simultaneously both tension
                and nobility
              </TextHighlighter>{" "}
              and how pleasantly legible lines of type can appear to the eye of
              the reader may be seen from the examples on the following pages.
            </p>

            <p>
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                The Renaissance created midline typography
              </TextHighlighter>{" "}
              which held its position until the 20th century.
            </p>

            <p>
              The new typography differs from the old in that it is the first to
              try to{" "}
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                develop the outward appearance from the function of the text
              </TextHighlighter>
            </p>

            <p>
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                The new typography uses the background
              </TextHighlighter>{" "}
              as an element of design which is on a par with the other elements.
            </p>

            <p>
              Earlier typography (midline typography){" "}
              <TextHighlighter
                className={highlightClass}
                transition={transition as Transition}
                highlightColor={highlightColor}
                useInViewOptions={inViewOptions}
              >
                played an active role against a dead, passive background.
              </TextHighlighter>
            </p>
          </div>
        </div>
      </div>
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/text-highlighter
```


  



### Component Source Code (text-highlighter.tsx)
```tsx
"use client"

import {
  ElementType,
  forwardRef,
  useEffect,
  useImperativeHandle,
  useMemo,
  useRef,
  useState,
} from "react"
import { motion, Transition, useInView, UseInViewOptions } from "motion/react"

import { cn } from "@/lib/utils"

type HighlightDirection = "ltr" | "rtl" | "ttb" | "btt"

type TextHighlighterProps = {
  /**
   * The text content to be highlighted
   */
  children: React.ReactNode

  /**
   * HTML element to render as
   * @default "p"
   */
  as?: ElementType

  /**
   * How to trigger the animation
   * @default "inView"
   */
  triggerType?: "hover" | "ref" | "inView" | "auto"

  /**
   * Animation transition configuration
   * @default { duration: 0.4, type: "spring", bounce: 0 }
   */
  transition?: Transition

  /**
   * Options for useInView hook when triggerType is "inView"
   */
  useInViewOptions?: UseInViewOptions

  /**
   * Class name for the container element
   */
  className?: string

  /**
   * Highlight color (CSS color string). Also can be a function that returns a color string, eg:
   * @default 'hsl(60, 90%, 68%)' (yellow)
   */
  highlightColor?: string

  /**
   * Direction of the highlight animation
   * @default "ltr" (left to right)
   */
  direction?: HighlightDirection
} & React.HTMLAttributes<HTMLElement>

export type TextHighlighterRef = {
  /**
   * Trigger the highlight animation
   * @param direction - Optional direction override for this animation
   */
  animate: (direction?: HighlightDirection) => void

  /**
   * Reset the highlight animation
   */
  reset: () => void
}

export const TextHighlighter = forwardRef<
  TextHighlighterRef,
  TextHighlighterProps
>(
  (
    {
      children,
      as = "span",
      triggerType = "inView",
      transition = { type: "spring", duration: 1, delay: 0, bounce: 0 },
      useInViewOptions = {
        once: true,
        initial: false,
        amount: 0.1,
      },
      className,
      highlightColor = "hsl(25, 90%, 80%)",
      direction = "ltr",
      ...props
    },
    ref
  ) => {
    const componentRef = useRef<HTMLDivElement>(null)
    const [isAnimating, setIsAnimating] = useState(false)
    const [isHovered, setIsHovered] = useState(false)
    const [currentDirection, setCurrentDirection] =
      useState<HighlightDirection>(direction)

    // this allows us to change the direction whenever the direction prop changes
    useEffect(() => {
      setCurrentDirection(direction)
    }, [direction])

    const isInView =
      triggerType === "inView"
        ? useInView(componentRef, useInViewOptions)
        : false

    useImperativeHandle(ref, () => ({
      animate: (animationDirection?: HighlightDirection) => {
        if (animationDirection) {
          setCurrentDirection(animationDirection)
        }
        setIsAnimating(true)
      },
      reset: () => setIsAnimating(false),
    }))

    const shouldAnimate =
      triggerType === "hover"
        ? isHovered
        : triggerType === "inView"
          ? isInView
          : triggerType === "ref"
            ? isAnimating
            : triggerType === "auto"
              ? true
              : false

    const ElementTag = as || "span"

    // Get background size based on direction
    const getBackgroundSize = (animated: boolean) => {
      switch (currentDirection) {
        case "ltr":
          return animated ? "100% 100%" : "0% 100%"
        case "rtl":
          return animated ? "100% 100%" : "0% 100%"
        case "ttb":
          return animated ? "100% 100%" : "100% 0%"
        case "btt":
          return animated ? "100% 100%" : "100% 0%"
        default:
          return animated ? "100% 100%" : "0% 100%"
      }
    }

    // Get background position based on direction
    const getBackgroundPosition = () => {
      switch (currentDirection) {
        case "ltr":
          return "0% 0%"
        case "rtl":
          return "100% 0%"
        case "ttb":
          return "0% 0%"
        case "btt":
          return "0% 100%"
        default:
          return "0% 0%"
      }
    }

    const animatedSize = useMemo(() => getBackgroundSize(shouldAnimate), [shouldAnimate, currentDirection])
    const initialSize = useMemo(() => getBackgroundSize(false), [currentDirection])
    const backgroundPosition = useMemo(() => getBackgroundPosition(), [currentDirection])

    const highlightStyle = {
      backgroundImage: `linear-gradient(${highlightColor}, ${highlightColor})`,
      backgroundRepeat: "no-repeat",
      backgroundPosition: backgroundPosition,
      backgroundSize: animatedSize,
      boxDecorationBreak: "clone",
      WebkitBoxDecorationBreak: "clone",
    } as React.CSSProperties

    return (
      <ElementTag
        ref={componentRef}
        onMouseEnter={() => triggerType === "hover" && setIsHovered(true)}
        onMouseLeave={() => triggerType === "hover" && setIsHovered(false)}
        {...props}
      >
        <motion.span
          className={cn("inline", className)}
          style={highlightStyle}
          animate={{
            backgroundSize: animatedSize,
          }}
          initial={{
            backgroundSize: initialSize,
          }}
          transition={transition}
        >
          {children}
        </motion.span>
      </ElementTag>
    )
  }
)

TextHighlighter.displayName = "TextHighlighter"

export default TextHighlighter
```





## Usage

Just wrap your text content with the component and set the highlight color with the `highlightColor` prop.

**File**: `Usage example`
```tsx
<TextHighlighter highlightColor="hsl(25, 90%, 80%)">Howdy!</TextHighlighter>
```


## Understanding the component

The magic behind this component lies in animating the text's background. Instead of using a solid background color (CSS prop: `background-color`), we use `background-image` with a linear gradient. This allows us to animate the entire background of the text by changing the `background-size` property; something that wouldn't be possible with the simple `background-color` property. We also use a linear gradient because we can't set a solid color directly as a background image (as far as I know).


**File**: `Highlighter style`
```tsx
const highlightStyle = {
  backgroundImage: `linear-gradient(${highlightColor}, ${highlightColor})`,
  backgroundRepeat: "no-repeat",
  backgroundPosition: backgroundPosition,
  backgroundSize: animatedSize,
  boxDecorationBreak: "clone",
  WebkitBoxDecorationBreak: "clone",
} as React.CSSProperties
```


We also use `box-decoration-break: clone` to make sure each individual line is properly highlighted when dealing with multi-line text. Check out [this demo](https://developer.mozilla.org/en-US/docs/Web/CSS/box-decoration-break) why this is important.

The direction of the highlight reveal is controlled by the `direction` prop. Depending on the value, we set the `background-position` and `background-size` accordingly. There is two function which returns the appropriate values:


**File**: `Get animation values by direction`
```tsx
// Get background size based on direction
const getBackgroundSize = (animated: boolean) => {
  switch (currentDirection) {
    case "ltr":
      return animated ? "100% 100%" : "0% 100%"
    case "rtl":
      return animated ? "100% 100%" : "0% 100%"
    case "ttb":
      return animated ? "100% 100%" : "100% 0%"
    case "btt":
      return animated ? "100% 100%" : "100% 0%"
    default:
      return animated ? "100% 100%" : "0% 100%"
  }
}

// Get background position based on direction
const getBackgroundPosition = () => {
  switch (currentDirection) {
    case "ltr":
      return "0% 0%"
    case "rtl":
      return "100% 0%"
    case "ttb":
      return "0% 0%"
    case "btt":
      return "0% 100%"
    default:
      return "0% 0%"
  }
}
```


Then, we just use motion to animate the `background-size` property based on the `shouldAnimate` state:


**File**: `Animation`
```tsx
<motion.span
  className={cn("inline", className)}
  style={highlightStyle}
  animate={{
    backgroundSize: animatedSize,
  }}
  initial={{
    backgroundSize: initialSize,
  }}
  transition={transition}
>
  {children}
</motion.span>
```


You can customize the transition by passing a `Transition` object to the `transition` prop. The default value is spring type animation `{ type: "spring", duration: 1, delay: 0., bounce: 0 }`.

By default, the animation will be triggered once the component is mounted. Another interesting trigger option is `inView`, which will trigger the animation when the component enters the viewport (demonstrated in the demo above). You can customize that behaviour by setting the `useInViewOptions` prop. For more information, check out the [useInView](https://www.react-spring.io/docs/hooks/use-in-view) documentation.

### Different directions

You can control the highlight animation direction via the `direction` prop. The available options are:

- `"ltr"` - Left to right animation
- `"rtl"` - Right to left animation  
- `"ttb"` - Top to bottom animation
- `"btt"` - Bottom to top animation

The following demo shows how to dynamically change the reveal direction based on the user's scroll direction. Scroll left and right to see the animations trigger.


### Demo Code (text-highlighter-scroll-demo.tsx)
```tsx
"use client"

import React, { useEffect, useRef, useState } from "react"
import { motion, Transition, useInView } from "motion/react"

import { TextHighlighter } from "@/fancy/components/text/text-highlighter"

const HIGHLIGHT_COLOR = "hsl(80, 100%, 50%)"

const DEMO_USE_IN_VIEW_OPTIONS = { once: false, initial: false, amount: 0.1 }
const DEMO_TRANSITION = { type: "spring", duration: 1, delay: 0.4, bounce: 0 }

const SECTION_CLASSES =
  "min-w-full h-full snap-start flex items-center justify-center shrink-0"
const CONTAINER_CLASSES =
  "max-w-[240px] sm:max-w-sm md:max-w-md lg:max-w-lg xl:max-w-xl mx-auto px-4 sm:px-6"
const PARAGRAPH_CLASSES =
  "text-sm sm:text-base md:text-lg leading-relaxed font-overusedGrotesk mb-3 sm:mb-4 last:mb-0"

function Section({
  children,
  delay = 0,
}: {
  children: React.ReactNode
  delay?: number
}) {
  const ref = useRef(null)
  const isInView = useInView(ref, {
    once: false,
    margin: "-20%",
    amount: 0.5,
  })

  return (
    <section className={SECTION_CLASSES}>
      <div className={CONTAINER_CLASSES}>
        <motion.div
          ref={ref}
          initial={{
            opacity: 0,
            filter: "blur(8px)",
          }}
          animate={
            isInView
              ? { opacity: 1, filter: "blur(0px)" }
              : { opacity: 0.3, filter: "blur(6px)" }
          }
          transition={{
            duration: 0.8,
            delay: isInView ? delay : 0,
            ease: [0.25, 0.1, 0.25, 1],
          }}
          
        >
          {children}
        </motion.div>
      </div>
    </section>
  )
}

function Paragraph({ children }: { children: React.ReactNode }) {
  return <p className={PARAGRAPH_CLASSES}>{children}</p>
}

export default function TextHighlighterDemo() {
  const containerRef = useRef<HTMLDivElement>(null)
  const [currentSection, setCurrentSection] = useState(1)
  const [scrollDirection, setScrollDirection] = useState<"ltr" | "rtl">("ltr")

  useEffect(() => {
    const container = containerRef.current
    if (!container) return

    let prevScrollLeft = container.scrollLeft

    const handleScroll = () => {
      const scrollLeft = container.scrollLeft
      const containerWidth = container.clientWidth
      const sectionIndex = Math.round(scrollLeft / containerWidth) + 1
      setCurrentSection(Math.min(5, Math.max(1, sectionIndex)))

      const scrollDiff = scrollLeft - prevScrollLeft
      if (Math.abs(scrollDiff) > 5) {
        setScrollDirection(scrollDiff > 0 ? "ltr" : "rtl")
      }
      prevScrollLeft = scrollLeft
    }

    container.addEventListener("scroll", handleScroll)
    return () => container.removeEventListener("scroll", handleScroll)
  }, [])

  return (
    <div >
      <div >
        <div key={currentSection} >
          {currentSection.toString().padStart(2, "0")}
        </div>
      </div>

      <div
        ref={containerRef}
        
      >
        <Section>
          <Paragraph>
            Our 
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              object detection systems
            </TextHighlighter>
             identify and locate items in real-time. From 
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              facial recognition
            </TextHighlighter>
            
              {" "}
              to product identification, we deliver precision at scale.
            
          </Paragraph>

          <Paragraph>
            Whether it's 
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              traffic monitoring
            </TextHighlighter>
             for smart cities or 
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              inventory management
            </TextHighlighter>
            
              {" "}
              for retail, our AI distinguishes between people, vehicles, and
              objects with unmatched accuracy.
            
          </Paragraph>
        </Section>

        <Section>
          <Paragraph>
            Advanced 
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              video analytics
            </TextHighlighter>
             track movement across frames. Our 
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              object tracking algorithms
            </TextHighlighter>
            
              {" "}
              power autonomous vehicles and security systems worldwide.
            
          </Paragraph>

          <Paragraph>
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              Scene understanding
            </TextHighlighter>
            
              {" "}
              capabilities analyze spatial relationships and context. From{` `}
            
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              sports performance analysis
            </TextHighlighter>
             to 
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              surveillance systems
            </TextHighlighter>
            , we make sense of complex visual data.
          </Paragraph>
        </Section>

        <Section>
          <Paragraph>
            Our 
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              OCR technology
            </TextHighlighter>
            
              {" "}
              converts printed and handwritten text to digital format instantly.{" "}
            
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              Document automation
            </TextHighlighter>
             streamlines workflows across industries.
          </Paragraph>

          <Paragraph>
            From 
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              invoice processing
            </TextHighlighter>
             to 
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              accessibility solutions
            </TextHighlighter>
            
              , our text recognition supports multiple languages and formats
              with exceptional accuracy.
            
          </Paragraph>
        </Section>

        <Section>
          <Paragraph>
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              3D depth perception
            </TextHighlighter>
             enables precise spatial understanding. Our 
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              stereo vision systems
            </TextHighlighter>
            
              {" "}
              power robotic automation and quality control processes.
            
          </Paragraph>

          <Paragraph>
            Advanced 
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              augmented reality
            </TextHighlighter>
             and 
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              virtual reality applications
            </TextHighlighter>
            
              {" "}
              rely on our depth analysis for immersive, interactive experiences.
            
          </Paragraph>
        </Section>

        <Section>
          <Paragraph>
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              Image segmentation
            </TextHighlighter>
            
              {" "}
              separates objects with pixel-perfect precision. Our{` `}
            
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              enhancement algorithms
            </TextHighlighter>
            
              {" "}
              restore clarity and remove noise from any visual content.
            
          </Paragraph>

          <Paragraph>
            Generate 
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              synthetic training data
            </TextHighlighter>
             and create 
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              high-resolution imagery
            </TextHighlighter>
             for machine learning models and creative applications.
          </Paragraph>

          <Paragraph>
            <TextHighlighter
              highlightColor={HIGHLIGHT_COLOR}
              direction={scrollDirection}
              useInViewOptions={DEMO_USE_IN_VIEW_OPTIONS}
              transition={DEMO_TRANSITION as Transition}
            >
              Transform your industry
            </TextHighlighter>
            
              {" "}
              with computer vision that sees, understands, and acts on visual
              information like never before.
            
          </Paragraph>
        </Section>
      </div>
    </div>
  )
}
```


### Hover

You can also trigger the highlight animation via hover, if you set the `triggerType` prop to `"hover"`:


### Demo Code (text-highlighter-hover-demo.tsx)
```tsx
"use client"

import { TextHighlighter } from "@/fancy/components/text/text-highlighter"

export default function TextHighlighterHoverDemo() {
  return (
    <div >
      <div >
        <div >
          <TextHighlighter
            triggerType="hover"
            direction="ltr"
            
            highlightColor="#BBC2E2"
            transition={{ type: "spring", duration: 0.68, bounce: 0 }}
          >
            hover me - left to right
          </TextHighlighter>

          <TextHighlighter
            triggerType="hover"
            direction="rtl"
            
            highlightColor="#BBC2E2"
            transition={{ type: "spring", duration: 0.8, bounce: 0 }}
          >
            hover me - right to left
          </TextHighlighter>

          <TextHighlighter
            triggerType="hover"
            direction="ttb"
            
            highlightColor="#BBC2E2"
            transition={{ type: "spring", duration: 0.8, bounce: 0 }}
          >
            hover me - top to bottom
          </TextHighlighter>

          <TextHighlighter
            triggerType="hover"
            direction="btt"
            
            highlightColor="#BBC2E2"
            transition={{ type: "spring", duration: 0.8, bounce: 0 }}
          >
            hover me - bottom to top
          </TextHighlighter>
        </div>
      </div>
    </div>
  )
}
```


### Control via ref

You can also trigger the animation via an exposed ref. This is useful if you want to trigger the animation programmatically:


### Demo Code (text-highlighter-ref-demo.tsx)
```tsx
"use client"

import { useRef, useState } from "react"

import {
  TextHighlighter,
  TextHighlighterRef,
} from "@/fancy/components/text/text-highlighter"
import { Transition } from "motion"

export default function TextHighlighterDemo() {
  const containerRef = useRef<HTMLDivElement | null>(null)
  const highlighterRefs = useRef<TextHighlighterRef[]>([])
  const [isHighlighted, setIsHighlighted] = useState(false)

  const transition = { type: "spring", duration: 1, delay: 0, bounce: 0 }
  const highlightClass = "rounded-[0.3em] px-px"
  const highlightColor = "#F7F764"

  const handleHighlight = () => {
    highlighterRefs.current.forEach((ref) => {
      ref.animate()
    })
    setIsHighlighted(true)
  }

  const handleReset = () => {
    highlighterRefs.current.forEach((ref) => {
      ref.reset()
    })
    setIsHighlighted(false)
  }

  return (
    <div >
      <div
        
        ref={containerRef}
      >
        <div >
          <div >
            <div >
              <p>
                The present-day designer has a host of printing types at his
                disposal. Since{" "}
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  Gutenberg
                </TextHighlighter>{" "}
                first invented movable type in 1436-55 hundreds of different
                types have been designed and cast in lead. The{" "}
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  most recent technical developments
                </TextHighlighter>{" "}
                with computer and photo-typesetting have once again brought new
                faces or variations of old ones on the market.
              </p>
            </div>

            <div >
              <p>
                Knowledge of the quality of a typeface is of the greatest
                importance for the{" "}
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  functional, aesthetic and psychological effect
                </TextHighlighter>{" "}
                of printed matter. Again, the typographic design, i.e. the
                correct spaces between letters and words and the length and
                spacing of lines conducive to easy reading, does much to enhance
                the impression created.
              </p>
            </div>

            <div >
              <p>
                By studying the classic designs of{" "}
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  Garamond, Caslon, Bodoni, Walbaum
                </TextHighlighter>{" "}
                and others, the designer can learn what the timeless criteria
                are which produce a refined and artistic typeface that makes for
                ease of reading.
              </p>
            </div>

            <div >
              <p>
                The lead type designs of{" "}
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  Berthold, Helvetica, Folio, Univers
                </TextHighlighter>{" "}
                etc. produce pleasant and easily legible type areas. The
                typographic rules that apply to the roman typefaces are also
                valid for the sans serifs.
              </p>
            </div>

            <div >
              <p>
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  The creators of these type designs
                </TextHighlighter>{" "}
                were extremely intelligent artists with high creative powers.
                This is shown by the fact that for more than four centuries
                innumerable type designers have sought to create new type
                alphabets but very few of these have gained acceptance. An{" "}
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  alphabet of Garamond
                </TextHighlighter>{" "}
                for example, is an artistic achievement of the first order.
              </p>
            </div>

            <div >
              <p>
                Every designer who is concerned with typography should take the
                trouble when creating graphic designs to{" "}
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  sketch words and sentences by hand
                </TextHighlighter>
                . Many designers take advantage of the Letraset process, which
                can undoubtedly produce a clean draft design that is almost
                ready for press.
              </p>
            </div>
          </div>
        </div>
      </div>

      <div >
        <button
          onClick={isHighlighted ? handleReset : handleHighlight}
          
        >
          {isHighlighted ? "Reset" : "Highlight"}
        </button>
      </div>
    </div>
  )
}
```


## Notes

- While the component only support a single-colored highlight directly, you can change it to an image, a fancy gradient, or anything that a `background-image` can handle. Just change the appropriate line:

**File**: `Fancier highlight color`
```ts
backgroundImage: `linear-gradient(${highlightColor}, ${highlightColor})`,   // change this to make it fancier
```


- As many users have pointed out, excessive animations can be distracting and impact readability, especially when highlighting large blocks of text. Consider using animations sparingly and adjusting the transition duration and delay to create a more subtle effect. You may also want to use the `useInViewOptions` prop to control when animations trigger, for example by increasing the `amount` threshold or setting `once: true` to only animate elements once.

## Props

### TextHighlighterProps


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The text content to be highlighted |
| as | `ElementType` | `"span"` | HTML element to render as |
| triggerType | `"auto" | "hover" | "ref" | "inView"` | `"inView"` | How to trigger the animation |
| transition | `Transition` | `{ type: "spring", duration: 1, delay: 0, bounce: 0 }` | Animation transition configuration |
| useInViewOptions | `UseInViewOptions` | `{ once: true, initial: false, amount: 0.5 }` | Options for useInView hook when triggerType is "inView" |
| className | `string` | - | Class name for the container element |
| highlightColor | `string` | `"hsl(25, 90%, 80%)"` | Highlight color (CSS color string) |
| direction | `"ltr" | "rtl" | "ttb" | "btt"` | `"ltr"` | Direction of the highlight animation |


### TextHighlighterRef


| Method | Description |
| --- | --- |
| animate(direction?: HighlightDirection) | Trigger the highlight animation with optional direction override |
| reset() | Reset the highlight animation to its initial state |


---

### Scramble In <a name="scramble-in"></a>

### Demo Code (scramble-in-demo.tsx)
```tsx
import { useEffect, useRef } from "react"

import ScrambleIn, {
  ScrambleInHandle,
} from "@/fancy/components/text/scramble-in"

export default function Preview() {
  const titles = [
    "1. One More Time (featuring Romanthony) - 5:20",
    "2. Aerodynamic - 3:27",
    "3. Digital Love - 4:58",
    "4. Harder, Better, Faster, Stronger - 3:45",
    "5. Crescendolls - 3:31",
    "6. Nightvision - 1:44",
    "7. Superheroes - 3:57",
    "8. High Life - 3:22",
    "9. Something About Us - 3:51",
    "10. Voyager - 3:47",
    "11. Veridis Quo - 5:44",
    "12. Short Circuit - 3:26",
    "13. Face to Face (featuring Todd Edwards) - 3:58",
    "14. Too Long (featuring Romanthony) - 10:00",
  ]

  const scrambleRefs = useRef<(ScrambleInHandle | null)[]>([])

  useEffect(() => {
    titles.forEach((_, index) => {
      const delay = index * 50
      setTimeout(() => {
        scrambleRefs.current[index]?.start()
      }, delay)
    })
  }, [])

  return (
    <div >
      {titles.map((model, index) => (
        <ScrambleIn
          key={index}
          ref={(el) => {
            scrambleRefs.current[index] = el
          }}
          text={model}
          scrambleSpeed={25}
          scrambledLetterCount={5}
          autoStart={false}
        />
      ))}
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/scramble-in
```






### Component Source Code (scramble-in.tsx)
```tsx
"use client"

import {
  forwardRef,
  useCallback,
  useEffect,
  useImperativeHandle,
  useState,
} from "react"

interface ScrambleInProps {
  text: string
  scrambleSpeed?: number
  scrambledLetterCount?: number
  characters?: string
  className?: string
  scrambledClassName?: string
  autoStart?: boolean
  onStart?: () => void
  onComplete?: () => void
}

export interface ScrambleInHandle {
  start: () => void
  reset: () => void
}

const ScrambleIn = forwardRef<ScrambleInHandle, ScrambleInProps>(
  (
    {
      text,
      scrambleSpeed = 50,
      scrambledLetterCount = 2,
      characters = "abcdefghijklmnopqrstuvwxyz!@#$%^&*()_+",
      className = "",
      scrambledClassName = "",
      autoStart = true,
      onStart,
      onComplete,
    },
    ref
  ) => {
    const [displayText, setDisplayText] = useState("")
    const [isAnimating, setIsAnimating] = useState(false)
    const [visibleLetterCount, setVisibleLetterCount] = useState(0)
    const [scrambleOffset, setScrambleOffset] = useState(0)

    const startAnimation = useCallback(() => {
      setIsAnimating(true)
      setVisibleLetterCount(0)
      setScrambleOffset(0)
      onStart?.()
    }, [onStart])

    const reset = useCallback(() => {
      setIsAnimating(false)
      setVisibleLetterCount(0)
      setScrambleOffset(0)
      setDisplayText("")
    }, [])

    useImperativeHandle(ref, () => ({
      start: startAnimation,
      reset,
    }))

    useEffect(() => {
      if (autoStart) {
        startAnimation()
      }
    }, [autoStart, startAnimation])

    useEffect(() => {
      let interval: NodeJS.Timeout

      if (isAnimating) {
        interval = setInterval(() => {
          // Increase visible text length
          if (visibleLetterCount < text.length) {
            setVisibleLetterCount((prev) => prev + 1)
          }
          // Start sliding scrambled text out
          else if (scrambleOffset < scrambledLetterCount) {
            setScrambleOffset((prev) => prev + 1)
          }
          // Complete animation
          else {
            clearInterval(interval)
            setIsAnimating(false)
            onComplete?.()
          }

          // Calculate how many scrambled letters we can show
          const remainingSpace = Math.max(0, text.length - visibleLetterCount)
          const currentScrambleCount = Math.min(
            remainingSpace,
            scrambledLetterCount
          )

          // Generate scrambled text
          const scrambledPart = Array(currentScrambleCount)
            .fill(0)
            .map(
              () => characters[Math.floor(Math.random() * characters.length)]
            )
            .join("")

          setDisplayText(text.slice(0, visibleLetterCount) + scrambledPart)
        }, scrambleSpeed)
      }

      return () => {
        if (interval) clearInterval(interval)
      }
    }, [
      isAnimating,
      text,
      visibleLetterCount,
      scrambleOffset,
      scrambledLetterCount,
      characters,
      scrambleSpeed,
      onComplete,
    ])

    const renderText = () => {
      const revealed = displayText.slice(0, visibleLetterCount)
      const scrambled = displayText.slice(visibleLetterCount)

      return (
        <>
          {revealed}
          {scrambled}
        </>
      )
    }

    return (
      <>
        {text}
        
          {renderText()}
        
      </>
    )
  }
)

ScrambleIn.displayName = "ScrambleIn"
export default ScrambleIn
```





## Usage

With the `autoStart` prop, you can start the animation automatically.
But there is also a `start` and `reset` method exposed via a ref if you need to control the animation from outside of the component, as you see in the demo above.

## Credits

Ported to Framer by [Framer University](https://framer.university/)

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| text* | `string` | - | The text to be displayed and scrambled |
| scrambleSpeed | `number` | `50` | Speed of the scrambling animation in milliseconds |
| scrambledLetterCount | `number` | `8` | Number of letters to scramble |
| autoStart | `boolean` | `true` | Whether to start the animation automatically |
| className | `string` | `undefined` | Additional CSS classes for styling |
| characters | `string` | `"ABCDEFGHIJKLMNO PQRSTUVWXYZ abcdefghijklmno pqrstuvwxyz !@#$%^&*()_+"` | Characters to use for scrambling |
| scrambledClassName | `string` | `undefined` | Additional CSS classes for styling the scrambled text |
| autoStart | `boolean` | `true` | Whether to start the animation automatically |
| onComplete | `() => void` | - | Callback function for when the animation completes |
| onStart | `() => void` | - | Callback function for when the animation starts |


---

### Text Along Path <a name="text-along-path"></a>

### Demo Code (text-along-path-demo.tsx)
```tsx
import { useCallback, useState } from "react"
import { AnimatePresence, motion } from "motion/react"

import { Button } from "@/components/ui/button"
import AnimatedPathText from "@/fancy/components/text/text-along-path"

export default function Preview() {
  // Rounded rectangle path
  const rectPath =
    "M 20,20 L 180,20 A 20,20 0 0,1 200,40 L 200,160 A 20,20 0 0,1 180,180 L 20,180 A 20,20 0 0,1 0,160 L 0,40 A 20,20 0 0,1 20,20"
  const [buttonState, setButtonState] = useState<
    "idle" | "loading" | "success"
  >("idle")
  const [email, setEmail] = useState("")

  const buttonCopy = {
    idle: "Subscribe",
    loading: (
      <motion.div  />
    ),
    success: "Done ✓",
  } as const

  const handleSubmit = useCallback(() => {
    if (buttonState === "success") return

    setButtonState("loading")

    setTimeout(() => {
      setButtonState("success")
    }, 1750)

    setTimeout(() => {
      setButtonState("idle")
      setEmail("")
    }, 3500)
  }, [buttonState])

  return (
    <div >
      <AnimatedPathText
        path={rectPath}
        svgClassName="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 py-2 sm:py-8"
        viewBox="-20 10 240 180"
        text="JOIN THE WAITLIST ✉ JOIN THE WAITLIST ✉ JOIN THE WAITLIST ✉ JOIN THE WAITLIST ✉ JOIN THE WAITLIST ✉ "
        textClassName="text-[10.6px] lowercase font-azeret-mono text-primary-blue"
        duration={20}
        preserveAspectRatio="none"
        textAnchor="start"
      />

      {/* This is just fluff for the demo */}
      <div >
        <div >
          <input
            type="email"
            placeholder="Enter your email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
            
          />
          <Button
            type="submit"
            onClick={handleSubmit}
            disabled={buttonState === "loading"}
            
          >
            <AnimatePresence mode="popLayout" initial={false}>
              <motion.span
                transition={{ type: "spring", duration: 0.3, bounce: 0 }}
                initial={{ opacity: 0, y: -25 }}
                animate={{ opacity: 1, y: 0 }}
                exit={{ opacity: 0, y: 25 }}
                key={buttonState}
              >
                {buttonCopy[buttonState]}
              </motion.span>
            </AnimatePresence>
          </Button>
        </div>
      </div>
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/text-along-path
```






### Component Source Code (text-along-path.tsx)
```tsx
import { RefObject, useEffect, useRef } from "react"
import { useScroll, UseScrollOptions, useTransform } from "motion/react"

type PreserveAspectRatioAlign =
  | "none"
  | "xMinYMin"
  | "xMidYMin"
  | "xMaxYMin"
  | "xMinYMid"
  | "xMidYMid"
  | "xMaxYMid"
  | "xMinYMax"
  | "xMidYMax"
  | "xMaxYMax"

type PreserveAspectRatioMeetOrSlice = "meet" | "slice"

type PreserveAspectRatio =
  | PreserveAspectRatioAlign
  | `${Exclude<PreserveAspectRatioAlign, "none">} ${PreserveAspectRatioMeetOrSlice}`

interface AnimatedPathTextProps {
  // Path properties
  path: string
  pathId?: string
  pathClassName?: string
  preserveAspectRatio?: PreserveAspectRatio
  showPath?: boolean

  // SVG properties
  width?: string | number
  height?: string | number
  viewBox?: string
  svgClassName?: string

  // Text properties
  text: string
  textClassName?: string
  textAnchor?: "start" | "middle" | "end"

  // Animation properties
  animationType?: "auto" | "scroll"

  // Animation properties if animationType is auto
  duration?: number
  repeatCount?: number | "indefinite"
  easingFunction?: {
    calcMode?: string
    keyTimes?: string
    keySplines?: string
  }

  // Scroll animation properties if animationType is scroll
  scrollContainer?: RefObject<HTMLElement | null>
  scrollOffset?: UseScrollOptions["offset"]
  scrollTransformValues?: [number, number]
}

const AnimatedPathText = ({
  // Path defaults
  path,
  pathId,
  pathClassName,
  preserveAspectRatio = "xMidYMid meet",
  showPath = false,

  // SVG defaults
  width = "100%",
  height = "100%",
  viewBox = "0 0 100 100",
  svgClassName,

  // Text defaults
  text,
  textClassName,
  textAnchor = "start",

  // Animation type
  animationType = "auto",

  // Animation defaults
  duration = 4,
  repeatCount = "indefinite",

  easingFunction = {},

  // Scroll animation defaults
  scrollContainer,
  scrollOffset = ["start end", "end end"],
  scrollTransformValues = [0, 100],
}: AnimatedPathTextProps) => {
  const textPathRefs = useRef<SVGTextPathElement[]>([])

  // naive id for the path. you should rather use yours :)
  const id =
    pathId || `animated-path-${Math.random().toString(36).substring(7)}`

  const { scrollYProgress } = useScroll({
    ...(scrollContainer && { container: scrollContainer }),
    offset: scrollOffset,
  })

  const t = useTransform(scrollYProgress, [0, 1], scrollTransformValues)

  useEffect(() => {
    // Re-initialize scroll handler when container ref changes
    const handleChange = (e: number) => {
      textPathRefs.current.forEach((textPath) => {
        if (textPath) {
          textPath.setAttribute("startOffset", `${t.get()}%`)
        }
      })
    }

    scrollYProgress.on("change", handleChange)

    return () => {
      scrollYProgress.clearListeners()
    }
  }, [scrollYProgress, t])

  const animationProps =
    animationType === "auto"
      ? {
          from: "0%",
          to: "100%",
          begin: "0s",
          dur: `${duration}s`,
          repeatCount: repeatCount,
          ...(easingFunction && easingFunction),
        }
      : null

  return (
    <svg
      className={svgClassName}
      xmlns="http://www.w3.org/2000/svg"
      width={width}
      height={height}
      viewBox={viewBox}
      preserveAspectRatio={preserveAspectRatio}
    >
      <path
        id={id}
        className={pathClassName}
        d={path}
        stroke={showPath ? "currentColor" : "none"}
        fill="none"
      />

      {/* First text element */}
      <text textAnchor={textAnchor} fill="currentColor">
        <textPath
          className={textClassName}
          href={`#${id}`}
          startOffset={"0%"}
          ref={(ref) => {
            if (ref) textPathRefs.current[0] = ref
          }}
        >
          {animationType === "auto" && (
            <animate attributeName="startOffset" {...animationProps} />
          )}
          {text}
        </textPath>
      </text>

      {/* Second text element (offset to hide the jump) */}
      {animationType === "auto" && (
        <text textAnchor={textAnchor} fill="currentColor">
          <textPath
            className={textClassName}
            href={`#${id}`}
            startOffset={"-100%"}
            ref={(ref) => {
              if (ref) textPathRefs.current[1] = ref
            }}
          >
            {animationType === "auto" && (
              <animate
                attributeName="startOffset"
                {...animationProps}
                from="-100%"
                to="0%"
              />
            )}
            {text}
          </textPath>
        </text>
      )}
    </svg>
  )
}

export default AnimatedPathText
```





## Usage

There are two types of animations available for this component, which you can control with the `animationType` prop:

1. `auto` — plays the animation automatically when the text is initially rendered. This is the default setting.
2. `scroll` — drives the animation with the scroll position of the container.
   To use this component, you'll need to provide an SVG path via the `path` prop. You can create this path using:

- Design tools like Figma, Illustrator, or any online SVG editor
- Code, by constructing the path programmatically

The component only requires the `d` attribute from your SVG path and the `viewBox` attribute from the SVG container.

### Path ID

Each path needs a unique `id` to properly reference it in the text elements. While the component includes a basic ID generator, it's recommended to provide your own via the `pathId` prop, especially when using multiple instances of the component. This ensures animations remain distinct and don't interfere with each other.

### Sizing and ViewBox

The SVG container can be sized flexibly - by default it will expand to fill its parent container. The `viewBox` attribute can be any dimensions, but it's recommended to:

1. Match the aspect ratio you want the final component to have
2. Use dimensions that make sense for your path coordinates

For example, if your path coordinates span 0-500 on x and 0-100 on y, a viewBox of "0 0 500 100" would be appropriate.

## Understanding the component

The component consist an svg container with a path element, and two text elements with `textPath` elements inside. The `textPath` elements are used to animate the text along the path. When it is used with the `auto` animation type, we use an `animate` element to animate the text along the path. When it is used with the `scroll` animation type, we animate the `startOffset` attribute of the `textPath` elements to scroll the text along the path.

## Auto animation

The `auto` animation type is the default setting, and it plays the animation automatically when the text is initially rendered. We start at 0% offset and animate to 100% offset, which means the text will start at the beginning of the path and end at the end of the path.

The relevant props for the `auto` animation type are:

- `duration` — the duration of the animation in milliseconds
- `repeatCount` — the number of times the animation should repeat. You can also set this to `indefinite` to make the animation repeat indefinitely (default setting)


### Demo Code (text-along-path-auto-demo.tsx)
```tsx
import React, { useRef } from "react"

import AnimatedPathText from "@/fancy/components/text/text-along-path"

export default function TextAlongPathAutoDemo() {
  const containerRef = useRef<HTMLDivElement>(null)

  const paths = [
    "M1 248C214 -47 582 158 679 -39",
    "M1 208C214 -87 582 118 679 -79",
    "M1 168C214 -127 582 78 679 -119",
  ]

  const texts = [
    `PARIS • LONDON • BERLIN • ROME • BARCELONA • MADRID • VIENNA • PRAGUE • AMSTERDAM • STOCKHOLM`,
    `BUDAPEST • COPENHAGEN • OSLO • HELSINKI • MILAN • MUNICH • VENICE • MADRID • VIENNA • PRAGUE`,
    `PARIS • BERLIN • ROME • BARCELONA • MADRID • VIENNA • PRAGUE • AMSTERDAM`,
  ]

  return (
    <div
      
      ref={containerRef}
    >
      <div >
        {paths.map((path, i) => (
          <AnimatedPathText
            key={`auto-path-${i}`}
            path={path}
            pathId={`auto-path-${i}`}
            svgClassName={`absolute -left-[100px] top-1/3 w-[calc(100%+200px)] h-full`}
            viewBox="0 0 680 250"
            text={texts[i]}
            textClassName={`text font-thin text-gray-800`}
            animationType="auto"
            duration={i * 0.5 + 5}
            textAnchor="start"
          />
        ))}
      </div>
    </div>
  )
}
```


## Animation on closed paths

You might notice the component uses two identical text elements with `textPath` elements when you use the `auto` animation type. The reason for this to achieve the illusion of continuous movement on a closed path. Here is how it works:

1. The first text element starts at the beginning of the path and animates forward
2. The second text element follows behind the first one at an offset
3. When the first text reaches the end of the path, the second text has moved into position to continue the animation
4. This creates the illusion of continuous movement without any visible jumps or gaps

This dual-text approach is necessary because animating a single text element would result in a noticeable "jump" when the animation resets back to the start position.

See an example of this in the first, and the following demo above:


### Demo Code (text-along-path-circle-demo.tsx)
```tsx
import { cn } from "@/lib/utils"
import AnimatedPathText from "@/fancy/components/text/text-along-path"

export default function Preview() {
  const circlePath =
    "M 100 100 m -50, 0 a 50,50 0 1,1 100,0 a 50,50 0 1,1 -100,0"

  return (
    <div >
      {[0, 90, 180, 270].map((rotation, i) => (
        <AnimatedPathText
          key={rotation}
          path={circlePath}
          pathId={`circle-path-${i}`}
          svgClassName={cn(
            "absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-full h-full ",
            {
              "rotate-0": rotation === 0,
              "rotate-90": rotation === 90,
              "rotate-180": rotation === 180,
              "-rotate-90": rotation === 270,
            }
          )}
          easingFunction={{
            calcMode: "spline",
            keyTimes: "0;1",
            keySplines: "0.762 0.002 0.253 0.999",
          }}
          viewBox="0 0 200 200"
          text="loading"
          textClassName="text-[15px]"
          duration={2.5}
          textAnchor="start"
        />
      ))}
    </div>
  )
}
```


This example above also demonstrates how to use the `easingFunction` prop to create more interesting animations. Please refer to the [MDN docs](https://developer.mozilla.org/en-US/docs/Web/CSS/easing-function) on what values you can use.

Another important note here is that you have to experiment with the text length and size, to ensure the text doesn't overlap with each other, since it's not calculated automatically.

## Preserve aspect ratio

The `preserveAspectRatio` attribute controls how the SVG content scales to fit its container when their aspect ratios differ. This is determined by comparing the `viewBox` dimensions to the actual SVG container size. For example, with `preserveAspectRatio="xMidYMid meet"`, the path and text will be centered both horizontally and vertically while maintaining proportions.

Please refer to the [MDN docs](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/preserveAspectRatio) for the poossible values. In short, the default value `xMidYMid meet` will work for most cases. If you set it to `none`, the SVG container will be stretched to the container size, but will also result in a distortion of the text. Check out this behaviour on the first demo. Resize your viewport to see the difference.

## Scroll

By setting the `animationType` prop to `scroll`, you can control the animation with the scroll position of the container. For tracking the scroll position, we use the `useScroll` hook from `motion/react`.

The relevant props are:

- `scrollContainer` — a ref to the container element that the scroll animation will be driven by
- `scrollOffset` — the scroll offset range for the animation
- `scrollTransformValues` — The `scrollYProgress` value returned by `useScroll` hook ranges between 0 and 1, and this prop defines how we should map these values to the `startOffset` attribute of the text elements. It will be converted to percentage values.

Please refer to the [motion docs](https://motion.dev/docs/react-scroll-animations) for more details.


### Demo Code (text-along-path-scroll-demo.tsx)
```tsx
import { useRef } from "react"

import AnimatedPathText from "@/fancy/components/text/text-along-path"

export default function Preview() {
  const container = useRef<HTMLDivElement>(null)

  const paths = [
    "M1 254C177 219 61 -64 269 15C477 94 332 285 214 348C96 411 155 546 331 486C507 426 410 267 667 215C872.6 173.4 951.333 264.333 965 315",
    "M1 214C177 179 61 -104 269 -25C477 54 332 245 214 308C96 371 155 506 331 446C507 386 410 227 667 175C872.6 133.4 951.333 224.333 965 275",
    "M1 294C177 259 61 -24 269 55C477 134 332 325 214 388C96 451 155 586 331 526C507 466 410 307 667 255C872.6 213.4 951.333 304.333 965 355",
    "M1 174C177 139 61 -144 269 -65C477 14 332 205 214 268C96 331 155 466 331 406C507 346 410 187 667 135C872.6 93.4 951.333 184.333 965 235",
    "M1 334C177 299 61 16 269 95C477 174 332 365 214 428C96 491 155 626 331 566C507 506 410 347 667 295C872.6 253.4 951.333 344.333 965 395",
    "M1 134C177 99 61 -184 269 -105C477 -26 332 165 214 228C96 291 155 426 331 366C507 306 410 147 667 95C872.6 53.4 951.333 144.333 965 195",
    "M1 374C177 339 61 56 269 135C477 214 332 405 214 468C96 531 155 666 331 606C507 546 410 387 667 335C872.6 293.4 951.333 384.333 965 435",
    "M1 94C177 59 61 -224 269 -145C477 -66 332 125 214 188C96 251 155 386 331 326C507 266 410 107 667 55C872.6 13.4 951.333 104.333 965 155",
  ]

  // Fun text phrases for each path
  const texts = [
    "Information is expanding daily. How to get it out visually is important.",
    "The details are not the details. They make the design.",
    "There's no other product that changes function like the computer.",
    "Innovation is the outcome of a habit, not a random act.",
    "The only important thing about design is how it relates to people.",
    "Good design is obvious. Great design is transparent.",
  ]

  return (
    <div
      
      ref={container}
    >
      <div >
        <p>SCROLL DOWN</p>
      </div>
      <div >
        {paths.map((path, i) => (
          <AnimatedPathText
            key={`path-${i}`}
            path={path}
            // showPath
            scrollContainer={container}
            pathId={`flowing-path-${i}`}
            svgClassName={`absolute -left-[100px] top-0 w-[calc(100%+200px)] h-full`}
            viewBox="0 0 900 600"
            text={texts[i]}
            textClassName={`text-xl font-thin font-calendas`}
            animationType="scroll"
            scrollTransformValues={[-130, 95]}
            textAnchor="start"
          />
        ))}
      </div>
    </div>
  );
}
```


## Notes

The performance impact of the animation increases with the length and complexity of the path, especially if you're using multiple instances, so keep an eye on it :).

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| path* | `string` | - | The path to be animated |
| text* | `string` | - | The text to be animated |
| pathId | `string` | - | The ID for the path |
| pathClassName | `string` | - | Additional CSS classes for the path |
| preserveAspectRatio | `PerserveAspectRatio` | `"xMidYMid meet"` | The aspect ratio to preserve when scaling the SVG |
| showPath | `boolean` | `false` | Whether to show the path |
| width | `string | number` | `100%` | The width of the SVG container |
| height | `string | number` | `100%` | The height of the SVG container |
| viewBox | `string` | `"0 0 100 100"` | The viewBox of the SVG container |
| svgClassName | `string` | - | Additional CSS classes for the SVG container |
| textClassName | `string` | - | Additional CSS classes for the text |
| textAnchor | `"start" | "middle" | "end"` | `"start"` | The text anchor of the text |
| animationType | `"auto" | "scroll"` | `"auto"` | The animation type |
| duration | `number` | `4` | The duration of the animation in milliseconds |
| repeatCount | `number | "indefinite"` | `"indefinite"` | The number of times the animation should repeat |
| easingFunction | `{ calcMode?: string; keyTimes?: string; keySplines?: string }` | - | The easing function for the animation |
| scrollContainer | `RefObject` | - | The ref to the container element that the scroll animation will be driven by |
| scrollOffset | `UseScrollOptions["offset"]` | `["start end", "end end"]` | The scroll offset range for the animation |
| scrollTransformValues | `[number, number]` | `[0, 100]` | The scrollYProgress value returned by `useScroll` hook ranges between 0 and 1, and this prop defines how we should map these values to the `startOffset` attribute of the text elements. It will be converted to percentage values. |


---

## Carousel <a name="carousel"></a>

### Box Carousel <a name="box-carousel"></a>

### Demo Code (box-carousel-demo.tsx)
```tsx
"use client"

import { useRef, useState  } from "react"
import { Bug, BugOff } from "lucide-react"
import BoxCarousel, { 
  type BoxCarouselRef, 
  type CarouselItem, 
} from "@/fancy/components/carousel/box-carousel"
import useScreenSize from "@/hooks/use-screen-size"

// Sample carousel items with mix of images and videos
const carouselItems: CarouselItem[] = [
  {
    id: "1",
    type: "image",
    src: "https://cdn.cosmos.so/778d0640-d4b8-45b4-8bbe-862e759c231d?format=jpeg",
    alt: "Blurry poster"
  },
  {
    id: "2", 
    type: "image",
    src: "https://cdn.cosmos.so/27ac2696-1f2b-498e-8d3d-11f2dd358ab9?format=jpeg",
    alt: "Abstract blurry figure"
  },
  {
    id: "3",
    type: "image", 
    src: "https://cdn.cosmos.so/c48b739d-202d-4340-ab6b-afa34f0d7142?format=jpeg",
    alt: "Long exposure photo of a person"
  },
  {
    id: "4",
    type: "image",
    src: "https://cdn.cosmos.so/5332f9ac-7823-4635-871d-d4b3032e1c62?format=jpeg", 
    alt: "Blurry portrait photo of a person"
  },
  {
    id: "5",
    type: "image",
    src: "https://cdn.cosmos.so/d9ed937e-7c3b-4f64-a4f3-708d639f13a1?format=jpeg",
    alt: "Long exposure shots with multiple people"
  },
  {
    id: "6",
    type: "image",
    src: "https://cdn.cosmos.so/33b43e2a-da66-42d9-a0b1-08165d80b0aa?format=jpeg",
    alt: "Close up blurry photo of a person poster"
  },
  {
    id: "7",
    type: "image",
    src: "https://cdn.cosmos.so/40342df7-2ea2-4297-add2-fe17cdc62551?format=jpeg",
    alt: "Long exposure shot of a motorcyclist"
  },
]

export default function BoxCarouselDemo() {
  const carouselRef = useRef<BoxCarouselRef>(null)
  const [debug, setDebug] = useState(false)
  const screenSize = useScreenSize()

  // Responsive dimensions based on screen size
  const getCarouselDimensions = () => {
    if (screenSize.lessThan("md")) {
      return { width: 200, height: 150 }
    }
    return { width: 350, height: 250 }
  }

  const { width, height } = getCarouselDimensions()

  const handleNext = () => {
    carouselRef.current?.next()
  }

  const handlePrev = () => {
    carouselRef.current?.prev()
  }

  const handleIndexChange = (index: number) => {
    console.log('Index changed:', index)
  }

  const toggleDebug = () => {
    setDebug(!debug)
  }

  return (
    <div >
      <button
        onClick={toggleDebug}
        
        title={debug ? "Debug Mode: ON" : "Debug Mode: OFF"}
      >
        {debug ? (
          <Bug size={10} />
        ) : (
          <BugOff size={10} />
        )}
      </button>

      <div >

        <div >
          <BoxCarousel
            ref={carouselRef}
            items={carouselItems}
            width={width}
            height={height}
            direction="right"
            onIndexChange={handleIndexChange}
            debug={debug}
            enableDrag
            perspective={1000}
          />
        </div>


        <div >
          <button 
            onClick={handlePrev}
            
          >
            Prev
          </button>
          <button 
            onClick={handleNext}
            
          >
            Next
          </button>
        </div>
      </div>
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/box-carousel
```






### Component Source Code (box-carousel.tsx)
```tsx
"use client"

import React, {
  forwardRef,
  memo,
  ReactNode,
  useCallback,
  useEffect,
  useImperativeHandle,
  useMemo,
  useRef,
  useState,
} from "react"
import {
  animate,
  motion,
  useMotionValue,
  useReducedMotion,
  useSpring,
  useTransform,
  ValueAnimationOptions,
} from "motion/react"

import { cn } from "@/lib/utils"

interface CarouselItem {
  /**
   * Unique identifier for the carousel item
   */
  id: string
  /**
   * The type of media: "image" or "video"
   */
  type: "image" | "video"
  /**
   * Source URL for the image or video
   */
  src: string
  /**
   * (Optional) Alternative text for images
   */
  alt?: string
  /**
   * (Optional) Poster image for videos (displayed before playback)
   */
  poster?: string
}

/**
 * Props for a single face of the cube in the BoxCarousel.
 */
interface FaceProps {
  /**
   * The CSS transform string to position and rotate the face in 3D space.
   */
  transform: string
  /**
   * Optional additional CSS class names for the face.
   */
  className?: string
  /**
   * Optional React children to render inside the face.
   */
  children?: ReactNode
  /**
   * Optional inline styles for the face.
   */
  style?: React.CSSProperties
  /**
   * If true, enables debug mode (e.g., shows backface and opacity).
   */
  debug?: boolean
}

const CubeFace = memo(
  ({ transform, className, children, style, debug }: FaceProps) => (
    <div
      className={cn(
        "absolute overflow-hidden",
        debug && "backface-visible opacity-50",
        className
      )}
      style={{ transform, ...style }}
    >
      {children}
    </div>
  )
)

CubeFace.displayName = "CubeFace"

const MediaRenderer = memo(
  ({
    item,
    className,
    debug = false,
  }: {
    item: CarouselItem
    className?: string
    debug?: boolean
  }) => {
    if (!debug) {
      if (item.type === "video") {
        return (
          <video
            src={item.src}
            poster={item.poster}
            className={cn("w-full h-full object-cover", className)}
            muted
            loop
            autoPlay
          />
        )
      }

      return (
        <img
          src={item.src}
          alt={item.alt || ""}
          draggable={false}
          className={cn("w-full h-full object-cover", className)}
        />
      )
    }

    return (
      <div
        className={cn(
          "w-full h-full flex items-center justify-center border text-2xl",
          className
        )}
      >
        {item.id}
      </div>
    )
  }
)

MediaRenderer.displayName = "MediaRenderer"

export interface BoxCarouselRef {
  /**
   * Advance to the next item in the carousel.
   */
  next: () => void

  /**
   * Go back to the previous item in the carousel.
   */
  prev: () => void

  /**
   * Get the index of the currently visible item.
   */
  getCurrentItemIndex: () => number
}

type RotationDirection = "top" | "bottom" | "left" | "right"

interface SpringConfig {
  stiffness?: number
  damping?: number
  mass?: number
}

/**
 * Props for the BoxCarousel component
 */
interface BoxCarouselProps extends React.HTMLProps<HTMLDivElement> {
  /**
   * Array of items to display in the carousel
   */
  items: CarouselItem[]

  /**
   * Width of the carousel in pixels
   */
  width: number

  /**
   * Height of the carousel in pixels
   */
  height: number

  /**
   * Additional CSS classes for the container
   */
  className?: string

  /**
   * Enable debug mode (shows extra info/overlays)
   */
  debug?: boolean

  /**
   * Perspective value for 3D effect (in px)
   * @default 600
   */
  perspective?: number

  /**
   * The axis and direction of rotation
   * @default "vertical"
   * "top" | "bottom" | "left" | "right"
   */
  direction?: RotationDirection

  /**
   * Transition configuration for rotation animation
   * @default { duration: 1.25, ease: [0.953, 0.001, 0.019, 0.995] }
   */
  transition?: ValueAnimationOptions

  /**
   * Transition configuration for snapping after drag
   * @default { type: "spring", damping: 30, stiffness: 200 }
   */
  snapTransition?: ValueAnimationOptions

  /**
   * Spring physics config for drag interaction
   * @default { stiffness: 200, damping: 30 }
   */
  dragSpring?: SpringConfig

  /**
   * Enable auto-play mode
   * @default false
   */
  autoPlay?: boolean

  /**
   * Interval (ms) between auto-play transitions
   * @default 3000
   */
  autoPlayInterval?: number

  /**
   * Callback when the current item index changes
   */
  onIndexChange?: (index: number) => void

  /**
   * Enable drag interaction
   * @default true
   */
  enableDrag?: boolean

  /**
   * Sensitivity of drag (higher = more rotation per pixel)
   * @default 0.5
   */
  dragSensitivity?: number
}

const BoxCarousel = forwardRef<BoxCarouselRef, BoxCarouselProps>(
  (
    {
      items,
      width,
      height,
      className,
      perspective = 600,
      debug = false,
      direction = "left",
      transition = { duration: 1.25, ease: [0.953, 0.001, 0.019, 0.995] },
      snapTransition = { type: "spring", damping: 30, stiffness: 200 },
      dragSpring = { stiffness: 200, damping: 30 },
      autoPlay = false,
      autoPlayInterval = 3000,
      onIndexChange,
      enableDrag = true,
      dragSensitivity = 0.5,
      ...props
    },
    ref
  ) => {
    const [currentItemIndex, setCurrentItemIndex] = useState(0)
    const [currentFrontFaceIndex, setCurrentFrontFaceIndex] = useState(1)

    const prefersReducedMotion = useReducedMotion()

    const _transition = prefersReducedMotion ? { duration: 0 } : transition

    // 0 ⇢ will be shown if the user presses "prev"
    const [prevIndex, setPrevIndex] = useState(items.length - 1)

    // 1 ⇢ item that is currently visible
    const [currentIndex, setCurrentIndex] = useState(0)

    // 2 ⇢ will be shown on the next "next"
    const [nextIndex, setNextIndex] = useState(1)

    // 3 ⇢ two steps ahead (the face that is at the back right now)
    const [afterNextIndex, setAfterNextIndex] = useState(2)

    const [currentRotation, setCurrentRotation] = useState(0)

    const rotationCount = useRef(1)
    const isRotating = useRef(false)
    const pendingIndexChange = useRef<number | null>(null)
    const isDragging = useRef(false)
    const startPosition = useRef({ x: 0, y: 0 })
    const startRotation = useRef(0)

    const baseRotateX = useMotionValue(0)
    const baseRotateY = useMotionValue(0)

    // Use springs for smoother animation during drag
    const springRotateX = useSpring(baseRotateX, dragSpring)
    const springRotateY = useSpring(baseRotateY, dragSpring)

    const handleAnimationComplete = useCallback(
      (triggeredBy: string) => {
        if (isRotating.current && pendingIndexChange.current !== null) {
          isRotating.current = false

          let newFrontFaceIndex: number
          let currentBackFaceIndex: number

          if (triggeredBy === "next") {
            newFrontFaceIndex = (currentFrontFaceIndex + 1) % 4
            currentBackFaceIndex = (newFrontFaceIndex + 2) % 4
          } else {
            newFrontFaceIndex = (currentFrontFaceIndex - 1 + 4) % 4
            currentBackFaceIndex = (newFrontFaceIndex + 3) % 4
          }

          setCurrentItemIndex(pendingIndexChange.current)
          onIndexChange?.(pendingIndexChange.current)

          const indexOffset = triggeredBy === "next" ? 2 : -1

          if (currentBackFaceIndex === 0) {
            setPrevIndex(
              (pendingIndexChange.current + indexOffset + items.length) %
                items.length
            )
          } else if (currentBackFaceIndex === 1) {
            setCurrentIndex(
              (pendingIndexChange.current + indexOffset + items.length) %
                items.length
            )
          } else if (currentBackFaceIndex === 2) {
            setNextIndex(
              (pendingIndexChange.current + indexOffset + items.length) %
                items.length
            )
          } else if (currentBackFaceIndex === 3) {
            setAfterNextIndex(
              (pendingIndexChange.current + indexOffset + items.length) %
                items.length
            )
          }

          pendingIndexChange.current = null
          rotationCount.current++

          setCurrentFrontFaceIndex(newFrontFaceIndex)
        }
      },
      [currentFrontFaceIndex, items.length, onIndexChange]
    )

    // Drag functionality - using direct event handlers like css-box
    const handleDragStart = useCallback(
      (e: React.MouseEvent | React.TouchEvent) => {
        if (!enableDrag || isRotating.current) return

        isDragging.current = true
        const point = "touches" in e ? e.touches[0] : e
        startPosition.current = { x: point.clientX, y: point.clientY }
        startRotation.current = currentRotation

        // Prevent default to avoid text selection
        e.preventDefault()
      },
      [enableDrag, currentRotation]
    )

    const handleDragMove = useCallback(
      (e: MouseEvent | TouchEvent) => {
        if (!isDragging.current || isRotating.current) return

        const point = "touches" in e ? e.touches[0] : e
        const deltaX = point.clientX - startPosition.current.x
        const deltaY = point.clientY - startPosition.current.y

        const isVertical = direction === "top" || direction === "bottom"
        const delta = isVertical ? deltaY : deltaX
        const rotationDelta = (delta * dragSensitivity) / 2

        let newRotation = startRotation.current

        if (direction === "top" || direction === "right") {
          newRotation += rotationDelta
        } else {
          newRotation -= rotationDelta
        }

        // Constrain rotation to ±120 degrees from start position. Otherwise the index recalculation will be off. TBD - find a better solution
        const minRotation = startRotation.current - 120
        const maxRotation = startRotation.current + 120
        newRotation = Math.max(minRotation, Math.min(maxRotation, newRotation))

        // Apply the rotation immediately during drag
        if (isVertical) {
          baseRotateX.set(newRotation)
        } else {
          baseRotateY.set(newRotation)
        }
      },
      [enableDrag, direction, dragSensitivity]
    )

    const handleDragEnd = useCallback(() => {
      if (!isDragging.current) return

      isDragging.current = false

      const isVertical = direction === "top" || direction === "bottom"
      const currentValue = isVertical ? baseRotateX.get() : baseRotateY.get()

      // Calculate the nearest quarter rotation (90-degree increment)
      const quarterRotations = Math.round(currentValue / 90)
      const snappedRotation = quarterRotations * 90

      // Calculate how many steps we've moved from the original position
      const rotationDifference = snappedRotation - currentRotation
      const steps = Math.round(rotationDifference / 90)

      if (steps !== 0) {
        isRotating.current = true

        // Calculate new item index
        let newItemIndex = currentItemIndex
        for (let i = 0; i < Math.abs(steps); i++) {
          if (steps > 0) {
            newItemIndex = (newItemIndex + 1) % items.length
          } else {
            newItemIndex =
              newItemIndex === 0 ? items.length - 1 : newItemIndex - 1
          }
        }

        pendingIndexChange.current = newItemIndex

        // Animate to the snapped position
        const targetMotionValue = isVertical ? baseRotateX : baseRotateY
        animate(targetMotionValue, snappedRotation, {
          ...snapTransition,
          onComplete: () => {
            handleAnimationComplete(steps > 0 ? "next" : "prev")
            setCurrentRotation(snappedRotation)
          },
        })
      } else {
        // Snap back to current position
        const targetMotionValue = isVertical ? baseRotateX : baseRotateY
        animate(targetMotionValue, currentRotation, snapTransition)
      }
    }, [
      direction,
      baseRotateX,
      baseRotateY,
      currentRotation,
      currentItemIndex,
      items.length,
      transition,
      handleAnimationComplete,
    ])

    // Set up global event listeners for drag
    useEffect(() => {
      if (enableDrag) {
        window.addEventListener("mousemove", handleDragMove)
        window.addEventListener("mouseup", handleDragEnd)
        window.addEventListener("touchmove", handleDragMove)
        window.addEventListener("touchend", handleDragEnd)

        return () => {
          window.removeEventListener("mousemove", handleDragMove)
          window.removeEventListener("mouseup", handleDragEnd)
          window.removeEventListener("touchmove", handleDragMove)
          window.removeEventListener("touchend", handleDragEnd)
        }
      }
    }, [enableDrag, handleDragMove, handleDragEnd])

    const next = useCallback(() => {
      if (items.length === 0 || isRotating.current) return

      isRotating.current = true
      const newIndex = (currentItemIndex + 1) % items.length
      pendingIndexChange.current = newIndex

      if (direction === "top") {
        animate(baseRotateX, currentRotation + 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("next")
            setCurrentRotation(currentRotation + 90)
          },
        })
      } else if (direction === "bottom") {
        animate(baseRotateX, currentRotation - 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("next")
            setCurrentRotation(currentRotation - 90)
          },
        })
      } else if (direction === "left") {
        animate(baseRotateY, currentRotation - 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("next")
            setCurrentRotation(currentRotation - 90)
          },
        })
      } else if (direction === "right") {
        animate(baseRotateY, currentRotation + 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("next")
            setCurrentRotation(currentRotation + 90)
          },
        })
      }
    }, [items.length, direction, transition, currentRotation])

    const prev = useCallback(() => {
      if (items.length === 0 || isRotating.current) return

      isRotating.current = true
      const newIndex =
        currentItemIndex === 0 ? items.length - 1 : currentItemIndex - 1
      pendingIndexChange.current = newIndex

      if (direction === "top") {
        animate(baseRotateX, currentRotation - 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("prev")
            setCurrentRotation(currentRotation - 90)
          },
        })
      } else if (direction === "bottom") {
        animate(baseRotateX, currentRotation + 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("prev")
            setCurrentRotation(currentRotation + 90)
          },
        })
      } else if (direction === "left") {
        animate(baseRotateY, currentRotation + 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("prev")
            setCurrentRotation(currentRotation + 90)
          },
        })
      } else if (direction === "right") {
        animate(baseRotateY, currentRotation - 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("prev")
            setCurrentRotation(currentRotation - 90)
          },
        })
      }
    }, [items.length, direction, transition])

    useImperativeHandle(
      ref,
      () => ({
        next,
        prev,
        getCurrentItemIndex: () => currentItemIndex,
      }),
      [next, prev, currentItemIndex]
    )

    const depth = useMemo(
      () => (direction === "top" || direction === "bottom" ? height : width),
      [direction, width, height]
    )

    const transform = useTransform(
      isDragging.current
        ? [springRotateX, springRotateY]
        : [baseRotateX, baseRotateY],
      ([x, y]) =>
        `translateZ(-${depth / 2}px) rotateX(${x}deg) rotateY(${y}deg)`
    )

    // Determine face transforms based on the desired rotation axis
    const faceTransforms = (() => {
      switch (direction) {
        case "left":
          return [
            // left, front, right, back (rotation around Y-axis)
            `rotateY(-90deg) translateZ(${width / 2}px)`,
            `rotateY(0deg) translateZ(${depth / 2}px)`,
            `rotateY(90deg) translateZ(${width / 2}px)`,
            `rotateY(180deg) translateZ(${depth / 2}px)`,
          ]
        case "top":
          return [
            // top, front, bottom, back (rotation around X-axis)
            `rotateX(90deg) translateZ(${height / 2}px)`,
            `rotateY(0deg) translateZ(${depth / 2}px)`,
            `rotateX(-90deg) translateZ(${height / 2}px)`,
            `rotateY(180deg) translateZ(${depth / 2}px) rotateZ(180deg)`,
          ]
        case "right":
          return [
            // right, front, left, back (rotation around Y-axis)
            `rotateY(90deg) translateZ(${width / 2}px)`,
            `rotateY(0deg) translateZ(${depth / 2}px)`,
            `rotateY(-90deg) translateZ(${width / 2}px)`,
            `rotateY(180deg) translateZ(${depth / 2}px)`,
          ]
        case "bottom":
          return [
            // bottom, front, top, back (rotation around X-axis)
            `rotateX(-90deg) translateZ(${height / 2}px)`,
            `rotateY(0deg) translateZ(${depth / 2}px)`,
            `rotateX(90deg) translateZ(${height / 2}px)`,
            `rotateY(180deg) translateZ(${depth / 2}px) rotateZ(180deg)`,
          ]
        default:
          return [
            // left, front, right, back (rotation around Y-axis)
            `rotateY(-90deg) translateZ(${width / 2}px)`,
            `rotateY(0deg) translateZ(${depth / 2}px)`,
            `rotateY(90deg) translateZ(${width / 2}px)`,
            `rotateY(180deg) translateZ(${depth / 2}px)`,
          ]
      }
    })()

    // Auto play functionality
    useEffect(() => {
      if (autoPlay && items.length > 0) {
        const interval = setInterval(next, autoPlayInterval)
        return () => clearInterval(interval)
      }
    }, [autoPlay, items.length, next, autoPlayInterval])

    const handleKeyDown = useCallback(
      (e: React.KeyboardEvent) => {
        if (isRotating.current) return

        switch (e.key) {
          case "ArrowLeft":
            e.preventDefault()
            if (direction === "left" || direction === "right") {
              prev()
            }
            break
          case "ArrowRight":
            e.preventDefault()
            if (direction === "left" || direction === "right") {
              next()
            }
            break
          case "ArrowUp":
            e.preventDefault()
            if (direction === "top" || direction === "bottom") {
              prev()
            }
            break
          case "ArrowDown":
            e.preventDefault()
            if (direction === "top" || direction === "bottom") {
              next()
            }
            break
          default:
            break
        }
      },
      [direction, next, prev, items.length]
    )

    return (
      <div
        className={cn("relative focus:outline-0", enableDrag && "cursor-move", className)}
        style={{
          width,
          height,
          perspective: `${perspective}px`,
        }}
        onKeyDown={handleKeyDown}
        tabIndex={0}
        aria-label={`3D carousel with ${items.length} items`}
        aria-describedby="carousel-instructions"
        aria-live="polite"
        aria-atomic="true"
        onMouseDown={handleDragStart}
        onTouchStart={handleDragStart}
        {...props}
      >
        <div  aria-live="assertive">
          Showing item {currentItemIndex + 1} of {items.length}:{" "}
          {items[currentItemIndex]?.alt || `Item ${currentItemIndex + 1}`}
        </div>

        <motion.div
          
          style={{
            transform: transform,
          }}
        >
          {/* First face */}
          <CubeFace
            transform={faceTransforms[0]}
            style={
              debug
                ? { width, height, backgroundColor: "#ff9999" }
                : { width, height }
            }
            debug={debug}
          >
            <MediaRenderer item={items[prevIndex]} debug={debug} />
          </CubeFace>

          {/* Second face */}
          <CubeFace
            transform={faceTransforms[1]}
            style={
              debug
                ? { width, height, backgroundColor: "#99ff99" }
                : { width, height }
            }
            debug={debug}
          >
            <MediaRenderer item={items[currentIndex]} debug={debug} />
          </CubeFace>

          {/* Third face */}
          <CubeFace
            transform={faceTransforms[2]}
            style={
              debug
                ? { width, height, backgroundColor: "#9999ff" }
                : { width, height }
            }
            debug={debug}
          >
            <MediaRenderer item={items[nextIndex]} debug={debug} />
          </CubeFace>

          {/* Fourth face */}
          <CubeFace
            transform={faceTransforms[3]}
            style={
              debug
                ? { width, height, backgroundColor: "#ffff99" }
                : { width, height }
            }
            debug={debug}
          >
            <MediaRenderer item={items[afterNextIndex]} debug={debug} />
          </CubeFace>
        </motion.div>
      </div>
    )
  }
)

BoxCarousel.displayName = "BoxCarousel"

export default BoxCarousel
export type { CarouselItem, RotationDirection, SpringConfig }
```





## Usage

The Box Carousel creates a 3D rotating cube effect where each face displays a different item from your collection. You can navigate through items using mouse/touch drag, keyboard arrows, or control it via ref.

You need to pass an items array with at least 4 items, as well as the desired width and height of your items.


**File**: `High-level example`
```tsx
function MyCarousel() {
  return (
    <BoxCarousel
      items={items}
      width={400}
      height={300}
      direction="right"
      enableDrag={true}
    />
  )
}
```


## Understanding the Component

The component constructs a 3D box using four faces positioned with CSS transforms. The order of the faces depends on the rotation direction, which will be important when we update the item indices. In the following snippet you can follow through the order when the rotation direction is `left`. If you want to dive deeper into how these transforms work, check out the [CSS Box documentation](/docs/components/blocks/css-box).


**File**: `Face positioning`
```tsx
const faceTransforms = (() => {
  switch (direction) {
    case "left":
      return [
        `rotateY(-90deg) translateZ(${width / 2}px)`,  // left face
        `rotateY(0deg) translateZ(${depth / 2}px)`,    // front face
        `rotateY(90deg) translateZ(${width / 2}px)`,   // right face
        `rotateY(180deg) translateZ(${depth / 2}px)`,  // back face
      ]
    // ... other directions
  }
})()
```


The `depth` is calculated based on the rotation direction - for horizontal rotations (left/right), it uses the width, and for vertical rotations (top/bottom), it uses the height. This means all items are constrained to the same aspect ratio:


**File**: `Depth`
```tsx
const depth = useMemo(
  () => (direction === "top" || direction === "bottom" ? height : width),
  [direction, width, height]
)
```


### Rotation

The component uses Motion's `useMotionValue` for control over rotations. For each rotation, we just add or subtract 90 degrees:


**File**: `Motion values`
```tsx
//...

const baseRotateX = useMotionValue(0)  // For vertical rotations
const baseRotateY = useMotionValue(0)  // For horizontal rotations

//...

// Rotate to next face when direction is left
} else if (direction === "left") {
  animate(baseRotateY, currentRotation - 90, {
    ..._transition,
    onComplete: () => {
      handleAnimationComplete("next")
      setCurrentRotation(currentRotation - 90)
    },
  })
}
//...
```


Then, we just transform these motion values to a CSS transform and use it on the whole box container.


**File**: `3D transform`
```tsx
//...

const transform = useTransform(
  isDragging.current ? [springRotateX, springRotateY] : [baseRotateX, baseRotateY],
  ([x, y]) => `translateZ(-${depth / 2}px) rotateX(${x}deg) rotateY(${y}deg)`
)

//...

<motion.div
  
  style={{
    transform: transform,
  }}
>
```


### Item Management

The component maintains four item indices to track which items are displayed on each face. The first face (at `prevIndex`) is, by default, the last item in our items array. The second face is the current camera-facing item, which is the first item in our array. The third face is the next item in our array, and the fourth face (backward-facing face) is the next item after the next item in our array.


**File**: `Item indices`
```ts
const [prevIndex, setPrevIndex] = useState(items.length - 1)     // Face 0
const [currentIndex, setCurrentIndex] = useState(0)             // Face 1 (visible)
const [nextIndex, setNextIndex] = useState(1)                   // Face 2
const [afterNextIndex, setAfterNextIndex] = useState(2)         // Face 3
```


If our carousel only had 4 items, we could leave these indices as-is. However, with more than 4 items, we need to update the indices after each rotation so that the correct items are always displayed on each face—even after several rotations.

In practice, only the index for the backward-facing face needs to be updated after a rotation; the other three faces remain consistent. The function that handles this may look a bit tricky at first, but the logic is straightforward: after each rotation, we determine which face is now at the back and update its index to point to the next appropriate item in the array.


**File**: `Update item indices`
```ts
const handleAnimationComplete = useCallback(
  (triggeredBy: string) => {
    if (isRotating.current && pendingIndexChange.current !== null) {
      isRotating.current = false

      let newFrontFaceIndex: number
      let currentBackFaceIndex: number

      if (triggeredBy === "next") {
        newFrontFaceIndex = (currentFrontFaceIndex + 1) % 4
        currentBackFaceIndex = (newFrontFaceIndex + 2) % 4
      } else {
        newFrontFaceIndex = (currentFrontFaceIndex - 1 + 4) % 4
        currentBackFaceIndex = (newFrontFaceIndex + 3) % 4
      }

      setCurrentItemIndex(pendingIndexChange.current)
      onIndexChange?.(pendingIndexChange.current)

      const indexOffset = triggeredBy === "next" ? 2 : -1

      if (currentBackFaceIndex === 0) {
        setPrevIndex(
          (pendingIndexChange.current + indexOffset + items.length) %
            items.length
        )
      } else if (currentBackFaceIndex === 1) {
        setCurrentIndex(
          (pendingIndexChange.current + indexOffset + items.length) %
            items.length
        )
      } else if (currentBackFaceIndex === 2) {
        setNextIndex(
          (pendingIndexChange.current + indexOffset + items.length) %
            items.length
        )
      } else if (currentBackFaceIndex === 3) {
        setAfterNextIndex(
          (pendingIndexChange.current + indexOffset + items.length) %
            items.length
        )
      }

      pendingIndexChange.current = null
      rotationCount.current++

      setCurrentFrontFaceIndex(newFrontFaceIndex)
    }
  },
[currentFrontFaceIndex, items.length, onIndexChange]
)
```


### Drag Interaction

The component supports drag interaction. In the following function you can see that we're modifying the base rotation values based on the delta of the mouse/touch position:


**File**: `Drag handling`
```ts
const handleDragMove = useCallback(
  (e: MouseEvent | TouchEvent) => {
    if (!isDragging.current || isRotating.current) return

    const point = "touches" in e ? e.touches[0] : e
    const deltaX = point.clientX - startPosition.current.x
    const deltaY = point.clientY - startPosition.current.y

    const isVertical = direction === "top" || direction === "bottom"
    const delta = isVertical ? deltaY : deltaX
    const rotationDelta = (delta * dragSensitivity) / 2

    let newRotation = startRotation.current

    if (direction === "top" || direction === "right") {
      newRotation += rotationDelta
    } else {
      newRotation -= rotationDelta
    }

    // Constrain rotation to +/-120 degrees from start position. Otherwise the index recalculation will be off. TBD - find a better solution
    const minRotation = startRotation.current - 120
    const maxRotation = startRotation.current + 120
    newRotation = Math.max(minRotation, Math.min(maxRotation, newRotation))

    // Apply the rotation immediately during drag
    if (isVertical) {
      baseRotateX.set(newRotation)
    } else {
      baseRotateY.set(newRotation)
    }
  },
  [enableDrag, direction, dragSensitivity]
)
```


When the drag interaction is released, the carousel will snap back to the nearest 90-degree increment:


**File**: `Drag snap`
```ts
const handleDragEnd = useCallback(() => {
  if (!isDragging.current) return

  isDragging.current = false

  const isVertical = direction === "top" || direction === "bottom"
  const currentValue = isVertical ? baseRotateX.get() : baseRotateY.get()

  // Calculate the nearest quarter rotation (90-degree increment)
  const quarterRotations = Math.round(currentValue / 90)
  const snappedRotation = quarterRotations * 90

  // Calculate how many steps we've moved from the original position
  const rotationDifference = snappedRotation - currentRotation
  const steps = Math.round(rotationDifference / 90)

  if (steps !== 0) {
    isRotating.current = true

    // Calculate new item index
    let newItemIndex = currentItemIndex
    for (let i = 0; i < Math.abs(steps); i++) {
      if (steps > 0) {
        newItemIndex = (newItemIndex + 1) % items.length
      } else {
        newItemIndex =
          newItemIndex === 0 ? items.length - 1 : newItemIndex - 1
      }
    }

    pendingIndexChange.current = newItemIndex

    // Animate to the snapped position
    const targetMotionValue = isVertical ? baseRotateX : baseRotateY
    animate(targetMotionValue, snappedRotation, {
      ...snapTransition,
      onComplete: () => {
        handleAnimationComplete(steps > 0 ? "next" : "prev")
        setCurrentRotation(snappedRotation)
      },
    })
  } else {
    // Snap back to current position
    const targetMotionValue = isVertical ? baseRotateX : baseRotateY
    animate(targetMotionValue, currentRotation, snapTransition)
  }
}, [
  direction,
  baseRotateX,
  baseRotateY,
  currentRotation,
  currentItemIndex,
  items.length,
  transition,
  handleAnimationComplete,
])
```


You can customize the snap transition by passing in a custom value for `snapTransition` prop. The default value is `{ type: "spring", damping: 30, stiffness: 200 }`.

You can also customize the drag sensitivity and spring physics by passing in custom values for `dragSensitivity` and `dragSpring` props. The default values are `0.5` and `{ stiffness: 200, damping: 30 }` respectively.

An important note here is that the drag rotation is constrained to a +/- 120 degree range for the sake of simplicity. Otherwise we would need to re-order the whole items array to keep the correct ordering of items after a huge rotation.
Feel free to open a PR if you'd like to fix this :).

### Auto-play Mode

You can enable automatic progression through items with the `autoPlay` prop:


### Demo Code (box-carousel-autoplay-demo.tsx)
```tsx
"use client"

import { useRef, useState, useEffect } from "react"
import { Bug, BugOff } from "lucide-react"
import BoxCarousel, { 
  type BoxCarouselRef, 
  type CarouselItem, 
} from "@/fancy/components/carousel/box-carousel"
import useScreenSize from "@/hooks/use-screen-size"

// Sample carousel items with mix of images and videos
const carouselItems: CarouselItem[] = [
  {
    id: "1",
    type: "image",
    src: "https://cdn.cosmos.so/778d0640-d4b8-45b4-8bbe-862e759c231d?format=jpeg",
    alt: "Blurry poster"
  },
  {
    id: "2", 
    type: "image",
    src: "https://cdn.cosmos.so/27ac2696-1f2b-498e-8d3d-11f2dd358ab9?format=jpeg",
    alt: "Abstract blurry figure"
  },
  {
    id: "3",
    type: "image", 
    src: "https://cdn.cosmos.so/c48b739d-202d-4340-ab6b-afa34f0d7142?format=jpeg",
    alt: "Long exposure photo of a person"
  },
  {
    id: "4",
    type: "image",
    src: "https://cdn.cosmos.so/5332f9ac-7823-4635-871d-d4b3032e1c62?format=jpeg", 
    alt: "Blurry portrait photo of a person"
  },
  {
    id: "5",
    type: "image",
    src: "https://cdn.cosmos.so/d9ed937e-7c3b-4f64-a4f3-708d639f13a1?format=jpeg",
    alt: "Long exposure shots with multiple people"
  },
  {
    id: "6",
    type: "image",
    src: "https://cdn.cosmos.so/33b43e2a-da66-42d9-a0b1-08165d80b0aa?format=jpeg",
    alt: "Close up blurry photo of a person poster"
  },
  {
    id: "7",
    type: "image",
    src: "https://cdn.cosmos.so/40342df7-2ea2-4297-add2-fe17cdc62551?format=jpeg",
    alt: "Long exposure shot of a motorcyclist"
  },
]

export default function BoxCarouselDemo() {
  const carouselRef = useRef<BoxCarouselRef>(null)
  const [debug, setDebug] = useState(false)

  const screenSize = useScreenSize()

  // Responsive dimensions based on screen size
  const getCarouselDimensions = () => {
    if (screenSize.lessThan("md")) {
      return { width: 200, height: 150 }
    }
    return { width: 350, height: 250 }
  }

  const { width, height } = getCarouselDimensions()

  const handleIndexChange = (index: number) => {
    console.log('Index changed:', index)
  }

  const toggleDebug = () => {
    setDebug(!debug)
  }

  return (
    <div >
      <button
        onClick={toggleDebug}
        
        title={debug ? "Debug Mode: ON" : "Debug Mode: OFF"}
      >
        {debug ? (
          <Bug size={10} />
        ) : (
          <BugOff size={10} />
        )}
      </button>

      <div >

        <div >
          <BoxCarousel
            ref={carouselRef}
            items={carouselItems}
            width={width}
            height={height}
            direction="top"
            autoPlay
            autoPlayInterval={1500}
            onIndexChange={handleIndexChange}
            debug={debug}
            enableDrag
            perspective={1000}
          />
        </div>

      </div>
    </div>
  )
}
```


### Keyboard Navigation

The component includes full keyboard support when the carousel is in focus:

- **Arrow keys**: Navigate based on rotation direction
  - Left/Right arrows work for `left`/`right` directions
  - Up/Down arrows work for `top`/`bottom` directions

### Mixed Media Support

Supports both images and videos with different handling:


### Demo Code (box-carousel-video-demo.tsx)
```tsx
"use client"

import { useRef, useState } from "react"
import { AnimatePresence, motion } from "motion/react"

import BoxCarousel, {
  type BoxCarouselRef,
  type CarouselItem,
} from "@/fancy/components/carousel/box-carousel"
import useScreenSize from "@/hooks/use-screen-size"

// Sample carousel items with mix of images and videos
const carouselItems: CarouselItem[] = [
  {
    id: "1",
    type: "video",
    src: "https://cdn.cosmos.so/3fe9c8a8-b562-4090-a5ac-5bbdf655a938.mp4",
    alt: "@portalsandpaths",
  },
  {
    id: "2",
    type: "video",
    src: "https://cdn.cosmos.so/594e87df-e8ca-4d03-8137-f78b5dab6793.mp4",
    alt: "@studio.size",
  },
  {
    id: "3",
    type: "image",
    src: "https://cdn.cosmos.so/92162e2e-abf6-4b98-a557-bfe25a336608?format=jpeg",
    alt: "@david_wise",
  },
  {
    id: "4",
    type: "video",
    src: "https://cdn.cosmos.so/aae0a1e6-d03d-43ae-aa7f-21627a950730.mp4",
    alt: "@svenvranjes",
  },
  {
    id: "5",
    type: "image",
    src: "https://cdn.cosmos.so/7a5f1a73-ca73-466b-afaf-2b3d0639aa1a?format=jpeg",
    alt: "@deconstructie",
  },
]

export default function BoxCarouselDemo() {
  const carouselRef = useRef<BoxCarouselRef>(null)
  const [currentIndex, setCurrentIndex] = useState(0)
  const screenSize = useScreenSize()

  // Responsive dimensions based on screen size
  const getCarouselDimensions = () => {
    if (screenSize.lessThan("md")) {
      return { width: 200, height: 150 }
    }
    return { width: 350, height: 250 }
  }

  const { width, height } = getCarouselDimensions()

  const handleIndexChange = (index: number) => {
    setCurrentIndex(index)
  }

  return (
    <div >


      <div >
        <div >
          <BoxCarousel
            ref={carouselRef}
            items={carouselItems}
            width={width}
            height={height}
            direction="right"
            onIndexChange={handleIndexChange}
            enableDrag
            perspective={1000}
          />
        </div>

        <div >
          <AnimatePresence mode="popLayout">
            <motion.span
              key={currentIndex}
              layoutId="id"
              layout
              initial={{ opacity: 0, y: 0 }}
              animate={{ opacity: 1, y: 0 }}
              exit={{ opacity: 0, y: 0 }}
              transition={{ duration: 0.2, ease: "easeOut" }}
              
            >
              {carouselItems[currentIndex]?.alt}
            </motion.span>
          </AnimatePresence>
        </div>
      </div>
    </div>
  )
}
```


Videos automatically play with `muted`, `loop`, and `autoPlay` attributes. If you need more custom controls here, modify the `MediaRenderer` component.

### Imperative API

You can access carousel controls programmatically using a ref. This can be handy when you want to trigger a rotation via buttons, just like in the first demo on the page:


**File**: `Ref usage`
```tsx
function MyComponent() {
  const carouselRef = useRef<BoxCarouselRef>(null)

  const handleNext = () => {
    carouselRef.current?.next()
  }

  const handlePrev = () => {
    carouselRef.current?.prev()
  }

  const getCurrentIndex = () => {
    return carouselRef.current?.getCurrentItemIndex() ?? 0
  }

  return (
    <>
      <BoxCarousel ref={carouselRef} items={items} width={400} height={300} />
      <button onClick={handleNext}>Next</button>
      <button onClick={handlePrev}>Previous</button>
    </>
  )
}
```


### Reduced Motion Support

The component automatically respects user preferences for reduced motion by setting transition duration to 0 when `prefers-reduced-motion` is detected. The drag interaction remains intact, though.

## Credits

You can find the links for each artwork used in the demo [here](https://www.cosmos.so/danielpetho/box-carousel-demo).

Ported to Framer by [Framer University](https://framer.university/)

## Resources

- [Intro to CSS 3D transforms](https://3dtransforms.desandro.com/) by David DeSandro
- [CSS Box](/docs/components/blocks/css-box)
- [Flickity](https://flickity.metafizzy.co/) by MetaFizzy

## Props

### BoxCarousel Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| items* | `CarouselItem[]` | - | Array of items to display in the carousel |
| width* | `number` | - | Width of the carousel in pixels |
| height* | `number` | - | Height of the carousel in pixels |
| className | `string` | - | Additional CSS classes for the container |
| direction | `"top" | "bottom" | "left" | "right"` | `"left"` | The axis and direction of rotation |
| perspective | `number` | `600` | CSS perspective value for 3D effect depth |
| debug | `boolean` | `false` | Enable debug mode to visualize cube faces |
| transition | `ValueAnimationOptions` | `{ duration: 1.25, ease: [0.953, 0.001, 0.019, 0.995] }` | Animation options for programmatic rotations |
| snapTransition | `ValueAnimationOptions` | `{ type: "spring", damping: 30, stiffness: 200 }` | Animation options for drag snap-back |
| dragSpring | `SpringConfig` | `{ stiffness: 200, damping: 30 }` | Spring physics configuration for drag interactions |
| autoPlay | `boolean` | `false` | Enable automatic progression through items |
| autoPlayInterval | `number` | `3000` | Interval in milliseconds for auto-play |
| enableDrag | `boolean` | `true` | Enable drag interaction for navigation |
| dragSensitivity | `number` | `0.5` | Sensitivity multiplier for drag movement |
| onIndexChange | `(index: number) => void` | - | Callback fired when the active item changes |


### BoxCarousel Ref Methods


| Method | Type | Default | Description |
| --- | --- | --- | --- |
| goTo | `(index: number) => void` | - | Programmatically go to a specific item index |
| next | `() => void` | - | Advance to the next item |
| prev | `() => void` | - | Go to the previous item |
| getCurrentItemIndex | `() => number` | - | Get the current active item index |


---

## Background <a name="background"></a>

### Animated Gradient With SVG <a name="animated-gradient-with-svg"></a>

### Demo Code (animated-gradient-demo.tsx)
```tsx
"use client"

import React from "react"

import AnimatedGradient from "@/fancy/components/background/animated-gradient-with-svg"

interface BentoCardProps {
  title: string
  subtitle?: string
  description?: string
  buttonText?: string
  align?: "left" | "center"
}

const gradientColors = ["#FF0000", "#FF4500", "#FF9900"]

const BentoCard: React.FC<BentoCardProps> = ({
  title,
  subtitle,
  description,
  buttonText,
  align = "left",
}) => (
  <div >
    
      <AnimatedGradient colors={gradientColors} speed={10} blur="medium" />
    
    <div
      className={`relative z-10 flex-1 ${align === "center" ? "items-center text-center" : "items-start text-left"} flex flex-col justify-between w-full h-full`}
    >
      <div>
        <div >
          {title}
        </div>
        {subtitle && (
          <div >
            {subtitle}
          </div>
        )}
      </div>
      {description && (
        <div >{description}</div>
      )}
      {buttonText && (
        <button >
          {buttonText}
        </button>
      )}
    </div>
  </div>
)

const AnimatedGradientDemo: React.FC = () => {
  return (
    <div >
      <div >
        {/* Top left card */}
        <div >
          <BentoCard
            title="Animated Bento"
            subtitle="#001"
            description="Using only SVG circles and blur"
          />
        </div>
        {/* Top right card */}
        <div >
          <BentoCard title="Gradients" buttonText="Explore More"  />
        </div>
      </div>
    </div>
  )
}

export default AnimatedGradientDemo
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/animated-gradient-with-svg
```





Add this hook for querying the dimensions of an element:


*Component Source: use-dimensions (Source file not found)*


Update your `globals.css` file to include the animation keyframes:


**File**: `globals.css`
```css
  /* ... */
  @theme: {
    --animate-background-gradient: background-gradient;
    @keyframes: background-gradient {
      0%, 100% {
        transform: translate(0, 0);
        animationDelay: var(--background-gradient-delay, 0s);
      }
      20% {
        transform: translate(calc(100% * var(--tx-1, 1)), calc(100% * var(--ty-1, 1)));
      }
      40% {
        transform: translate(calc(100% * var(--tx-2, -1)), calc(100% * var(--ty-2, 1)));
      }
      60% {
        transform: translate(calc(100% * var(--tx-3, 1)), calc(100% * var(--ty-3, -1)));
      }
      80% {
        transform: translate(calc(100% * var(--tx-4, -1)), calc(100% * var(--ty-4, -1)));
      }     
    }
  },
};
```


Then, copy and paste the following code into your project:


### Component Source Code (animated-gradient-with-svg.tsx)
```tsx
"use client"

import React, { useMemo, useRef } from "react"

import { cn } from "@/lib/utils"
import { useDimensions } from "@/hooks/use-debounced-dimensions"

interface AnimatedGradientProps {
  colors: string[]
  speed?: number
  blur?: "light" | "medium" | "heavy"
}

const randomInt = (min: number, max: number) => {
  return Math.floor(Math.random() * (max - min + 1)) + min
}

const AnimatedGradient: React.FC<AnimatedGradientProps> = ({
  colors,
  speed = 5,
  blur = "light",
}) => {
  const containerRef = useRef<HTMLDivElement>(null)
  const dimensions = useDimensions(containerRef)

  const circleSize = useMemo(
    () => Math.max(dimensions.width, dimensions.height),
    [dimensions.width, dimensions.height]
  )

  const blurClass =
    blur === "light"
      ? "blur-2xl"
      : blur === "medium"
        ? "blur-3xl"
        : "blur-[100px]"

  return (
    <div ref={containerRef} >
      <div className={cn(`absolute inset-0`, blurClass)}>
        {colors.map((color, index) => {
          const animationProps = {
            animation: `background-gradient ${speed}s infinite ease-in-out`,
            animationDuration: `${speed}s`,
            top: `${Math.random() * 50}%`,
            left: `${Math.random() * 50}%`,
            "--tx-1": Math.random() - 0.5,
            "--ty-1": Math.random() - 0.5,
            "--tx-2": Math.random() - 0.5,
            "--ty-2": Math.random() - 0.5,
            "--tx-3": Math.random() - 0.5,
            "--ty-3": Math.random() - 0.5,
            "--tx-4": Math.random() - 0.5,
            "--ty-4": Math.random() - 0.5,
          } as React.CSSProperties

          return (
            <svg
              key={index}
              className={cn("absolute", "animate-background-gradient")}
              width={circleSize * randomInt(0.5, 1.5)}
              height={circleSize * randomInt(0.5, 1.5)}
              viewBox="0 0 100 100"
              style={animationProps}
            >
              <circle cx="50" cy="50" r="50" fill={color} />
            </svg>
          )
        })}
      </div>
    </div>
  )
}

export default AnimatedGradient
```





## Understanding the component

Animated gradients can be achieved with many different techniques (shaders, CSS gradients, etc.), this component uses simple SVG circles with a blur filter to create the effect.

1. For each color in the `colors` prop array, the component creates an SVG circle element
2. Each circle is given a random initial position using percentage values
3. The movement of each circle is controlled by 8 CSS variables that define target positions:
   - `--tx-1` and `--ty-1` for the first position
   - `--tx-2` and `--ty-2` for the second position
   - And so on for positions 3 and 4
4. These variables are set to random values between -0.5 and 0.5.


**File**: `Movement variables`
```tsx
style={
  {
    //...
    "--tx-1": (Math.random() - 0.5),
    "--ty-1": (Math.random() - 0.5),
    "--tx-2": (Math.random() - 0.5),
    "--ty-2": (Math.random() - 0.5),
    "--tx-3": (Math.random() - 0.5),
    "--ty-3": (Math.random() - 0.5),
    "--tx-4": (Math.random() - 0.5),
    "--ty-4": (Math.random() - 0.5),
  } as React.CSSProperties
}
```


5. The `background-gradient` animation keyframes are used to animate the circles between these positions
6. Lastly, we blur the container element which holds the circles, to create a smooth effect.

If you would like to achieve a more complex animation, you have to edit the component directly, for example:

1. Add more keyframe positions by increasing the number of `--tx` and `--ty` variables
2. Use cubic-bezier timing functions to create non-linear movement
3. Add rotation or scaling transforms

and so on.

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| colors* | `string[]` | - | Array of color strings to be used in the gradient |
| speed | `number` | `5` | Speed of the animation (this is somewhat an arbitrary number, refer         tothe source code for more details) |
| blur | `"light" | "medium" | "heavy"` | `"light"` | Intensity of the blur effect |


---

### Pixel Trail <a name="pixel-trail"></a>

### Demo Code (pixel-trail-demo.tsx)
```tsx
import useScreenSize from "@/hooks/use-screen-size"
import PixelTrail from "@/fancy/components/background/pixel-trail"

const PixelTrailDemo: React.FC = () => {
  const screenSize = useScreenSize()

  return (
    <div >
      <div >
        <PixelTrail
          pixelSize={screenSize.lessThan(`md`) ? 16 : 24}
          fadeDuration={500}
          pixelClassName="bg-white"
        />
      </div>

      <div >
        <h2 >
          FANCYCOMPONENTS.DEV
        </h2>
        <p >
          Make the web fun again.
        </p>
      </div>
    </div>
  )
}

export default PixelTrailDemo
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/pixel-trail
```





Install the `uuid` package:


```bash
# Install command:
@types/uuid uuid
```


We use the uuid package to generate a unique ID for each trail instance — so each trail will work without interfering with other effects.

Then, copy and paste the component source code into your project:


### Component Source Code (pixel-trail.tsx)
```tsx
"use client"

import React, { useCallback, useMemo, useRef } from "react"
import { motion, useAnimationControls } from "motion/react"
import { v4 as uuidv4 } from "uuid"

import { cn } from "@/lib/utils"
import { useDimensions } from "@/hooks/use-dimensions"

interface PixelTrailProps {
  pixelSize: number // px
  fadeDuration?: number // ms
  delay?: number // ms
  className?: string
  pixelClassName?: string
}

const PixelTrail: React.FC<PixelTrailProps> = ({
  pixelSize = 20,
  fadeDuration = 500,
  delay = 0,
  className,
  pixelClassName,
}) => {
  const containerRef = useRef<HTMLDivElement>(null)
  const dimensions = useDimensions(containerRef)
  const trailId = useRef(uuidv4())

  const handleMouseMove = useCallback(
    (e: React.MouseEvent<HTMLDivElement>) => {
      if (!containerRef.current) return

      const rect = containerRef.current.getBoundingClientRect()
      const x = Math.floor((e.clientX - rect.left) / pixelSize)
      const y = Math.floor((e.clientY - rect.top) / pixelSize)

      const pixelElement = document.getElementById(
        `${trailId.current}-pixel-${x}-${y}`
      )
      if (pixelElement) {
        const animatePixel = (pixelElement as any).__animatePixel
        if (animatePixel) animatePixel()
      }
    },
    [pixelSize]
  )

  const columns = useMemo(
    () => Math.ceil(dimensions.width / pixelSize),
    [dimensions.width, pixelSize]
  )
  const rows = useMemo(
    () => Math.ceil(dimensions.height / pixelSize),
    [dimensions.height, pixelSize]
  )

  return (
    <div
      ref={containerRef}
      className={cn(
        "absolute inset-0 w-full h-full pointer-events-auto",
        className
      )}
      onMouseMove={handleMouseMove}
    >
      {Array.from({ length: rows }).map((_, rowIndex) => (
        <div key={rowIndex} >
          {Array.from({ length: columns }).map((_, colIndex) => (
            <PixelDot
              key={`${colIndex}-${rowIndex}`}
              id={`${trailId.current}-pixel-${colIndex}-${rowIndex}`}
              size={pixelSize}
              fadeDuration={fadeDuration}
              delay={delay}
              className={pixelClassName}
            />
          ))}
        </div>
      ))}
    </div>
  )
}

export default PixelTrail

interface PixelDotProps {
  id: string
  size: number
  fadeDuration: number
  delay: number
  className?: string
}

const PixelDot: React.FC<PixelDotProps> = React.memo(
  ({ id, size, fadeDuration, delay, className }) => {
    const controls = useAnimationControls()

    const animatePixel = useCallback(() => {
      controls.start({
        opacity: [1, 0],
        transition: { duration: fadeDuration / 1000, delay: delay / 1000 },
      })
    }, [])

    // Attach the animatePixel function to the DOM element
    const ref = useCallback(
      (node: HTMLDivElement | null) => {
        if (node) {
          ;(node as any).__animatePixel = animatePixel
        }
      },
      [animatePixel]
    )

    return (
      <motion.div
        id={id}
        ref={ref}
        className={cn("cursor-pointer-none", className)}
        style={{
          width: `${size}px`,
          height: `${size}px`,
        }}
        initial={{ opacity: 0 }}
        animate={controls}
        exit={{ opacity: 0 }}
      />
    )
  }
)

PixelDot.displayName = "PixelDot"
```





## Usage

Just drop the `PixelTrail` component into your project, define a pixelSize, and pass the `pixelColor` prop. You can also pass a `className` prop to style the container, and a `pixelClassName` prop to style the individual pixels.

## Examples

### Without fading

If you set the `fadeDuration` prop to `0`, and increase the `delay` prop, you can create a trail effect that doesn't fade.


### Demo Code (pixel-trail-no-fade-demo.tsx)
```tsx
import Image from "next/image"

import PixelTrail from "@/fancy/components/background/pixel-trail"

const PixelTrailDemo: React.FC = () => {
  return (
    <div >
      <Image
        src="https://images.unsplash.com/photo-1562016600-ece13e8ba570?q=80&w=2838&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
        alt="water surface"
        fill
        
      />
      <div >
        <PixelTrail
          pixelSize={20}
          delay={130}
          fadeDuration={0}
          pixelClassName="bg-[#f1ff76]"
        />
      </div>

      <ul >
        <a>WATER SUPPLY CO.</a>
        <a
          href=""
          
        >
          Menu
        </a>
      </ul>
      <div >
        <div >
          <h2 >100%</h2>
          <h2 >purity</h2>
        </div>
        <p >
          {
            "we deliver more than just hydration — we offer nature's purest refreshment, untouched by modern contaminants. Our water is sourced from deep, protected aquifers and naturally filtered through ancient rock layers, with unmatched clarity and taste."
          }
        </p>
      </div>
    </div>
  )
}

export default PixelTrailDemo
```


### Customizing the pixels

You can customize the individual pixels by passing a `pixelClassName` prop.


### Demo Code (pixel-trail-custom-pixel-demo.tsx)
```tsx
import useScreenSize from "@/hooks/use-screen-size"
import PixelTrail from "@/fancy/components/background/pixel-trail"

const PixelTrailDemo: React.FC = () => {
  const screenSize = useScreenSize()

  return (
    <div >
      <div >
        <PixelTrail
          pixelSize={screenSize.lessThan(`md`) ? 14 : 20}
          fadeDuration={0}
          delay={600}
          pixelClassName="rounded-full bg-"
        />
      </div>

      <div >
        <h2 >
          fancy ✽ components{" "}
        </h2>
        <p >
          with react, motion, and typrscript.
        </p>
      </div>
    </div>
  )
}

export default PixelTrailDemo
```


### Combining with SVG Filters

The following example combines the [GooeyFilter](/docs/components/filter/gooey-filter) component with the PixelTrail component to create a fluid interface. Unfortunately, the component doesn't support Safari, so you'll need to create a fallback for that.


### Demo Code (gooey-svg-filter-pixel-trail-demo.tsx)
```tsx
import useDetectBrowser from "@/hooks/use-detect-browser"
import useScreenSize from "@/hooks/use-screen-size"
import PixelTrail from "@/fancy/components/background/pixel-trail"
import GooeySvgFilter from "@/fancy/components/filter/gooey-svg-filter"

export default function GooeyDemo() {
  const screenSize = useScreenSize()
  const browserName = useDetectBrowser()
  const isSafari = browserName === "Safari"

  return (
    <div >
      <img
        src="https://images.aiscribbles.com/34fe5695dbc942628e3cad9744e8ae13.png?v=60d084"
        alt="impressionist painting"
        
      />

      <GooeySvgFilter id="gooey-filter-pixel-trail" strength={5} />

      <div
        
        style={{ filter: isSafari ? "none" : "url(#gooey-filter-pixel-trail)" }}
      >
        <PixelTrail
          pixelSize={screenSize.lessThan(`md`) ? 24 : 32}
          fadeDuration={0}
          delay={500}
          pixelClassName="bg-white"
        />
      </div>

      <p >
        Speaking things into existence
        
      </p>
    </div>
  )
}
```


## Notes

1. The component operates by dividing the container into a grid of pixels and dynamically recoloring them as you move your cursor. Each pixel is represented by a single div element, so perf may be impacted when using a large number of pixels, especially on the first render.

2. Keep the z-index of the effect's container lower than the rest of your content, so the pointer-events will captured by all of your other elements.

## Credits

Ported to Framer by [Framer University](https://framer.university/)

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| pixelSize | `number` | `20` | Size of each pixel in pixels |
| fadeDuration | `number` | `500` | Duration of the fade animation in milliseconds |
| delay | `number` | `0` | Delay before the fade animation starts in milliseconds |
| className | `string` | - | Additional CSS classes for styling |
| pixelClassName | `string` | - | Additional CSS classes for styling the individual pixels |


---

## Physics <a name="physics"></a>

### Elastic Line <a name="elastic-line"></a>

### Demo Code (elastic-line-demo.tsx)
```tsx
"use client"

import { motion, Variants } from "motion/react"

import ElasticLine from "@/fancy/components/physics/elastic-line"

export default function Preview() {
  const textVariants = {
    hidden: { opacity: 0, y: 10 },
    visible: (i: number) => ({
      opacity: 1,
      y: 0,
      transition: {
        delay: i * 0.3,
        type: "spring",
        stiffness: 300,
        damping: 30,
      },
    }),
  }

  return (
    <div >
      <div >
        {/* Animated elastic line */}
        <ElasticLine
          releaseThreshold={50}
          strokeWidth={1}
          animateInTransition={{
            type: "spring",
            stiffness: 300,
            damping: 30,
            delay: 0.15,
          }}
        />
      </div>

      {/* This is just fluff for the demo */}
      <div >
        <div >
          <motion.p
            variants={textVariants as Variants}
            initial="hidden"
            animate="visible"
            
            custom={0}
          >
            FANCY COMPONENTS
          </motion.p>
        </div>
        <div >
          <motion.p
            variants={textVariants as Variants}
            initial="hidden"
            animate="visible"
            
            custom={0}
          >
            ✽
          </motion.p>
          <motion.p
            variants={textVariants as Variants}
            initial="hidden"
            animate="visible"
            
            custom={1}
          >
            Ready to use, fancy, animated React components & microinteractions
            for creative developers.
          </motion.p>
        </div>

        {/* <div >
          <motion.p
            variants={textVariants}
            initial="hidden"
            animate="visible"
            custom={2}
          >
            
          </motion.p>
        </div> */}
      </div>
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/elastic-line
```





Create a hook for querying the cursor position:


*Component Source: use-mouse-position (Source file not found)*


And a hook for querying the dimensions of an element:


*Component Source: use-dimensions (Source file not found)*


For better readability, there is another hook for getting the elastic line's control point, and if the line is grabbed or not:


*Component Source: use-elastic-line-events (Source file not found)*


Then, copy and paste the component code into your project, and update your imports:


### Component Source Code (elastic-line.tsx)
```tsx
"use client"

import React, { useEffect, useRef, useState } from "react"
import {
  animate,
  motion,
  useAnimationFrame,
  useMotionValue,
  ValueAnimationTransition,
} from "motion/react"

import { useDimensions } from "@/hooks/use-dimensions"
import { useElasticLineEvents } from "@/hooks/use-elastic-line-events"

interface ElasticLineProps {
  isVertical?: boolean
  grabThreshold?: number
  releaseThreshold?: number
  strokeWidth?: number
  transition?: ValueAnimationTransition
  animateInTransition?: ValueAnimationTransition
  className?: string
}

const ElasticLine: React.FC<ElasticLineProps> = ({
  isVertical = false,
  grabThreshold = 5,
  releaseThreshold = 100,
  strokeWidth = 1,
  transition = {
    type: "spring",
    stiffness: 300,
    damping: 5,
  },
  animateInTransition = {
    duration: 0.3,
    ease: "easeInOut",
  },
  className,
}) => {
  const containerRef = useRef<SVGSVGElement>(null)
  const dimensions = useDimensions(containerRef)
  const pathRef = useRef<SVGPathElement>(null)
  const [hasAnimatedIn, setHasAnimatedIn] = useState(false)

  // Clamp releaseThreshold to container dimensions
  const clampedReleaseThreshold = Math.min(
    releaseThreshold,
    isVertical ? dimensions.width / 2 : dimensions.height / 2
  )

  const { isGrabbed, controlPoint } = useElasticLineEvents(
    containerRef,
    isVertical,
    grabThreshold,
    clampedReleaseThreshold
  )

  const x = useMotionValue(dimensions.width / 2)
  const y = useMotionValue(dimensions.height / 2)
  const pathLength = useMotionValue(0)

  useEffect(() => {
    // Initial draw animation
    if (!hasAnimatedIn && dimensions.width > 0 && dimensions.height > 0) {
      animate(pathLength, 1, {
        ...animateInTransition,
        onComplete: () => setHasAnimatedIn(true),
      })
    }
    x.set(dimensions.width / 2)
    y.set(dimensions.height / 2)
  }, [dimensions, hasAnimatedIn])

  useEffect(() => {
    if (!isGrabbed && hasAnimatedIn) {
      animate(x, dimensions.width / 2, transition)
      animate(y, dimensions.height / 2, transition)
    }
  }, [isGrabbed])

  useAnimationFrame(() => {
    if (isGrabbed) {
      x.set(controlPoint.x)
      y.set(controlPoint.y)
    }

    const controlX = hasAnimatedIn ? x.get() : dimensions.width / 2
    const controlY = hasAnimatedIn ? y.get() : dimensions.height / 2

    pathRef.current?.setAttribute(
      "d",
      isVertical
        ? `M${dimensions.width / 2} 0Q${controlX} ${controlY} ${
            dimensions.width / 2
          } ${dimensions.height}`
        : `M0 ${dimensions.height / 2}Q${controlX} ${controlY} ${
            dimensions.width
          } ${dimensions.height / 2}`
    )
  })

  return (
    <svg
      ref={containerRef}
      className={`w-full h-full ${className}`}
      viewBox={`0 0 ${dimensions.width} ${dimensions.height}`}
      preserveAspectRatio="none"
    >
      <motion.path
        ref={pathRef}
        stroke="currentColor"
        strokeWidth={strokeWidth}
        initial={{ pathLength: 0 }}
        style={{ pathLength }}
        fill="none"
      />
    </svg>
  )
}

export default ElasticLine
```





## Understanding the component

This component is made with a simple svg quadratic curve, with 2+1 points. The start and end points of the curve positioned at the two edges of the parent container, either horizontally or vertically, depending on the `isVertical` prop. This means, the line will always be centered in the container, and it will always fill up the entire container, so make sure to position your container properly.

The third point of the line is the control point, named `Q`, which is positioned at the center of the container by default. When the cursor moves close to the line (within `grabThreshold`), the control point will be controlled by the cursor's position. When the distance between them is greater than the `releaseThreshold` prop, the control point is animated back to the center of the container, with the help of motion's `animate` function.

For better readability — the calculation of the control point's position, and the signal it's grabbed — done in a separate hook, called `useElasticLineEvents`.

To achiave the elastic effect we use a springy transition by default, but feel free to experiment with other type of animations, easings, durations, etc.

The compoment also have an `animateInTransition` prop, which is used when the line is initially rendered. If you want to skip this, just set the transiton's `duration` to `0`.

## Resources

- [MDN Web Docs for SVG Quadratic Curve](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths)
- [Motion docs for SVG paths](https://www.framer.com/motion/component/#%23%23svg-line-drawing/)

## Credits

Ported to Framer by [Framer University](https://framer.university/)

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| isVertical | `boolean` | `false` | Whether the line is vertical or horizontal |
| grabThreshold | `number` | `5` | The distance threshold for grabbing the line |
| releaseThreshold | `number` | `100` | The distance threshold for releasing the line |
| strokeWidth | `number` | `1` | The width of the line stroke |
| transition | `Transition` | `{ type: "spring", stiffness: 400, damping: 5, delay: 0 }` | The transition object of the line. Refer to motion docs for more details |
| animateInTransition | `Transition` | `{ type: "spring", stiffness: 400, damping: 5, delay: 0 }` | The transition object of the line when it is initially rendered. Refer to motion docs for more details |
| className | `string` | - | Additional CSS classes for styling on the svg container |


---

### Gravity <a name="gravity"></a>

### Demo Code (gravity-demo.tsx)
```tsx
import Gravity, { MatterBody } from "@/fancy/components/physics/gravity"

export default function Preview() {
  return (
    <div >
      <div >
        fancy
      </div>
      <p >
        components made with:
      </p>
      <Gravity gravity={{ x: 0, y: 1 }} >
        <MatterBody
          matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
          x="30%"
          y="10%"
        >
          <div >
            react
          </div>
        </MatterBody>
        <MatterBody
          matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
          x="30%"
          y="30%"
        >
          <div >
            typescript
          </div>
        </MatterBody>
        <MatterBody
          matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
          x="40%"
          y="20%"
          angle={10}
        >
          <div >
            motion
          </div>
        </MatterBody>
        <MatterBody
          matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
          x="75%"
          y="10%"
        >
          <div >
            tailwind
          </div>
        </MatterBody>
        <MatterBody
          matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
          x="80%"
          y="20%"
        >
          <div >
            drei
          </div>
        </MatterBody>
        <MatterBody
          matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
          x="50%"
          y="10%"
        >
          <div >
            matter-js
          </div>
        </MatterBody>
      </Gravity>
    </div>
  )
}
```


Setting up Matter.js for creating physics-based animations can be a bit tricky and cumbersome, especially with React. This component simplifies the process by wrapping your content with a physics world, and transforming your React components into Matter.js bodies.

## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/gravity
```





First, install the following dependencies:


```bash
# Install command:
matter-js @types/matter-js poly-decomp svg-path-commander
```


We use the `matter-js` library to create the physics simulation. The `poly-decomp` package is used to decompose bodies into a set of vertices, which is required for the `svg` body type. The `svg-path-commander` package is used to parse SVG paths and convert them into a set of vertices (since the built-in feature for this in `matter-js` is outdated).

Then, create an utility function for parsing SVG paths into a set of vertices:


*Component Source: svg-path-to-vertices (Source file not found)*


And another one for calculating the position of an element based on its container, and a posiiton value:


*Component Source: calculate-position (Source file not found)*


Then, copy the component source code:


### Component Source Code (gravity.tsx)
```tsx
"use client"

import {
  createContext,
  forwardRef,
  ReactNode,
  useCallback,
  useContext,
  useEffect,
  useImperativeHandle,
  useRef,
  useState,
} from "react"
import { calculatePosition } from "@/utils/calculate-position"
import { parsePathToVertices } from "@/utils/svg-path-to-vertices"
import { debounce } from "lodash"
import Matter, {
  Bodies,
  Common,
  Engine,
  Events,
  Mouse,
  MouseConstraint,
  Query,
  Render,
  Runner,
  World,
} from "matter-js"

import { cn } from "@/lib/utils"

type GravityProps = {
  children: ReactNode
  debug?: boolean
  gravity?: { x: number; y: number }
  resetOnResize?: boolean
  grabCursor?: boolean
  addTopWall?: boolean
  autoStart?: boolean
  className?: string
}

type PhysicsBody = {
  element: HTMLElement
  body: Matter.Body
  props: MatterBodyProps
}

type MatterBodyProps = {
  children: ReactNode
  matterBodyOptions?: Matter.IBodyDefinition
  isDraggable?: boolean
  bodyType?: "rectangle" | "circle" | "svg"
  sampleLength?: number
  x?: number | string
  y?: number | string
  angle?: number
  className?: string
}

export type GravityRef = {
  start: () => void
  stop: () => void
  reset: () => void
}

const GravityContext = createContext<{
  registerElement: (
    id: string,
    element: HTMLElement,
    props: MatterBodyProps
  ) => void
  unregisterElement: (id: string) => void
} | null>(null)

export const MatterBody = ({
  children,
  className,
  matterBodyOptions = {
    friction: 0.1,
    restitution: 0.1,
    density: 0.001,
    isStatic: false,
  },
  bodyType = "rectangle",
  isDraggable = true,
  sampleLength = 15,
  x = 0,
  y = 0,
  angle = 0,
  ...props
}: MatterBodyProps) => {
  const elementRef = useRef<HTMLDivElement>(null)
  const idRef = useRef(Math.random().toString(36).substring(7))
  const context = useContext(GravityContext)

  useEffect(() => {
    if (!elementRef.current || !context) return
    context.registerElement(idRef.current, elementRef.current, {
      children,
      matterBodyOptions,
      bodyType,
      sampleLength,
      isDraggable,
      x,
      y,
      angle,
      ...props,
    })

    return () => context.unregisterElement(idRef.current)
  }, [props, children, matterBodyOptions, isDraggable])

  return (
    <div
      ref={elementRef}
      className={cn(
        "absolute",
        className,
        isDraggable && "pointer-events-none"
      )}
    >
      {children}
    </div>
  )
}

const Gravity = forwardRef<GravityRef, GravityProps>(
  (
    {
      children,
      debug = false,
      gravity = { x: 0, y: 1 },
      grabCursor = true,
      resetOnResize = true,
      addTopWall = true,
      autoStart = true,
      className,
      ...props
    },
    ref
  ) => {
    const canvas = useRef<HTMLDivElement>(null)
    const engine = useRef(Engine.create())
    const render = useRef<Render>(undefined)
    const runner = useRef<Runner>(undefined)
    const bodiesMap = useRef(new Map<string, PhysicsBody>())
    const frameId = useRef<number>(undefined)
    const mouseConstraint = useRef<Matter.MouseConstraint>(undefined)
    const mouseDown = useRef(false)
    const [canvasSize, setCanvasSize] = useState({ width: 0, height: 0 })

    const isRunning = useRef(false)

    // Register Matter.js body in the physics world
    const registerElement = useCallback(
      (id: string, element: HTMLElement, props: MatterBodyProps) => {
        if (!canvas.current) return
        const width = element.offsetWidth
        const height = element.offsetHeight
        const canvasRect = canvas.current!.getBoundingClientRect()

        const angle = (props.angle || 0) * (Math.PI / 180)

        const x = calculatePosition(props.x, canvasRect.width, width)
        const y = calculatePosition(props.y, canvasRect.height, height)

        let body
        if (props.bodyType === "circle") {
          const radius = Math.max(width, height) / 2
          body = Bodies.circle(x, y, radius, {
            ...props.matterBodyOptions,
            angle: angle,
            render: {
              fillStyle: debug ? "#888888" : "#00000000",
              strokeStyle: debug ? "#333333" : "#00000000",
              lineWidth: debug ? 3 : 0,
            },
          })
        } else if (props.bodyType === "svg") {
          const paths = element.querySelectorAll("path")
          const vertexSets: Matter.Vector[][] = []

          paths.forEach((path) => {
            const d = path.getAttribute("d")
            const p = parsePathToVertices(d!, props.sampleLength)
            vertexSets.push(p)
          })

          body = Bodies.fromVertices(x, y, vertexSets, {
            ...props.matterBodyOptions,
            angle: angle,
            render: {
              fillStyle: debug ? "#888888" : "#00000000",
              strokeStyle: debug ? "#333333" : "#00000000",
              lineWidth: debug ? 3 : 0,
            },
          })
        } else {
          body = Bodies.rectangle(x, y, width, height, {
            ...props.matterBodyOptions,
            angle: angle,
            render: {
              fillStyle: debug ? "#888888" : "#00000000",
              strokeStyle: debug ? "#333333" : "#00000000",
              lineWidth: debug ? 3 : 0,
            },
          })
        }

        if (body) {
          World.add(engine.current.world, [body])
          bodiesMap.current.set(id, { element, body, props })
        }
      },
      [debug]
    )

    // Unregister Matter.js body from the physics world
    const unregisterElement = useCallback((id: string) => {
      const body = bodiesMap.current.get(id)
      if (body) {
        World.remove(engine.current.world, body.body)
        bodiesMap.current.delete(id)
      }
    }, [])

    // Keep react elements in sync with the physics world
    const updateElements = useCallback(() => {
      bodiesMap.current.forEach(({ element, body }) => {
        const { x, y } = body.position
        const rotation = body.angle * (180 / Math.PI)

        element.style.transform = `translate(${
          x - element.offsetWidth / 2
        }px, ${y - element.offsetHeight / 2}px) rotate(${rotation}deg)`
      })

      frameId.current = requestAnimationFrame(updateElements)
    }, [])

    const initializeRenderer = useCallback(() => {
      if (!canvas.current) return

      const height = canvas.current.offsetHeight
      const width = canvas.current.offsetWidth

      Common.setDecomp(require("poly-decomp"))

      engine.current.gravity.x = gravity.x
      engine.current.gravity.y = gravity.y

      render.current = Render.create({
        element: canvas.current,
        engine: engine.current,
        options: {
          width,
          height,
          wireframes: false,
          background: "#00000000",
        },
      })

      const mouse = Mouse.create(render.current.canvas)
      mouseConstraint.current = MouseConstraint.create(engine.current, {
        mouse: mouse,
        constraint: {
          stiffness: 0.2,
          render: {
            visible: debug,
          },
        },
      })

      // Add walls
      const walls = [
        // Floor
        Bodies.rectangle(width / 2, height + 10, width, 20, {
          isStatic: true,
          friction: 1,
          render: {
            visible: debug,
          },
        }),

        // Right wall
        Bodies.rectangle(width + 10, height / 2, 20, height, {
          isStatic: true,
          friction: 1,
          render: {
            visible: debug,
          },
        }),

        // Left wall
        Bodies.rectangle(-10, height / 2, 20, height, {
          isStatic: true,
          friction: 1,
          render: {
            visible: debug,
          },
        }),
      ]

      const topWall = addTopWall
        ? Bodies.rectangle(width / 2, -10, width, 20, {
            isStatic: true,
            friction: 1,
            render: {
              visible: debug,
            },
          })
        : null

      if (topWall) {
        walls.push(topWall)
      }

      const touchingMouse = () =>
        Query.point(
          engine.current.world.bodies,
          mouseConstraint.current?.mouse.position || { x: 0, y: 0 }
        ).length > 0

      if (grabCursor) {
        Events.on(engine.current, "beforeUpdate", (event) => {
          if (canvas.current) {
            if (!mouseDown.current && !touchingMouse()) {
              canvas.current.style.cursor = "default"
            } else if (touchingMouse()) {
              canvas.current.style.cursor = mouseDown.current
                ? "grabbing"
                : "grab"
            }
          }
        })

        canvas.current.addEventListener("mousedown", (event) => {
          mouseDown.current = true

          if (canvas.current) {
            if (touchingMouse()) {
              canvas.current.style.cursor = "grabbing"
            } else {
              canvas.current.style.cursor = "default"
            }
          }
        })
        canvas.current.addEventListener("mouseup", (event) => {
          mouseDown.current = false

          if (canvas.current) {
            if (touchingMouse()) {
              canvas.current.style.cursor = "grab"
            } else {
              canvas.current.style.cursor = "default"
            }
          }
        })
      }

      World.add(engine.current.world, [mouseConstraint.current, ...walls])

      render.current.mouse = mouse

      runner.current = Runner.create()
      Render.run(render.current)
      updateElements()
      runner.current.enabled = false

      if (autoStart) {
        runner.current.enabled = true
        startEngine()
      }
    }, [updateElements, debug, autoStart])

    // Clear the Matter.js world
    const clearRenderer = useCallback(() => {
      if (frameId.current) {
        cancelAnimationFrame(frameId.current)
      }

      if (mouseConstraint.current) {
        World.remove(engine.current.world, mouseConstraint.current)
      }

      if (render.current) {
        Mouse.clearSourceEvents(render.current.mouse)
        Render.stop(render.current)
        render.current.canvas.remove()
      }

      if (runner.current) {
        Runner.stop(runner.current)
      }

      if (engine.current) {
        World.clear(engine.current.world, false)
        Engine.clear(engine.current)
      }

      bodiesMap.current.clear()
    }, [])

    const handleResize = useCallback(() => {
      if (!canvas.current || !resetOnResize) return

      const newWidth = canvas.current.offsetWidth
      const newHeight = canvas.current.offsetHeight

      setCanvasSize({ width: newWidth, height: newHeight })

      // Clear and reinitialize
      clearRenderer()
      initializeRenderer()
    }, [clearRenderer, initializeRenderer, resetOnResize])

    const startEngine = useCallback(() => {
      if (runner.current) {
        runner.current.enabled = true

        Runner.run(runner.current, engine.current)
      }
      if (render.current) {
        Render.run(render.current)
      }
      frameId.current = requestAnimationFrame(updateElements)
      isRunning.current = true
    }, [updateElements, canvasSize])

    const stopEngine = useCallback(() => {
      if (!isRunning.current) return

      if (runner.current) {
        Runner.stop(runner.current)
      }
      if (render.current) {
        Render.stop(render.current)
      }
      if (frameId.current) {
        cancelAnimationFrame(frameId.current)
      }
      isRunning.current = false
    }, [])

    const reset = useCallback(() => {
      stopEngine()
      bodiesMap.current.forEach(({ element, body, props }) => {
        body.angle = props.angle || 0

        const x = calculatePosition(
          props.x,
          canvasSize.width,
          element.offsetWidth
        )
        const y = calculatePosition(
          props.y,
          canvasSize.height,
          element.offsetHeight
        )
        body.position.x = x
        body.position.y = y
      })
      updateElements()
      handleResize()
    }, [])

    useImperativeHandle(
      ref,
      () => ({
        start: startEngine,
        stop: stopEngine,
        reset,
      }),
      [startEngine, stopEngine]
    )

    useEffect(() => {
      if (!resetOnResize) return

      const debouncedResize = debounce(handleResize, 500)
      window.addEventListener("resize", debouncedResize)

      return () => {
        window.removeEventListener("resize", debouncedResize)
        debouncedResize.cancel()
      }
    }, [handleResize, resetOnResize])

    useEffect(() => {
      initializeRenderer()
      return clearRenderer
    }, [initializeRenderer, clearRenderer])

    return (
      <GravityContext.Provider value={{ registerElement, unregisterElement }}>
        <div
          ref={canvas}
          className={cn(className, "absolute top-0 left-0 w-full h-full")}
          {...props}
        >
          {children}
        </div>
      </GravityContext.Provider>
    )
  }
)

Gravity.displayName = "Gravity"
export default Gravity
```





## Usage

First, you need to wrap your scene / content with the `Gravity` component. Set the gravity direction vector in the `gravity` prop (default is `{x: 0, y: 1}`). Then, in order to transform your regular HTML elements into Matter bodies, you need to wrap them with the `MatterBody` component. 

You need to set each bodies `x` and `y` position, either as a percentage of your container size, or as a number. You do not need to set the width and height manually, everything else is taken care of by component :). High-level example:


**File**: `Gravity usage example`
```tsx
<Gravity>
  <MatterBody x="50%" y="50%">
    <div>Hello world!</div>
  </MatterBody>
  <MatterBody x="10%" y="10%">
    <div>fancy!</div>
  </MatterBody>
</Gravity>
```


## Understanding the component

### Gravity component

At its core, the Gravity component creates and manages a Matter.js physics world. It handles:

1. **Physics Setup**: Creates a canvas and initializes the Matter.js physics engine with:

   - A physics engine to calculate forces and collisions
   - A renderer to visualize the physics (when debug mode is enabled)
   - A runner to step the physics simulation forward
   - Mouse constraints to enable dragging of elements

2. **Animation Loop**: Continuously updates the positions of your HTML elements to match their physics bodies in the Matter.js world. This creates the illusion that your DOM elements are actually affected by physics.

3. **Controls**: Exposes three main methods:

   - `start()`: Begins the physics simulation
   - `stop()`: Pauses the physics simulation
   - `reset()`: Returns all elements to their starting positions

4. **Debug Mode**: When enabled via the `debug` prop, shows the actual Matter.js physics bodies as overlays, which is super helpful for development.

### MatterBody component

The `MatterBody` component transforms regular HTML elements into physics-enabled elements. Key features:

- **Positioning**: Set initial positions with `x` and `y` props


**File**: `Matter Body `
```
<MatterBody x="50%" y="100px">
  <div>I'm physics-enabled!</div>
</MatterBody>
```


- **Body Types**: Choose between different physics shapes:

  - `rectangle`: Default, good for most elements
  - `circle`: Perfect for round elements
  - `svg`: For complex custom shapes

- **Physics Properties**: Customize how elements behave with `matterBodyOptions`. The most commonly used options are:


**File**: `Matter Body options`
```tsx
<MatterBody 
  matterBodyOptions={{ 
    friction: 0.5,     // How slippery it is
    restitution: 0.7,  // How bouncy it is
    density: 0.001,    // How heavy it is
    isStatic: false,   // If true, the element won't move but can be collided with
    force: { x: 0, y: 0 } // Initial force applied to the body
    // ... 
  }}
>
  <div>I'm bouncy!</div>
</MatterBody>
```


For a complete list of options, check out the [Matter.js Body documentation](https://brm.io/matter-js/docs/classes/Body.html#properties). You can fine-tune everything from angular velocity to mass to create exactly the physics behavior you want.

### Context

The components use React Context to communicate. When you wrap an element with `MatterBody`, it registers itself with the parent `Gravity` component. The registration process:

1. Creates a Matter.js physics body matching your element's size and shape
2. Adds the body to the physics world
3. Sets up a sync system where the HTML element's position updates to match its physics body

## Examples

### Non-draggable bodies

By default, the MatterBody makes its element draggable. You can disable this behavior by setting the `isDraggable` prop to `false`. (Under the hood, we just add back the pointer-events to the elements, so they will be clickable, hover-able, etc, but the Matter body underneath will not receive any pointer events). This can be handy to create creative footers with clickable links for example:


### Demo Code (gravity-non-draggable-demo.tsx)
```tsx
"use client"

import { motion } from "motion/react"

import Gravity, { MatterBody } from "@/fancy/components/physics/gravity"

const socialLinks = [
  { name: "LinkedIn", x: "30%", y: "10%" },
  { name: "X (Twitter)", x: "30%", y: "30%" },
  { name: "Instagram", x: "40%", y: "20%", angle: 10 },
  { name: "GitHub", x: "75%", y: "10%", angle: -4 },
  { name: "BlueSky", x: "80%", y: "20%", angle: 5 },
]

const stars = ["✱", "✽", "✦", "✸", "✹", "✺"]

export default function Preview() {
  return (
    <div >
      <p >
        CONTACT
      </p>
      <Gravity gravity={{ x: 0, y: 1 }} >
        {socialLinks.map((link) => (
          <MatterBody
            key={link.name}
            matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
            x={link.x}
            y={link.y}
            angle={link.angle || 0}
            isDraggable={false}
          >
            <motion.div
              
              whileTap={{ scale: 0.9 }}
            >
              {link.name}
            </motion.div>
          </MatterBody>
        ))}

        {stars.map((star, i) => (
          <MatterBody
            key={i}
            matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
            x={`${Math.random() * 60 + 20}%`}
            y={`${Math.random() * 20 + 40}%`}
            angle={Math.random() * 360}
          >
            <div
              className={`aspect-square w-12 h-12 sm:w-14 sm:h-14 md:w-16 md:h-16 bg-primary-blue text-white rounded-lg text-center`}
            ></div>
          </MatterBody>
        ))}
      </Gravity>
    </div>
  )
}
```


### Different body types

With the `bodyType` prop, you can choose between different types of bodies. The available types are `circle`, `rectangle`, and `svg`.

In this example, we have a mixed of `circle` and `rectangle` bodies. Again, you do not need to define the sizes on the `MatterBody` component, you can define them on your component level, eg. adding `w-12 h12` to your tailwind classes. Then, the component will calculate the size for the matter.js engine.


### Demo Code (gravity-body-types-demo.tsx)
```tsx
import {
  Atom,
  AudioLines,
  BatteryCharging,
  Brain,
  Cloud,
  Cog,
  Cpu,
  Cuboid,
  Earth,
  Eye,
  Globe,
  HandMetal,
  Heart,
  Laptop,
  Layers,
  MessageCircle,
  Microscope,
  Move,
  PaintRoller,
  PersonStanding,
  Pyramid,
  Regex,
  Rocket,
  Satellite,
  Save,
  ScanFace,
  Settings,
  Sigma,
  Sparkles,
  Star,
  Sun,
  TrendingUp,
  Zap,
} from "lucide-react"

import Gravity, { MatterBody } from "@/fancy/components/physics/gravity"

export default function Preview() {
  const icons = [
    { icon: Atom, size: 24 },
    { icon: Brain, size: 24 },
    { icon: Cog, size: 24 },
    { icon: Cpu, size: 24 },
    { icon: TrendingUp, size: 24 },
    { icon: Globe, size: 24 },
    { icon: Laptop, size: 24 },
    { icon: Microscope, size: 24 },
    { icon: Pyramid, size: 24 },
    { icon: Rocket, size: 24 },
    { icon: PaintRoller, size: 24 },
    { icon: Eye, size: 24 },
    { icon: ScanFace, size: 24 },
    { icon: PersonStanding, size: 24 },
    { icon: Sun, size: 24 },
    { icon: Sparkles, size: 24 },
    { icon: Regex, size: 24 },
    { icon: Cloud, size: 24 },
    { icon: Settings, size: 24 },
    { icon: MessageCircle, size: 24 },
    { icon: Cuboid, size: 24 },
    { icon: Atom, size: 24 },
    { icon: Brain, size: 24 },
    { icon: AudioLines, size: 24 },
    { icon: BatteryCharging, size: 24 },
    { icon: Satellite, size: 24 },
    { icon: Move, size: 24 },
    { icon: Star, size: 24 },
    { icon: HandMetal, size: 24 },
    { icon: Heart, size: 24 },
    { icon: Save, size: 24 },
    { icon: Layers, size: 24 },
    { icon: Earth, size: 24 },
    { icon: Zap, size: 24 },
    { icon: Sigma, size: 24 },
  ]

  return (
    <div >
      <h2 >
        icons from lucide.dev
      </h2>
      <Gravity gravity={{ x: 0, y: 1 }} >
        {icons.map((IconData, index) => {
          const Icon = IconData.icon
          const randomX = Math.random() * 60 + 20 // Random x between 20-80%
          const randomY = Math.random() * 20 + 5 // Random y between 5-25%
          const bodyType = Math.random() > 0.7 ? "rectangle" : "circle"

          return (
            <MatterBody
              key={index}
              matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
              bodyType={bodyType}
              x={`${randomX}%`}
              y={`${randomY}%`}
            >
              <div
                className={`p-4 ${
                  bodyType === "circle" ? "rounded-full" : "rounded-md"
                } bg-white border border-border shadow-md text-foreground dark:text-muted`}
              >
                <Icon size={IconData.size} />
              </div>
            </MatterBody>
          )
        })}
      </Gravity>
    </div>
  )
}
```


### SVGs

The third `bodyType` option is `svg`, which allows you to create physics bodies from SVG elements. This is particularly useful for creating custom-shaped physics objects that match your SVG graphics.

Here's how it works:

1. The component takes your SVG element and extracts the path data
2. It converts the path into a series of vertices (points) that outline the shape (with a custom converter using the `svg-path-commander` package)
3. These vertices are then converted into polygons by matter.js (with the help of the `poly-decomp` package).
4. The resulting polygons are then used to create Matter.js bodies


### Demo Code (gravity-svg-bodies-demo.tsx)
```tsx
"use client"

import { useState } from "react"

import Gravity, { MatterBody } from "@/fancy/components/physics/gravity"

export default function Preview() {
  const [debug, setDebug] = useState(false)

  return (
    <div >
      <button
        onClick={() => setDebug(!debug)}
        
      >
        {debug ? "Disable Debug" : "Enable Debug"}
      </button>
      <Gravity gravity={{ x: 0, y: 1 }}  debug={debug}>
        <MatterBody
          matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
          x="30%"
          y="10%"
          bodyType="svg"
        >
          <svg
            width="111"
            height="108"
            viewBox="0 0 111 108"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M19.9185 107.176L33.6145 66.472L0.5905 44.328H41.0385L55.5025 0.679993L70.2225 44.328L110.415 44.328L77.3905 66.472L91.3425 107.176L55.5025 81.192L19.9185 107.176Z"
              fill="#1F464D"
            />
          </svg>
        </MatterBody>
        <MatterBody
          matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
          x="80%"
          y="30%"
          bodyType="svg"
        >
          <svg
            width="152"
            height="153"
            viewBox="0 0 152 153"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M45.3648 152.4L41.7648 150.8L52.5648 100L1.76484 110.8L0.164844 107.2L41.7648 76.4L0.164844 46L1.76484 42.4L52.5648 52.8L41.7648 2.39999L45.3648 0.799989L76.1648 42.4L106.565 0.799989L110.165 2.39999L99.7648 52.8L150.165 42.4L151.765 46L110.165 76.4L151.765 107.2L150.165 110.8L99.7648 100L110.165 150.8L106.565 152.4L76.1648 110.8L45.3648 152.4Z"
              fill="#0015FF"
            />
          </svg>
        </MatterBody>
        <MatterBody
          matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
          x="40%"
          y="20%"
          angle={10}
          bodyType="svg"
        >
          <svg
            width="99"
            height="99"
            viewBox="0 0 99 99"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M46.9325 98.376C45.0125 87.368 37.2045 73.544 22.3565 62.408C15.0605 56.904 7.6365 53.32 0.3405 51.784V46.408C14.8045 42.952 29.0125 33.224 38.1005 20.04C42.7085 13.384 45.6525 6.856 46.9325 0.0719986H52.3085C54.4845 13 64.4685 27.336 78.0365 36.936C84.6925 41.672 91.6045 44.872 98.6445 46.408V51.784C84.4365 54.728 67.9245 67.4 59.7325 80.328C55.6365 86.856 53.2045 92.872 52.3085 98.376H46.9325Z"
              fill="#E794DA"
            />
          </svg>
        </MatterBody>
        <MatterBody
          matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
          x="75%"
          y="10%"
          bodyType="circle"
        >
          <div  />
        </MatterBody>
        <MatterBody
          matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
          x="80%"
          y="20%"
        >
          <div  />
        </MatterBody>
        <MatterBody
          matterBodyOptions={{
            friction: 0.5,
            restitution: 0.2,
            isStatic: true,
          }}
          x="50%"
          y="95%"
          bodyType="svg"
        >
          <svg
            width="298"
            height="125"
            viewBox="0 0 298 125"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M10.7705 44.16H2.3225L1.8105 41.344C1.8105 41.344 7.9545 39.936 10.7705 37.376V32.512C10.7705 16.128 19.3465 -7.62939e-06 37.9065 -7.62939e-06C48.1465 -7.62939e-06 53.2665 3.96799 53.2665 8.448C53.2665 12.032 50.0665 14.336 46.9945 14.336C46.2265 14.336 45.7145 14.208 45.7145 14.208C45.0745 12.288 42.1305 5.24799 35.0905 5.24799C25.8745 5.24799 21.7785 14.848 21.7785 28.672V38.4H42.2585V44.416H21.7785V84.48C21.7785 90.496 23.4425 91.776 27.2825 92.032L34.1945 92.544V96C34.1945 96 22.0345 95.616 16.4025 95.616C10.5145 95.616 0.5305 96 0.5305 96V92.544L3.9865 92.032C7.8265 91.52 10.7705 88.32 10.7705 83.456V44.16ZM58.4255 83.584C58.4255 89.216 62.1375 91.648 66.4895 91.648C72.6335 91.648 74.9375 88.96 80.1855 87.68C80.1855 87.68 79.5455 84.096 79.5455 79.36C79.5455 74.752 79.6735 70.528 79.6735 70.528C68.1535 70.528 58.4255 74.752 58.4255 83.584ZM79.8015 66.432C79.8015 66.432 80.1855 57.856 80.1855 53.632C80.1855 46.848 77.8815 41.728 69.8175 41.728C63.6735 41.728 61.2415 45.056 61.2415 48.256C61.2415 50.304 62.0095 51.968 62.9055 52.992C61.2415 54.272 59.0655 54.912 56.7615 54.912C52.6655 54.912 49.9775 52.736 49.9775 49.024C49.9775 42.112 60.8575 36.352 72.6335 36.352C85.8175 36.352 90.8095 44.032 90.8095 55.936C90.8095 64 90.1695 71.936 90.1695 80.512C90.1695 86.912 90.9375 91.008 96.1855 91.008C99.3855 91.008 101.434 89.728 101.434 89.728L102.586 91.904C102.586 91.904 98.7455 98.048 91.3215 98.048C85.3055 98.048 82.1055 94.208 81.0815 90.496C75.8335 91.648 72.1215 98.048 61.8815 98.048C52.6655 98.048 47.1615 93.44 47.1615 84.992C47.1615 70.656 64.9535 66.432 79.8015 66.432ZM170.138 92.032L174.106 92.544V96C174.106 96 165.658 95.616 160.026 95.616C153.754 95.616 144.666 96 144.666 96V92.544L147.354 92.032C151.194 91.264 154.138 88.32 154.138 83.456V54.656C154.138 47.744 150.682 44.16 144.026 44.16C137.242 44.16 132.378 48.512 127.514 49.408V84.48C127.514 90.496 128.41 91.648 132.506 92.032L136.602 92.544V96C136.602 96 127.77 95.616 122.01 95.616C116.25 95.616 106.138 96 106.138 96V92.544L109.722 92.032C113.562 91.52 116.506 88.32 116.506 83.456V52.864C116.506 47.36 114.33 45.184 106.906 45.184L106.138 42.368C118.042 41.216 127.514 36.48 127.514 36.48V45.824C130.586 44.928 138.266 36.352 148.25 36.352C159.642 36.352 165.018 43.776 165.018 53.248V84.48C165.018 90.496 166.042 91.52 170.138 92.032ZM207.426 42.112C198.466 42.112 191.298 49.408 191.298 66.048C191.298 77.952 198.082 89.856 213.314 89.856C219.202 89.856 224.194 88.704 227.906 86.912L229.186 89.344C227.522 91.392 220.61 98.048 207.554 98.048C197.826 98.048 180.034 91.392 180.034 68.608C180.034 52.096 191.938 36.352 212.418 36.352C220.994 36.352 230.338 39.808 230.338 46.72C230.338 50.56 226.114 52.992 222.146 52.992C220.866 52.992 219.714 52.736 218.562 52.224C218.562 52.224 219.074 50.944 219.074 49.28C219.074 46.208 216.77 42.112 207.426 42.112ZM263.396 38.4V41.856L260.196 42.368C257.636 42.752 256.356 43.648 256.356 45.312C256.356 45.568 256.356 46.208 256.612 47.104L268.004 83.968L282.084 47.872C282.468 46.72 282.724 45.952 282.724 45.44C282.724 43.904 281.7 42.752 279.396 42.368L275.94 41.856V38.4C275.94 38.4 281.316 38.784 288.1 38.784C293.732 38.784 297.956 38.4 297.956 38.4V41.856L296.036 42.24C292.196 43.008 289.38 44.928 287.332 49.92L263.396 106.624C259.044 116.992 252.9 124.416 242.66 124.416C233.444 124.416 230.883 119.68 230.883 115.712C230.883 112.896 233.059 109.824 237.796 109.824C238.564 109.824 239.588 109.952 239.844 110.08C239.716 110.464 239.588 110.976 239.588 111.488C239.588 115.072 241.764 116.48 245.348 116.48C251.236 116.48 256.1 113.024 259.3 104.96L262.5 97.024L245.988 50.304C243.94 44.416 241.635 42.88 238.436 42.368L234.98 41.856V38.4C234.98 38.4 244.708 38.784 250.468 38.784C255.46 38.784 263.396 38.4 263.396 38.4Z"
              fill="black"
            />
          </svg>
        </MatterBody>
      </Gravity>
    </div>
  )
}
```


As you can see in the demo above, SVG bodies can produce varying results. Simple shapes like the stars translate well, maintaining their shapes in the physics simulation. More complex shapes like the _fancy_ text at the bottom (which is an SVG path, and not an HTML element) end up with rougher approximations.

This variance in quality stems from the challenging process of converting SVG paths to physics bodies. Therefore, there are a few caveats to keep in mind:

1. **SVG Requirements**:

   - Keep them simple. The simpler the SVG, the better the decomposition, and the simulation.
   - It's only tested with single-path SVGs, and it probably won't work with nested paths.
   - Avoid shapes with holes or complex curves, or shapes that are seem to be too complex to decompose into polygons.

2. **Performance Impact**:
   - Complex SVGs create more detailed physics bodies, which can slow down the simulation
   - More vertices mean more calculations
   - The initial path-to-vertices conversion can be slow.

If you're not getting the desired results, you have several options:

1. Break down complex SVGs into simpler shapes
2. Use basic physics bodies (rectangles/circles) with the SVG as a visual overlay
3. Fine-tune the vertex sampling with the `sampleLength` prop

While the demo's _fancy_ text on the bottom worked well by chance for me, you more than likely will need to experiment with different settings to get the desired results. Use the `debug` prop to visualize the physics bodies and their vertices, and adjust the `sampleLength` prop to control the accuracy of the conversion.

For more details on the decomposition process, refer to the [poly-decomp documentation](https://github.com/schteppe/poly-decomp), the [Matter.js documentation](https://brm.io/matter-js/docs/classes/Bodies.html#method_fromVertices), and to the [SVG path commander documentation](https://github.com/thednp/svg-path-commander).

## Props

### Gravity


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The content to be displayed and animated |
| debug | `boolean` | `false` | Whether to show the physics bodies and their vertices |
| gravity | `{ x: number; y: number }` | `{ x: 0, y: 1 }` | The direction of gravity |
| resetOnResize | `boolean` | `true` | Whether to reset the physics world when the window is resized |
| grabCursor | `boolean` | `true` | Whether to show grab/grabbing cursor when interacting with bodies |
| addTopWall | `boolean` | `true` | Whether to add a wall at the top of the canvas |
| autoStart | `boolean` | `true` | Whether to automatically start the physics simulation |
| className | `string` | - | Additional CSS classes to apply to the container |


### MatterBody


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The content to be displayed and animated |
| matterBodyOptions | `Matter.IBodyDefinition` | `{ friction: 0.1, restitution: 0.1, density: 0.001, isStatic: false }` | Matter.js body configuration options |
| bodyType | `"rectangle" | "circle" | "svg"` | `"rectangle"` | The type of physics body to create |
| isDraggable | `boolean` | `true` | Whether the body can be dragged with the mouse |
| sampleLength | `number` | `15` | The sampling distance for SVG path vertices |
| x | `number | string` | `0` | Initial x position (can be percentage string) |
| y | `number | string` | `0` | Initial y position (can be percentage string) |
| angle | `number` | `0` | Initial rotation angle in degrees |
| className | `string` | - | Additional CSS classes to apply to the container |


---

### Cursor Attractor & Gravity <a name="cursor-attractor--gravity"></a>

### Demo Code (cursor-attractor-and-gravity-demo.tsx)
```tsx
import { motion } from "motion/react"

import useScreenSize from "@/hooks/use-screen-size"
import Gravity, {
  MatterBody,
} from "@/fancy/components/physics/cursor-attractor-and-gravity"

export default function Preview() {
  const screenSize = useScreenSize()
  const words = [
    "we",
    "analyze",
    { text: "millions", highlight: true },
    { text: "of", highlight: true },
    { text: "data", highlight: true },
    { text: "points", highlight: true },
    "per",
    "second",
    "to",
    "provide",
    "you",
    "with",
    "the",
    "most",
    "accurate",
    "insights.",
  ]

  return (
    <div >
      <Gravity
        attractorStrength={0.0}
        cursorStrength={0.0004}
        cursorFieldRadius={200}
        
      >
        {[...Array(150)].map((_, i) => {
          // Adjust max size based on screen size
          const maxSize = screenSize.lessThan("sm")
            ? 20
            : screenSize.lessThan("md")
              ? 30
              : 40
          const size = Math.max(
            screenSize.lessThan("sm") ? 10 : 20,
            Math.random() * maxSize
          )
          return (
            <MatterBody
              key={i}
              matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
              x={`${Math.random() * 100}%`}
              y={`${Math.random() * 100}%`}
            >
              <div
                
                style={{
                  width: `${size}px`,
                  height: `${size}px`,
                }}
              />
            </MatterBody>
          )
        })}
      </Gravity>
      
        {words.map((word, index) => {
          const text = typeof word === "string" ? word : word.text
          const highlight = typeof word === "object" && word.highlight

          return (
            <motion.span
              key={index}
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              transition={{ duration: 0.5, delay: index * 0.05 }}
              className={`${highlight ? "text-[#0015ff]" : ""} ${index < words.length - 1 ? "mr-1" : ""}`}
            >
              {text}
            </motion.span>
          )
        })}
      
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/cursor-attractor-and-gravity
```





Install the following dependencies:


```bash
# Install command:
matter-js @types/matter-js poly-decomp
```


Then, create an utility function for parsing SVG paths into a set of vertices:


*Component Source: svg-path-to-vertices (Source file not found)*


The other is for calculating the position of an element based on its container, and a posiiton value


*Component Source: calculate-position (Source file not found)*


And another one for calculating the position of an element based on its container, and a posiiton value:


### Component Source Code (cursor-attractor-and-gravity.tsx)
```tsx
"use client"

import {
  createContext,
  forwardRef,
  ReactNode,
  useCallback,
  useContext,
  useEffect,
  useImperativeHandle,
  useRef,
  useState,
} from "react"
import { calculatePosition } from "@/utils/calculate-position"
import { parsePathToVertices } from "@/utils/svg-path-to-vertices"
import { debounce } from "lodash"
import Matter, {
  Bodies,
  Common,
  Engine,
  Events,
  Render,
  Runner,
  World,
  Body,
} from "matter-js"

import { cn } from "@/lib/utils"
import { useMousePositionRef } from "@/hooks/use-mouse-position-ref"

type GravityProps = {
  children: ReactNode
  debug?: boolean
  attractorPoint?: { x: number | string; y: number | string }
  attractorStrength?: number
  cursorStrength?: number
  cursorFieldRadius?: number
  resetOnResize?: boolean
  addTopWall?: boolean
  autoStart?: boolean
  className?: string
}

type PhysicsBody = {
  element: HTMLElement
  body: Matter.Body
  props: MatterBodyProps
}

type MatterBodyProps = {
  children: ReactNode
  matterBodyOptions?: Matter.IBodyDefinition
  isDraggable?: boolean
  bodyType?: "rectangle" | "circle" | "svg"
  sampleLength?: number
  x?: number | string
  y?: number | string
  angle?: number
  className?: string
}

export type GravityRef = {
  start: () => void
  stop: () => void
  reset: () => void
}

const GravityContext = createContext<{
  registerElement: (
    id: string,
    element: HTMLElement,
    props: MatterBodyProps
  ) => void
  unregisterElement: (id: string) => void
} | null>(null)

export const MatterBody = ({
  children,
  className,
  matterBodyOptions = {
    friction: 0.1,
    restitution: 0.1,
    density: 0.001,
    isStatic: false,
  },
  bodyType = "rectangle",
  isDraggable = true,
  sampleLength = 15,
  x = 0,
  y = 0,
  angle = 0,
  ...props
}: MatterBodyProps) => {
  const elementRef = useRef<HTMLDivElement>(null)
  const idRef = useRef(Math.random().toString(36).substring(7))
  const context = useContext(GravityContext)

  useEffect(() => {
    if (!elementRef.current || !context) return
    context.registerElement(idRef.current, elementRef.current, {
      children,
      matterBodyOptions,
      bodyType,
      sampleLength,
      isDraggable,
      x,
      y,
      angle,
      ...props,
    })

    return () => context.unregisterElement(idRef.current)
  }, [props, children, matterBodyOptions, isDraggable])

  return (
    <div
      ref={elementRef}
      className={cn(
        "absolute",
        className,
      )}
    >
      {children}
    </div>
  )
}

const Gravity = forwardRef<GravityRef, GravityProps>(
  (
    {
      children,
      debug = false,
      attractorPoint = { x: 0.5, y: 0.5 },
      attractorStrength = 0.001,
      cursorStrength = 0.0005,
      cursorFieldRadius = 100,
      resetOnResize = true,
      addTopWall = true,
      autoStart = true,
      className,
      ...props
    },
    ref
  ) => {
    const canvas = useRef<HTMLDivElement>(null)
    const engine = useRef(Engine.create())
    const render = useRef<Render>(undefined)
    const runner = useRef<Runner>(undefined)
    const bodiesMap = useRef(new Map<string, PhysicsBody>())
    const frameId = useRef<number>(undefined)
    const [canvasSize, setCanvasSize] = useState({ width: 0, height: 0 })
    const mouseRef = useMousePositionRef(canvas)

    const isRunning = useRef(false)

    // Register Matter.js body in the physics world
    const registerElement = useCallback(
      (id: string, element: HTMLElement, props: MatterBodyProps) => {
        if (!canvas.current) return
        const width = element.offsetWidth
        const height = element.offsetHeight
        const canvasRect = canvas.current!.getBoundingClientRect()

        const angle = (props.angle || 0) * (Math.PI / 180)

        const x = calculatePosition(props.x, canvasRect.width, width)
        const y = calculatePosition(props.y, canvasRect.height, height)

        let body
        if (props.bodyType === "circle") {
          const radius = Math.max(width, height) / 2
          body = Bodies.circle(x, y, radius, {
            ...props.matterBodyOptions,
            angle: angle,
            render: {
              fillStyle: debug ? "#888888" : "#00000000",
              strokeStyle: debug ? "#333333" : "#00000000",
              lineWidth: debug ? 3 : 0,
            },
          })
        } else if (props.bodyType === "svg") {
          const paths = element.querySelectorAll("path")
          const vertexSets: Matter.Vector[][] = []

          paths.forEach((path) => {
            const d = path.getAttribute("d")
            const p = parsePathToVertices(d!, props.sampleLength)
            vertexSets.push(p)
          })

          body = Bodies.fromVertices(x, y, vertexSets, {
            ...props.matterBodyOptions,
            angle: angle,
            render: {
              fillStyle: debug ? "#888888" : "#00000000",
              strokeStyle: debug ? "#333333" : "#00000000",
              lineWidth: debug ? 3 : 0,
            },
          })
        } else {
          body = Bodies.rectangle(x, y, width, height, {
            ...props.matterBodyOptions,
            angle: angle,
            render: {
              fillStyle: debug ? "#888888" : "#00000000",
              strokeStyle: debug ? "#333333" : "#00000000",
              lineWidth: debug ? 3 : 0,
            },
          })
        }

        if (body) {
          World.add(engine.current.world, [body])
          bodiesMap.current.set(id, { element, body, props })
        }
      },
      [debug]
    )

    // Unregister Matter.js body from the physics world
    const unregisterElement = useCallback((id: string) => {
      const body = bodiesMap.current.get(id)
      if (body) {
        World.remove(engine.current.world, body.body)
        bodiesMap.current.delete(id)
      }
    }, [])

    // Keep react elements in sync with the physics world
    const updateElements = useCallback(() => {
      bodiesMap.current.forEach(({ element, body }) => {
        const { x, y } = body.position
        const rotation = body.angle * (180 / Math.PI)

        element.style.transform = `translate(${
          x - element.offsetWidth / 2
        }px, ${y - element.offsetHeight / 2}px) rotate(${rotation}deg)`
      })

      frameId.current = requestAnimationFrame(updateElements)
    }, [])

    const initializeRenderer = useCallback(() => {
      if (!canvas.current) return

      const height = canvas.current.offsetHeight
      const width = canvas.current.offsetWidth

      Common.setDecomp(require("poly-decomp"))

      // Remove default gravity
      engine.current.gravity.x = 0
      engine.current.gravity.y = 0

      render.current = Render.create({
        element: canvas.current,
        engine: engine.current,
        options: {
          width,
          height,
          wireframes: false,
          background: "#00000000",
        },
      })

      // Add walls
      const walls = [
        // Floor
        Bodies.rectangle(width / 2, height + 10, width, 20, {
          isStatic: true,
          friction: 1,
          render: {
            visible: debug,
          },
        }),

        // Right wall
        Bodies.rectangle(width + 10, height / 2, 20, height, {
          isStatic: true,
          friction: 1,
          render: {
            visible: debug,
          },
        }),

        // Left wall
        Bodies.rectangle(-10, height / 2, 20, height, {
          isStatic: true,
          friction: 1,
          render: {
            visible: debug,
          },
        }),
      ]

      const topWall = addTopWall
        ? Bodies.rectangle(width / 2, -10, width, 20, {
            isStatic: true,
            friction: 1,
            render: {
              visible: debug,
            },
          })
        : null

      if (topWall) {
        walls.push(topWall)
      }

      World.add(engine.current.world, [...walls])

      runner.current = Runner.create()
      Render.run(render.current)
      updateElements()
      runner.current.enabled = false

      if (autoStart) {
        runner.current.enabled = true
        startEngine()
      }

      // Add force application before update
      Events.on(engine.current, "beforeUpdate", () => {
        const bodies = engine.current.world.bodies.filter(
          (body) => !body.isStatic
        )

        // Calculate attractor position in pixels
        const attractorX = typeof attractorPoint.x === 'string' 
          ? (width * parseFloat(attractorPoint.x) / 100)
          : width * attractorPoint.x
        const attractorY = typeof attractorPoint.y === 'string'
          ? (height * parseFloat(attractorPoint.y) / 100) 
          : height * attractorPoint.y

        bodies.forEach((body) => {
          // Apply attractor force
          const dx = attractorX - body.position.x
          const dy = attractorY - body.position.y
          const distance = Math.sqrt(dx * dx + dy * dy)

          if (distance > 0) {
            const force = {
              x: (dx / distance) * attractorStrength * body.mass,
              y: (dy / distance) * attractorStrength * body.mass,
            }
            Body.applyForce(body, body.position, force)
          }

          // Apply cursor force if mouse is present
          if (mouseRef.current?.x && mouseRef.current?.y && mouseRef.current.x > 0 && mouseRef.current.y > 0) {
            const mdx = mouseRef.current.x - body.position.x
            const mdy = mouseRef.current.y - body.position.y
            const mouseDistance = Math.sqrt(mdx * mdx + mdy * mdy)

            if (mouseDistance > 0 && mouseDistance < cursorFieldRadius) {
              const mouseForce = {
                x: (mdx / mouseDistance) * cursorStrength * body.mass,
                y: (mdy / mouseDistance) * cursorStrength * body.mass,
              }
              Body.applyForce(body, body.position, mouseForce)
            }
          }
        })
      })
    }, [updateElements, debug, autoStart, attractorPoint, attractorStrength, cursorStrength])

    // Clear the Matter.js world
    const clearRenderer = useCallback(() => {
      if (frameId.current) {
        cancelAnimationFrame(frameId.current)
      }

      if (render.current) {
        Render.stop(render.current)
        render.current.canvas.remove()
      }

      if (runner.current) {
        Runner.stop(runner.current)
      }

      if (engine.current) {
        World.clear(engine.current.world, false)
        Engine.clear(engine.current)
      }

      bodiesMap.current.clear()
    }, [])

    const handleResize = useCallback(() => {
      if (!canvas.current || !resetOnResize) return

      const newWidth = canvas.current.offsetWidth
      const newHeight = canvas.current.offsetHeight

      setCanvasSize({ width: newWidth, height: newHeight })

      // Clear and reinitialize
      clearRenderer()
      initializeRenderer()
    }, [clearRenderer, initializeRenderer, resetOnResize])

    const startEngine = useCallback(() => {
      if (runner.current) {
        runner.current.enabled = true

        Runner.run(runner.current, engine.current)
      }
      if (render.current) {
        Render.run(render.current)
      }
      frameId.current = requestAnimationFrame(updateElements)
      isRunning.current = true
    }, [updateElements, canvasSize])

    const stopEngine = useCallback(() => {
      if (!isRunning.current) return

      if (runner.current) {
        Runner.stop(runner.current)
      }
      if (render.current) {
        Render.stop(render.current)
      }
      if (frameId.current) {
        cancelAnimationFrame(frameId.current)
      }
      isRunning.current = false
    }, [])

    const reset = useCallback(() => {
      stopEngine()
      bodiesMap.current.forEach(({ element, body, props }) => {
        body.angle = props.angle || 0

        const x = calculatePosition(
          props.x,
          canvasSize.width,
          element.offsetWidth
        )
        const y = calculatePosition(
          props.y,
          canvasSize.height,
          element.offsetHeight
        )
        body.position.x = x
        body.position.y = y
      })
      updateElements()
      handleResize()
    }, [])

    useImperativeHandle(
      ref,
      () => ({
        start: startEngine,
        stop: stopEngine,
        reset,
      }),
      [startEngine, stopEngine]
    )

    useEffect(() => {
      if (!resetOnResize) return

      const debouncedResize = debounce(handleResize, 500)
      window.addEventListener("resize", debouncedResize)

      return () => {
        window.removeEventListener("resize", debouncedResize)
        debouncedResize.cancel()
      }
    }, [handleResize, resetOnResize])

    useEffect(() => {
      initializeRenderer()
      return clearRenderer
    }, [initializeRenderer, clearRenderer])

    return (
      <GravityContext.Provider value={{ registerElement, unregisterElement }}>
        <div
          ref={canvas}
          className={cn(className, "absolute top-0 left-0 w-full h-full")}
          {...props}
        >
          {children}
        </div>
      </GravityContext.Provider>
    )
  }
)

Gravity.displayName = "Gravity"
export default Gravity
```





## Usage

First, you need to wrap your scene / content with the `Gravity` component. Set the attraction point coordinates in the `attractorPoint` prop. This point will either attract or repel all the bodies inside the container. Then, in order to transform your regular HTML elements into Matter bodies, you need to wrap them with the `MatterBody` component. 

You need to set each bodies `x` and `y` position, either as a percentage of your container size, or as a number. You do not need to set the width and height manually, everything else is taken care of by component :). Lastly, set the strength and radius of the cursor attractor, which will also attract or repell all bodies. High-level example:


**File**: `Gravity usage example`
```tsx
<Gravity attractorPoint={{ x: "50%", y: "50%" }} attractorStrength={0.0006} cursorStrength={-0.005} cursorFieldRadius={200}>
  <MatterBody x="50%" y="50%">
    <div>Hello world!</div>
  </MatterBody>
  <MatterBody x="10%" y="10%">
    <div>fancy!</div>
  </MatterBody>
</Gravity>
```


## Understanding the component

Please refer to the [Gravity](/docs/components/physics/gravity) documentation, since the component is almost identical. The only difference is that in this component that we don't use a directional gravitational force.Instead, we use attractor force(s), either from a defined static point (optional), and/or from the cursor position, that attract or repel all bodies inside the container. This is achieved by calculating the distance between the attractor point(s) and each body, and applying a force in the opposite direction of the body's velocity. The force is proportional to the distance and inversely proportional to the mass of the body.

## Examples

### Repel

By setting one of the attractor points' strength to a negative value, you can create a repelling effect. The following demo showcases a negative force from the cursor by applying a negative value to the `cursorStrength` prop.


### Demo Code (cursor-attractor-and-gravity-image-demo.tsx)
```tsx
import useScreenSize from "@/hooks/use-screen-size"
import Gravity, {
  MatterBody,
} from "@/fancy/components/physics/cursor-attractor-and-gravity"

export default function Preview() {
  const screenSize = useScreenSize()

  const getImageCount = () => {
    if (screenSize.lessThan("sm")) return 50
    if (screenSize.lessThan("md")) return 60
    if (screenSize.lessThan("lg")) return 70
    return 80
  }

  const getMaxSize = () => {
    if (screenSize.lessThan("sm")) return 40
    if (screenSize.lessThan("md")) return 50
    return 60
  }

  const getMinSize = () => {
    if (screenSize.lessThan("sm")) return 10
    if (screenSize.lessThan("md")) return 20
    return 20
  }

  return (
    <div >
      <div>
        <p >
          join the community
        </p>
      </div>
      <Gravity
        attractorPoint={{ x: "33%", y: "50%" }}
        attractorStrength={0.0005}
        cursorStrength={-0.004}
        cursorFieldRadius={screenSize.lessThan("sm") ? 100 : 200}
        
      >
        {[...Array(getImageCount())].map((_, i) => {
          const size = Math.max(getMinSize(), Math.random() * getMaxSize())
          return (
            <MatterBody
              key={i}
              matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
              x={`${Math.random() * 100}%`}
              y={`${Math.random() * 30}%`}
            >
              <img
                src={`https://randomuser.me/api/portraits/${i % 2 === 0 ? "men" : "women"}/${i}.jpg`}
                alt={`Avatar ${i}`}
                
                style={{
                  width: `${size}px`,
                  height: `${size}px`,
                }}
              />
            </MatterBody>
          )
        })}
      </Gravity>
    </div>
  )
}
```


### SVGs

Youy can choose `svg` as a `bodyType` for your matter bodies. This is particularly useful for creating custom-shaped physics objects that match your SVG graphics.

Here's how it works:

1. The component takes your SVG element and extracts the path data
2. It converts the path into a series of vertices (points) that outline the shape (with a custom converter using the `svg-path-commander` package)
3. These vertices are then converted into polygons by matter.js (with the help of the `poly-decomp` package).
4. The resulting polygons are then used to create Matter.js bodies


### Demo Code (cursor-attractor-and-gravity-svg-demo.tsx)
```tsx
import { useState } from "react"

import Gravity, {
  MatterBody,
} from "@/fancy/components/physics/cursor-attractor-and-gravity"

export default function Preview() {
  const [debug, setDebug] = useState(false)

  return (
    <div >
      <button
        onClick={() => setDebug(!debug)}
        
      >
        {debug ? "Disable Debug" : "Enable Debug"}
      </button>
      <p >
        fancy components
      </p>
      <Gravity
        //attractorPoint={{ x: "33%", y: "50%" }}
        attractorStrength={0.0}
        cursorStrength={0.0005}
        cursorFieldRadius={200}
        
        debug={debug}
      >
        {["#0015FF", "#E794DA"].map((color, i) => (
          <>
            <MatterBody
              key={`star1-${i}`}
              matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
              x={`${10 + Math.random() * 80}%`}
              y={`${10 + Math.random() * 80}%`}
              bodyType="svg"
            >
              <svg
                width="111"
                height="108"
                viewBox="0 0 111 108"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M19.9185 107.176L33.6145 66.472L0.5905 44.328H41.0385L55.5025 0.679993L70.2225 44.328L110.415 44.328L77.3905 66.472L91.3425 107.176L55.5025 81.192L19.9185 107.176Z"
                  fill={color}
                />
              </svg>
            </MatterBody>

            <MatterBody
              key={`star2-${i}`}
              matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
              x={`${10 + Math.random() * 80}%`}
              y={`${10 + Math.random() * 80}%`}
              bodyType="svg"
            >
              <svg
                width="152"
                height="153"
                viewBox="0 0 152 153"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M45.3648 152.4L41.7648 150.8L52.5648 100L1.76484 110.8L0.164844 107.2L41.7648 76.4L0.164844 46L1.76484 42.4L52.5648 52.8L41.7648 2.39999L45.3648 0.799989L76.1648 42.4L106.565 0.799989L110.165 2.39999L99.7648 52.8L150.165 42.4L151.765 46L110.165 76.4L151.765 107.2L150.165 110.8L99.7648 100L110.165 150.8L106.565 152.4L76.1648 110.8L45.3648 152.4Z"
                  fill={color}
                />
              </svg>
            </MatterBody>

            <MatterBody
              key={`star3-${i}`}
              matterBodyOptions={{ friction: 0.5, restitution: 0.2 }}
              x={`${10 + Math.random() * 80}%`}
              y={`${10 + Math.random() * 80}%`}
              angle={10}
              bodyType="svg"
            >
              <svg
                width="99"
                height="99"
                viewBox="0 0 99 99"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M46.9325 98.376C45.0125 87.368 37.2045 73.544 22.3565 62.408C15.0605 56.904 7.6365 53.32 0.3405 51.784V46.408C14.8045 42.952 29.0125 33.224 38.1005 20.04C42.7085 13.384 45.6525 6.856 46.9325 0.0719986H52.3085C54.4845 13 64.4685 27.336 78.0365 36.936C84.6925 41.672 91.6045 44.872 98.6445 46.408V51.784C84.4365 54.728 67.9245 67.4 59.7325 80.328C55.6365 86.856 53.2045 92.872 52.3085 98.376H46.9325Z"
                  fill={color}
                />
              </svg>
            </MatterBody>
          </>
        ))}
      </Gravity>
    </div>
  )
}
```


As you can see in the demo above, SVG bodies can produce varying results. Simple shapes like some of the stars translate well, but some of them are a bit rough.

This variance in quality stems from the challenging process of converting SVG paths to physics bodies. Therefore, there are a few caveats to keep in mind:

1. **SVG Requirements**:

   - Keep them simple. The simpler the SVG, the better the decomposition, and the simulation.
   - It's only tested with single-path SVGs, and it probably won't work with nested paths.
   - Avoid shapes with holes or complex curves, or shapes that are seem to be too complex to decompose into polygons.

2. **Performance Impact**:
   - Complex SVGs create more detailed physics bodies, which can slow down the simulation
   - More vertices mean more calculations
   - The initial path-to-vertices conversion can be slow.

If you're not getting the desired results, you have several options:

1. Break down complex SVGs into simpler shapes
2. Use basic physics bodies (rectangles/circles) with the SVG as a visual overlay
3. Fine-tune the vertex sampling with the `sampleLength` prop

You more than likely will need to experiment with different settings to get the desired results. Use the `debug` prop to visualize the physics bodies and their vertices, and adjust the `sampleLength` prop to control the accuracy of the conversion.

For more details on the decomposition process, refer to the [poly-decomp documentation](https://github.com/schteppe/poly-decomp), the [Matter.js documentation](https://brm.io/matter-js/docs/classes/Bodies.html#method_fromVertices), and to the [SVG path commander documentation](https://github.com/thednp/svg-path-commander).

## Credits

Ported to Framer by [Framer University](https://framer.university/)

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The content to be displayed |
| attractorPoint | `{ x: number | string; y: number | string }` | `{ x: 0.5, y: 0.5 }` | The attractor point coordinates |
| attractorStrength | `number` | `0.001` | The strength of the attractor force |
| cursorStrength | `number` | `0.0005` | The strength of the cursor force |
| cursorFieldRadius | `number` | `100` | The radius of the cursor field |
| resetOnResize | `boolean` | `true` | Whether to reset the physics world when the window is resized |
| addTopWall | `boolean` | `true` | Whether to add a wall at the top of the canvas |
| autoStart | `boolean` | `true` | Whether to automatically start the physics simulation |
| className | `string` | - | Additional CSS classes for styling |


---

## Image <a name="image"></a>

### Image Trail <a name="image-trail"></a>

### Demo Code (image-trail-demo.tsx)
```tsx
import Image from "next/image"
import { exampleImages } from "@/utils/demo-images"

import ImageTrail,{
  ImageTrailItem,
} from "@/fancy/components/image/image-trail"

const ImageTrailDemo = () => {
  return (
    <div >
      <ImageTrail
         threshold={80}
         keyframes={{ opacity: [0, 1, 1, 0], scale: [1, 1, 2] }}
         keyframesOptions={{
           opacity: { duration: 2, times: [0, 0.001, 0.9, 1] },
           scale: { duration: 2, times: [0, 0.8, 1] },
         }}
         repeatChildren={1}
      >
        {[...exampleImages, ...exampleImages].map((image, index) => (
          <ImageTrailItem key={index}>
            <div >
              <Image
                src={image.url}
                alt="image"
                fill
                
                sizes="96px"
              />
            </div>
          </ImageTrailItem>
        ))}
      </ImageTrail>
      <h1 >
        ALBUMS
      </h1>
    </div>
  )
}

export default ImageTrailDemo
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/image-trail
```






### Component Source Code (image-trail.tsx)
```tsx
// author: Khoa Phan <https://www.pldkhoa.dev>

"use client"

import React, { ElementType, HTMLAttributes, useEffect, useMemo } from "react"
import type { DOMKeyframesDefinition, AnimationOptions } from "motion"
import { useAnimate } from "motion/react"

import { cn } from "@/lib/utils"

interface ImageTrailProps extends HTMLAttributes<HTMLDivElement> {
  /**
   * The content to be displayed
   */
  children: React.ReactNode

  /**
   * HTML Tag
   */
  as?: ElementType

  /**
   * How much distance in pixels the mouse has to travel to trigger of an element to appear.
   */
  threshold?: number

  /**
   * The intensity for the momentum movement after showing the element. The value will be clamped > 0 and <= 1.0. Defaults to 0.3.
   */
  intensity?: number

  /**
   * Animation Keyframes for defining the animation sequence. Example: { scale: [0, 1, 1, 0] }
   */
  keyframes?: DOMKeyframesDefinition

  /**
   * Options for the animation/keyframes. Example: { duration: 1, times: [0, 0.1, 0.9, 1] }
   */
  keyframesOptions?: AnimationOptions

  /**
   * Animation keyframes for the x and y positions after showing the element. Describes how the element should try to arrive at the mouse position.
   */
  trailElementAnimationKeyframes?: {
    x?: AnimationOptions
    y?: AnimationOptions
  }

  /**
   * The number of times the children will be repeated. Defaults to 3.
   */
  repeatChildren?: number

  /**
   * The base zIndex for all elements. Defaults to 0.
   */
  baseZIndex?: number

  /**
   * Controls stacking order behavior.
   * - "new-on-top": newer elements stack above older ones (default)
   * - "old-on-top": older elements stay visually on top
   */
  zIndexDirection?: "new-on-top" | "old-on-top"
}

interface ImageTrailItemProps extends HTMLAttributes<HTMLDivElement> {
  /**
   * HTML Tag
   */
  as?: ElementType

  /**
   * The content to be displayed
   */
  children: React.ReactNode
}

/**
 * Helper functions
 */
const MathUtils = {
  // linear interpolation
  lerp: (a: number, b: number, n: number) => (1 - n) * a + n * b,
  // distance between two points
  distance: (x1: number, y1: number, x2: number, y2: number) =>
    Math.hypot(x2 - x1, y2 - y1),
}

const ImageTrail = ({
  className,
  as = "div",
  children,
  threshold = 100,
  intensity = 0.3,
  keyframes,
  keyframesOptions,
  repeatChildren = 3,
  trailElementAnimationKeyframes = {
    x: { duration: 1, type: "tween", ease: "easeOut" },
    y: { duration: 1, type: "tween", ease: "easeOut" },
  },
  baseZIndex = 0,
  zIndexDirection = "new-on-top",
  ...props
}: ImageTrailProps) => {
  const allImages = React.useRef<NodeListOf<HTMLElement>>(undefined)
  const currentId = React.useRef(0)
  const lastMousePos = React.useRef({ x: 0, y: 0 })
  const cachedMousePos = React.useRef({ x: 0, y: 0 })
  const [containerRef, animate] = useAnimate()
  const zIndices = React.useRef<number[]>([])

  const clampedIntensity = useMemo(
    () => Math.max(0.0001, Math.min(1, intensity)),
    [intensity]
  )

  useEffect(() => {
    allImages.current = containerRef?.current?.querySelectorAll(
      ".image-trail-item"
    ) as NodeListOf<HTMLElement>

    zIndices.current = Array.from(
      { length: allImages.current.length },
      (_, index) => index
    )
  }, [containerRef, allImages])

  const handleMouseMove = (e: React.MouseEvent) => {
    const containerRect = containerRef?.current?.getBoundingClientRect()
    const mousePos = {
      x: e.clientX - (containerRect?.left || 0),
      y: e.clientY - (containerRect?.top || 0),
    }

    cachedMousePos.current.x = MathUtils.lerp(
      cachedMousePos.current.x || mousePos.x,
      mousePos.x,
      clampedIntensity
    )

    cachedMousePos.current.y = MathUtils.lerp(
      cachedMousePos.current.y || mousePos.y,
      mousePos.y,
      clampedIntensity
    )

    const distance = MathUtils.distance(
      mousePos.x,
      mousePos.y,
      lastMousePos.current.x,
      lastMousePos.current.y
    )

    if (distance > threshold && allImages?.current) {
      const N = allImages.current.length
      const current = currentId.current

      if (zIndexDirection === "new-on-top") {
        // Shift others down, put current on top
        for (let i = 0; i < N; i++) {
          if (i !== current) {
            zIndices.current[i] -= 1
          }
        }
        zIndices.current[current] = N - 1
      } else {
        // Shift others up, put current at bottom
        for (let i = 0; i < N; i++) {
          if (i !== current) {
            zIndices.current[i] += 1
          }
        }
        zIndices.current[current] = 0
      }

      allImages.current[current].style.display = "block"
      allImages.current.forEach((img, index) => {
        img.style.zIndex = String(zIndices.current[index] + baseZIndex)
      })

      animate(
        allImages.current[currentId.current],
        {
          x: [
            cachedMousePos.current.x -
              allImages.current[currentId.current].offsetWidth / 2,
            mousePos.x - allImages.current[currentId.current].offsetWidth / 2,
          ],
          y: [
            cachedMousePos.current.y -
              allImages.current[currentId.current].offsetHeight / 2,
            mousePos.y -
              allImages.current?.[currentId.current].offsetHeight / 2,
          ],
          ...keyframes,
        },
        {
          ...trailElementAnimationKeyframes.x,
          ...trailElementAnimationKeyframes.y,
          ...keyframesOptions,
        }
      )
      currentId.current = (current + 1) % N
      lastMousePos.current = { x: mousePos.x, y: mousePos.y }
    }
  }

  const ElementTag = as ?? "div"

  return (
    <ElementTag
      className={cn("h-full w-full relative", className)}
      onMouseMove={handleMouseMove}
      ref={containerRef}
      {...props}
    >
      {Array.from({ length: repeatChildren }).map(() => (
        <>{children}</>
      ))}
    </ElementTag>
  )
}

export const ImageTrailItem = ({
  className,
  children,
  as = "div",
  ...props
}: ImageTrailItemProps) => {
  const ElementTag = as ?? "div"
  return (
    <ElementTag
      {...props}
      className={cn(
        "absolute top-0 left-0 will-change-transform hidden",
        className,
        "image-trail-item"
      )}
    >
      {children}
    </ElementTag>
  )
}

export default ImageTrail
```





## Introduction

This component is a fun mouse interaction effect. The idea is to follow the mouse and show a trail of random images. It's a kind of brutalist effect and there are various possibilities when it comes to showing and hiding the images.

## Usage

Wrap each image with the `ImageTrailItem` component and wrap everything with the `ImageTrail` as a parent component.


**File**: `Image trail usage example`
```tsx
<ImageTrail>
  <ImageTrailItem>...</ImageTrailItem>
  <ImageTrailItem>...</ImageTrailItem>
  <ImageTrailItem>...</ImageTrailItem>
</ImageTrail>
```


## Understanding the component

The `ImageTrail` component creates its effect by tracking the mouse (or touch) position and animating a series of child elements (`ImageTrailItem`) to follow that movement with a configurable delay and visual style.

### How the Trail Follows the Cursor

1.  **Tracking Position:** The component continuously monitors the mouse or touch position.
2.  **Calculating Target Position (Linear Interpolation):** Instead of instantly moving trail items to the current cursor position, it calculates a "target" position using linear interpolation (`lerp`). This smooths out the movement.


**File**: `Calculating target position`
```tsx
    const lerp = (a: number, b: number, n: number) => (1 - n) * a + n * b

    //...

    cachedMousePos.current.x = MathUtils.lerp(
      cachedMousePos.current.x || mousePos.x,
      mousePos.x,
      clampedIntensity
    )

    cachedMousePos.current.y = MathUtils.lerp(
      cachedMousePos.current.y || mousePos.y,
      mousePos.y,
      clampedIntensity
    )
    ```


3.  **Controlling Responsiveness (`intensity`):** The `intensity` prop (a value between 0 and 1) controls how quickly the calculated target position updates to match the actual cursor position.
    *   Lower values (e.g., 0.1) result in a smoother, more delayed "momentum" effect, where the trail items lag behind the cursor.
    *   Higher values (e.g., 0.8) make the trail more responsive but less smooth.
    *   An intensity of `1` positions the trail items exactly at the cursor's current position.


### Demo Code (image-trail-instant-demo.tsx)
```tsx
import ImageTrail,{
  ImageTrailItem,
} from "@/fancy/components/image/image-trail"

const images = [
  "https://cdn.cosmos.so/7dc46d69-ad3b-4942-ab84-511ae786892e?format=jpeg",
  "https://cdn.cosmos.so/cb5c5995-4ba7-4519-a0e8-aa6427cc0a90?format=jpeg", 
  "https://cdn.cosmos.so/264d0ae9-f2e9-4deb-843c-229e70bbe4cc?format=jpeg",
  "https://cdn.cosmos.so/d9ed5da2-92b8-4b71-84e8-7bef645c44dc?format=jpeg",
  "https://cdn.cosmos.so/223c2fad-7dcb-46e0-9f00-826edcf8d7b1?format=jpeg",
  "https://cdn.cosmos.so/48e640ee-75e1-4390-a34a-b790785f033a?format=jpeg",
  "https://cdn.cosmos.so/d9c19f7c-d605-4257-8546-dafe0daa250f.?format=jpeg",
  "https://cdn.cosmos.so/5ff27be0-bed8-4779-b520-6896e68e7e4d?format=jpeg",
  "https://cdn.cosmos.so/9098d86e-b3f7-425f-be96-40df45c82342?format=jpeg",
  "https://cdn.cosmos.so/5c01be2f-57fe-4a1a-91ad-a37b9426e080?format=jpeg",
  "https://cdn.cosmos.so/6a1d9c63-5b32-4a22-b03e-5dc63164ad8a?format=jpeg",
]

const ImageTrailDemo = () => {
  return (
    <div >
      <ImageTrail
        threshold={100}
        intensity={1}
        keyframes={{ scale: [1, 1] }}
        keyframesOptions={{
          scale: { duration: 1, times: [1, 1] },
        }}
        repeatChildren={1}
      >
        {images.map((url, index) => (
          <ImageTrailItem key={index}>
            <div >
              <img src={url} alt="image"  />
            </div>
          </ImageTrailItem>
        ))}
      </ImageTrail>
      <p >
        move your cursor
      </p>
    </div>
  )
}

export default ImageTrailDemo
```


4.  **Triggering Animation (`threshold`):** An animation cycle for the next trail item is only triggered when the cursor moves a certain distance. This distance is calculated using the Pythagorean theorem (`Math.hypot`):


**File**: `Calculating distance`
```tsx
    const distance = (x1: number, y1: number, x2: number, y2: number) =>
      Math.hypot(x2 - x1, y2 - y1)
    ```


    The `threshold` prop (default: `100` pixels) defines this minimum distance. No new trail items are animated until the cursor has moved at least this far since the last item was triggered.

### Animating the Trail Items

When the movement threshold is met:

1.  **Cycling Through Items:** The component activates the next available `ImageTrailItem` in the sequence. By default, it cycles through all children and repeats from the beginning. You can set the `repeatChildren` prop to a number greater than 1 to duplicate the children internally and avoid immediate repetition.
2.  **Making Items Visible:** Trail items are initially hidden (`display: none`). When triggered, the next item is made visible (`display: block`) and starts its animation.
3.  **Animating Position:** The core movement animation uses the `animate` function from `Motion`. Each triggered item animates its `x` and `y` coordinates towards the continuously updating `cachedMousePos` (calculated using `lerp` as described above).

### Customizing the Visual Animation

Beyond the position animation, you can control how each `ImageTrailItem` visually appears and disappears using the `keyframes` and `keyframesOptions` props passed to the main `ImageTrail` component.

*   **`keyframes`:** Define the states of the animation (e.g., scale, opacity). You should define the initial state (how the element appears) and the final state (how it disappears).
*   **`keyframesOptions`:** Fine-tune the timing, duration, and easing for the properties defined in `keyframes`. The `times` array within options specifies the progress points (0 to 1) at which each keyframe state should be reached.

**Example:**


**File**: `Custom keyframes`
```tsx
keyframes={{ scale: [0, 1, 1, 0], opacity: [0, 1, 1, 0] }}
keyframesOptions={{
  duration: 0.6, // Total duration for one item's animation
  scale: { times: [0, 0.1, 0.7, 1] }, // Scale keyframe timings
  opacity: { times: [0, 0.1, 0.7, 1] }, // Opacity keyframe timings
}}
```


In this example:
*   The item starts invisible (`opacity: 0`, `scale: 0`).
*   At 10% of the duration (`0.06s`), it quickly scales up and fades in (`opacity: 1`, `scale: 1`).
*   It remains fully visible until 70% of the duration (`0.42s`).
*   From 70% to 100% (`0.42s` to `0.6s`), it scales down and fades out back to the initial state (`opacity: 0`, `scale: 0`).

There must be the same number of values in a `times` array as there are keyframes for that property. If `times` is omitted, the keyframes are spread evenly across the `duration`. You can read more about this in the [Motion docs](https://motion.dev/docs/animate#times).

The component reuses DOM elements rather than creating new ones for each animation. It maintains a fixed set of `ImageTrailItem` elements that get recycled as needed. When an item needs to be triggered again, the component updates its position and restarts the animation without having to remove and remount elements in the DOM. For this recycling to work properly, make sure to define both initial and final states in the `keyframes` prop, as explained in the example above.

### Z Index Stacking

The component automatically manages the `z-index` of the trail items to control their stacking order. You can customize this behavior using two props:

- `zIndexDirection`: Controls whether newer or older items should appear on top
  - `"new-on-top"` (default): The most recently triggered item will have the highest z-index
  - `"old-on-top"`: The oldest items stay on top, with new items appearing underneath

- `baseZIndex`: Sets the starting z-index value (defaults to 0)

When a new item is triggered, the component maintains proper stacking by:
- For `"new-on-top"`: Setting the current item to the highest z-index and shifting all others down by 1
- For `"old-on-top"`: Setting the current item to the lowest z-index (baseZIndex) and shifting all others up by 1

Example with `zIndexDirection="old-on-top"`:


### Demo Code (image-trail-zindex-demo.tsx)
```tsx
import ImageTrail,{
  ImageTrailItem,
} from "@/fancy/components/image/image-trail"

const images = [
  "https://cdn.cosmos.so/7dc46d69-ad3b-4942-ab84-511ae786892e?format=jpeg",
  "https://cdn.cosmos.so/cb5c5995-4ba7-4519-a0e8-aa6427cc0a90?format=jpeg",
  "https://cdn.cosmos.so/264d0ae9-f2e9-4deb-843c-229e70bbe4cc?format=jpeg",
  "https://cdn.cosmos.so/d9ed5da2-92b8-4b71-84e8-7bef645c44dc?format=jpeg",
  "https://cdn.cosmos.so/223c2fad-7dcb-46e0-9f00-826edcf8d7b1?format=jpeg",
  "https://cdn.cosmos.so/48e640ee-75e1-4390-a34a-b790785f033a?format=jpeg",
  "https://cdn.cosmos.so/d9c19f7c-d605-4257-8546-dafe0daa250f.?format=jpeg",
  "https://cdn.cosmos.so/5ff27be0-bed8-4779-b520-6896e68e7e4d?format=jpeg",
  "https://cdn.cosmos.so/9098d86e-b3f7-425f-be96-40df45c82342?format=jpeg",
  "https://cdn.cosmos.so/5c01be2f-57fe-4a1a-91ad-a37b9426e080?format=jpeg",
  "https://cdn.cosmos.so/6a1d9c63-5b32-4a22-b03e-5dc63164ad8a?format=jpeg",
]

const ImageTrailDemo = () => {
  return (
    <div >
      <ImageTrail
        threshold={1}
        intensity={1}
        
        keyframes={{ scale: [1, 0], rotateX: [0, -90], rotateY: [0, -90], rotateZ: [0, 360] }}
        keyframesOptions={{
          scale: { duration: 1, type: "tween", ease: "easeOut" },
          rotateZ: { duration: 3, type: "tween", ease: "easeOut" },
          rotateY: { duration: 3, type: "tween", ease: "easeOut" },
          rotateX: { duration: 3, type: "tween", ease: "easeOut" },
        }}
        repeatChildren={10}
        zIndexDirection="old-on-top"
      >
        {images.map((url, index) => (
          <ImageTrailItem key={index} >
            <div >
              <img src={url} alt="image"  />
            </div>
          </ImageTrailItem>
        ))}
      </ImageTrail>
      <p >
        move your cursor
      </p>
    </div>
  )
}

export default ImageTrailDemo
```


That's it! :)

### Non-image elements

The component is not constrained to be used with images, you can wrap videos, svgs, or basically any HTML elements inside a `ImageTrailItem`.


### Demo Code (image-trail-various-elements-demo.tsx)
```tsx
import Image from "next/image"

import ImageTrail,{
  ImageTrailItem,
} from "@/fancy/components/image/image-trail"

const ImageTrailVariousElementsDemo = () => {
  return (
    <div >
      <ImageTrail
        threshold={60}
        keyframes={{ opacity: [0, 1, 1, 0], scale: [1, 1, 0] }}
        keyframesOptions={{
          opacity: { duration: 1, times: [0, 0.001, 0.9, 1] },
          scale: { duration: 1, times: [0, 0.8, 1] },
        }}
      >
        <ImageTrailItem>
          <div >
            <Image
              src={
                "https://images.unsplash.com/photo-1727341554370-80e0fe9ad082?q=80&w=2276&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
              }
              alt=""
              fill
              
            />
          </div>
        </ImageTrailItem>
        <ImageTrailItem>
          <div >
            <svg
              
              viewBox="0 0 107 243"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M90.6323 71.1433C95.9279 63.8693 101.283 56.5333 104.965 48.2748C105.355 47.1008 106.388 45.9327 106.181 44.6678C105.859 43.3526 103.93 43.0857 103.28 44.2887C100.187 51.4196 96.0129 56.3747 93.3873 59.7922C88.1477 66.6601 85.7113 69.8668 79.8443 76.9862C77.933 79.6185 75.3537 82.6433 72.6644 86.2504C73.4926 81.4036 74.2088 76.5375 74.7996 71.6559C74.9251 71.5843 75.0448 71.4992 75.1471 71.389C75.0911 71.4315 75.0545 71.4605 75.0178 71.4857C75.0583 71.4547 75.1066 71.418 75.1568 71.3793C75.2031 71.3503 75.3267 71.2594 75.4638 71.1433C75.7089 70.9751 75.8807 70.8552 75.9985 70.7681C76.2997 70.6115 76.5796 70.4007 76.8325 70.186C77.9947 69.4549 77.124 69.9752 79.2554 68.4163C80.7323 67.5769 89.335 58.7981 92.8661 54.05C96.8952 48.5765 99.7197 43.8168 101.515 38.4285C102.253 36.2623 102.907 34.0652 103.403 31.8313C103.538 30.903 104.054 29.8876 103.531 29.0056C102.811 27.6885 100.673 27.9554 100.316 29.4273C97.4184 40.5907 91.3679 49.4836 83.3965 57.7905C81.2478 60.1094 78.9407 62.2621 76.6124 64.3973C76.4097 64.6178 76.2186 64.8286 76.0371 65.0297C79.2419 52.6478 84.5298 40.9254 87.8581 28.5879C89.3543 22.923 90.1323 17.0995 90.5571 11.2624C90.6111 8.70172 91.1363 5.584 90.7501 3.12385C90.3756 2.0311 88.8002 1.74872 88.0222 2.58231C87.2364 3.45458 87.5627 4.76202 87.3697 5.8393C86.6534 15.6122 85.831 23.1241 82.7382 32.7017C80.9003 38.4304 78.765 44.1069 76.8287 49.8337C76.9522 47.4799 77.0353 45.1532 77.0565 42.9445C77.2804 37.4982 77.319 32.0421 76.5159 26.6364C75.7205 20.0161 74.093 13.5253 72.0929 7.17381C71.3091 4.73301 70.9713 3.853 70.396 2.39277C69.8824 1.37158 70.477 1.6733 69.2434 0.630829C68.1275 -0.0518993 66.5676 0.592147 66.2355 1.85703C66.0618 2.42178 66.2336 2.9788 66.3977 3.52227C66.7278 4.69626 67.0425 5.87411 67.3784 7.04809C70.8072 18.7009 73.342 28.1198 73.0969 39.6856C73.1471 46.9693 72.9327 54.2415 72.342 61.502C72.3227 61.8385 72.3053 62.177 72.2879 62.5135C70.9809 59.8387 69.4345 56.7287 69.1932 55.9318C66.9672 50.3172 65.4285 44.4492 63.9343 38.6045C62.2392 31.6263 61.3222 24.4934 60.1137 17.4244C59.5441 15.4652 56.5826 16.2446 57.0903 18.2599C58.824 31.5296 61.2681 48.2594 66.8861 62.0996C67.3205 63.2272 69.5832 68.3428 70.6488 69.0062C70.9828 69.2886 71.3477 69.4356 71.7107 69.4839C70.784 76.5452 69.3226 83.9063 67.4943 91.366C67.3167 90.3622 67.1371 89.3585 66.9595 88.3547C65.7896 81.1425 64.0791 74.0174 61.9439 67.0315C59.8106 60.6413 57.2892 54.3846 55.1269 48.0021C53.2736 42.401 51.592 36.7439 49.87 31.1022C49.395 29.3847 46.7212 30.0655 47.1961 31.841C49.8815 43.2694 53.1616 54.5277 57.0421 65.6061C59.8588 74.5918 61.9574 83.7941 63.7316 93.0371C64.2528 96.031 64.4439 97.0174 64.6158 99.2783C64.7393 99.9417 64.6119 100.667 64.8609 101.296C64.2374 103.493 63.5791 105.69 62.9072 107.885C62.7759 105.903 62.5906 103.976 62.357 102.305C61.1909 96.1896 60.3106 89.989 58.5673 84.003C56.9456 79.1446 55.1752 74.3268 53.1964 69.6038C52.2195 67.2965 50.4395 63.8113 49.4452 61.0765C49.1981 60.4672 49.0842 59.7284 48.4587 59.388C47.3447 58.6918 45.8292 59.8387 46.2231 61.1113C47.3003 64.2348 48.6421 67.2636 49.6827 70.4007C51.177 75.3383 52.5477 80.3147 54.0574 85.2505C57.0575 98.2436 58.4186 106.268 59.2082 116.524C59.2622 117.395 59.2487 118.358 59.2584 119.166C58.7043 120.793 58.1386 122.412 57.5672 124.023C57.154 122.816 56.7371 121.613 56.3896 120.391C54.0246 112.09 51.2658 103.912 48.4278 95.7622C47.6189 93.4548 46.6999 91.1881 45.9431 88.8614C45.4894 87.8402 45.5261 86.4303 44.5242 85.7707C43.3349 85.0261 41.7209 86.2543 42.1399 87.6081C44.0106 93.4587 45.4856 99.4098 46.9914 105.361C49.3043 113.262 52.6519 125.288 54.9802 130.926C54.9879 130.958 55.0053 130.984 55.015 131.016C54.2678 133.016 53.5168 135.003 52.7542 136.96C51.7059 139.882 50.6769 142.689 49.6556 145.427C49.3332 142.354 48.895 139.2 48.4742 136.801C47.0687 129.003 45.6941 121.184 43.75 113.5C43.084 110.877 42.3214 108.278 41.7036 105.641C41.248 104.173 41.219 102.497 40.4854 101.151C39.4718 99.7193 37.1068 100.891 37.6107 102.586C39.0316 109.233 39.2093 112.409 40.3078 118.466C42.2326 129.195 44.7076 142.397 45.7636 151.2C45.9586 153.251 45.9779 152.744 46.059 154.79C45.1265 157.142 44.1825 159.455 43.2172 161.757C41.4024 149.137 38.7961 136.645 35.6705 124.284C35.3037 122.665 34.9253 121.05 34.5121 119.443C34.3905 118.996 34.0913 118.611 33.6955 118.377C32.3924 117.561 30.6239 118.907 31.0815 120.391C33.9098 135.192 37.9119 157.976 39.3231 168.273C39.3231 168.866 39.493 169.352 39.7653 169.723C38.3791 172.816 36.9234 175.937 35.3848 179.123C35.1434 175.636 34.6994 172.158 34.3249 168.683C33.2939 159.179 30.622 149.984 28.2686 140.751C27.923 139.335 27.5987 137.915 27.3033 136.488C27.1933 136.08 26.9191 135.73 26.5581 135.517C25.3689 134.774 23.7549 135.999 24.1738 137.354C26.2183 149.073 29.1393 162.606 29.9637 172.684C30.4637 177.133 30.8576 181.604 30.6761 186.085C30.6529 187.064 30.5853 188.039 30.512 189.014C24.6121 200.537 16.4785 213.977 11.6791 222.193C8.88358 227.067 6.10163 231.954 3.10535 236.706C2.5088 237.679 1.90644 238.646 1.2983 239.611C0.773184 240.303 0.678612 241.354 1.27709 242.029C3.55712 244.342 4.94517 241.011 6.49929 239.388C7.48969 238.131 10.847 233.778 12.7293 230.89C20.9652 218.843 29.0544 205.099 36.4022 190.638C47.4509 182.122 64.1177 170.067 77.3461 158.415C78.9871 157.039 80.6165 155.649 82.2806 154.301C83.0258 153.57 84.1726 153.131 84.6167 152.16C85.3812 150.593 83.4235 148.868 81.9505 149.808C71.4462 157.457 59.4437 167.296 49.2715 174.864C46.5764 176.916 43.8619 178.941 41.1514 180.972C42.5646 177.995 43.945 175.001 45.281 171.992C45.3678 171.924 45.4508 171.851 45.5281 171.767C50.4704 168.259 62.8609 159.6 70.0407 153.552C76.5912 148.017 82.8946 142.116 88.6574 135.759C90.1246 134.246 87.9122 131.984 86.3696 133.467C79.2342 140.876 70.228 147.748 65.3707 151.494C61.7643 154.169 58.1927 156.896 54.5536 159.525C52.7041 160.795 50.841 162.053 49.0012 163.339C49.6518 161.768 50.2908 160.198 50.9144 158.625C51.4183 158.221 51.8797 157.626 52.8624 156.629C60.4554 148.86 68.0561 141.087 75.9271 133.598C79.6338 129.96 83.1533 126.139 86.7384 122.383C89.14 119.838 91.9819 117.041 93.3719 115.138C93.9028 114.258 94.9685 113.416 94.6924 112.295C94.3873 111.053 92.5726 110.803 91.9548 111.937C89.8524 115.006 84.1205 119.4 79.8559 123.574C71.4945 131.363 63.2064 139.221 55.1675 147.342C55.3509 146.826 55.5478 146.307 55.7293 145.791C57.3413 141.196 58.9958 136.612 60.6098 132.013C61.2256 131.523 61.7797 130.906 62.4091 130.469C65.1003 128.224 67.8688 126.069 70.533 123.791C76.6472 118.917 82.5451 113.766 87.8851 108.04C90.6381 105.057 93.2367 101.934 95.7755 98.7677C96.6674 97.6769 98.3528 95.4508 98.4126 95.3367C98.7717 94.7255 99.4165 94.2072 99.5111 93.4819C99.8026 91.9559 97.54 91.1726 96.8025 92.5439C89.2076 102.208 75.5912 113.49 63.5713 123.319C64.6235 120.118 65.6313 116.904 66.556 113.662C66.6661 113.558 66.7761 113.451 66.9151 113.327C69.9693 110.204 73.0235 107.075 76.0796 103.951C81.9525 97.8896 88.0956 92.041 93.2232 85.3124C95.6461 82.2198 96.8855 80.4656 99.4397 76.8411C100.712 74.8819 102.521 72.9691 103.399 70.8339C103.681 69.3582 101.49 68.5942 100.776 69.9249C97.6578 74.2088 92.5205 80.0343 88.1786 84.8675C81.854 91.4105 75.3576 97.7871 68.9847 104.284C69.5272 102 70.0485 99.7096 70.535 97.4139C78.3616 86.1924 82.3289 81.8233 90.6362 71.1453L90.6323 71.1433Z"
                fill="black"
              />
            </svg>
          </div>
        </ImageTrailItem>
        <ImageTrailItem>
          <div >
            <video
              src={
                "https://cdn.cosmos.so/96ae0b34-289d-489d-94a1-c68925ddd3a9.mp4"
              }
              
              autoPlay
              muted
              playsInline
              loop
            />
          </div>
        </ImageTrailItem>
        <ImageTrailItem>
          <div >
            Hey, this is just a simple div
          </div>
        </ImageTrailItem>
      </ImageTrail>
      <p >
        move your cursor
      </p>
    </div>
  )
}

export default ImageTrailVariousElementsDemo
```


## Notes

- The `ImageTrailItem` component assigns a default className of `.image-trail-item` to identify elements for animation within the `ImageTrail` component. Be cautious when applying custom `className` values with the same name (`.image-trail-item`) in your application, as this may cause conflicts or unintended behavior due to duplicate class selectors. To avoid issues, ensure custom classes are unique or use the `className` prop to extend styles without overriding the default `.image-trail-item` class.

- When using the `ImageTrail` component, content is heavily animated. To prevent performance issues, avoid using overly large images or videos.

## Props

### Image Trail Wrapper


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The content to be displayed |
| threshold | `number` | `100` | How much distance in pixels the mouse has to travel to trigger an element to appear |
| as | `ElementType` | `div` | HTML Tag |
| intensity | `number` | `0.3` | The intensity for the momentum movement after showing the element. The value will be clamped greater than 0 and less than or equal to 1.0 |
| keyframes | `DOMKeyframesDefinition` | - | Animation Keyframes for defining the animation sequence. Example: `{ scale: [0, 1, 1, 0] }` |
| keyframesOptions | `AnimationOptions` | - | Options for the animation/keyframes. Example: `{ duration: 1, times: [0, 0.1, 0.9, 1] }` |
| trailElementAnimationKeyframes | `{ x?: AnimationOptions, y?: AnimationOptions }` | `{ x: { duration: 1, type: "tween", ease: "easeOut" }, y: { duration: 1, type: "tween", ease: "easeOut" } }` | Animation keyframes for the x and y positions after showing the element. Describes how the element should try to arrive at the mouse position |
| repeatChildren | `number` | `3` | The number of times the children will be repeated |
| baseZIndex | `number` | `0` | The base zIndex for all elements |
| zIndexDirection | `"new-on-top" | "old-on-top"` | `"new-on-top"` | Controls stacking order behavior. "new-on-top": newer elements stack above older ones, "old-on-top": older elements stay visually on top |
| className | `string` | - | Additional CSS class names |


### Image Trail Item


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The content to be displayed |
| as | `ElementType` | `div` | HTML Tag |
| className | `string` | - | Additional CSS class names |


---

### Parallax Floating <a name="parallax-floating"></a>

### Demo Code (parallax-floating-demo.tsx)
```tsx
"use client"

import { useEffect } from "react"
import { exampleImages } from "@/utils/demo-images"
import { motion, stagger, useAnimate } from "motion/react"

import Floating, {
  FloatingElement,
} from "@/fancy/components/image/parallax-floating"

const Preview = () => {
  const [scope, animate] = useAnimate()

  useEffect(() => {
    animate("img", { opacity: [0, 1] }, { duration: 0.5, delay: stagger(0.15) })
  }, [])

  return (
    <div
      
      ref={scope}
    >
      <motion.div
        
        initial={{ opacity: 0, y: 10 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.88, delay: 1.5 }}
      >
        <p >
          fancy.
        </p>
        <p >
          Download
        </p>
      </motion.div>

      <Floating sensitivity={-1} >
        <FloatingElement depth={0.5} >
          <motion.img
            initial={{ opacity: 0 }}
            src={exampleImages[0].url}
            
          />
        </FloatingElement>
        <FloatingElement depth={1} >
          <motion.img
            initial={{ opacity: 0 }}
            src={exampleImages[1].url}
            
          />
        </FloatingElement>
        <FloatingElement depth={2} >
          <motion.img
            initial={{ opacity: 0 }}
            src={exampleImages[2].url}
            
          />
        </FloatingElement>
        <FloatingElement depth={1} >
          <motion.img
            initial={{ opacity: 0 }}
            src={exampleImages[3].url}
            
          />
        </FloatingElement>

        <FloatingElement depth={1} >
          <motion.img
            initial={{ opacity: 0 }}
            src={exampleImages[4].url}
            
          />
        </FloatingElement>
        <FloatingElement depth={2} >
          <motion.img
            initial={{ opacity: 0 }}
            src={exampleImages[7].url}
            
          />
        </FloatingElement>

        <FloatingElement depth={4} >
          <motion.img
            initial={{ opacity: 0 }}
            src={exampleImages[5].url}
            
          />
        </FloatingElement>
        <FloatingElement depth={1} >
          <motion.img
            initial={{ opacity: 0 }}
            src={exampleImages[6].url}
            
          />
        </FloatingElement>
      </Floating>
    </div>
  )
}

export default Preview
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/parallax-floating
```





Create a hook for querying the cursor position:


*Component Source: use-mouse-position (Source file not found)*


Then copy and paste the following code into your project:


### Component Source Code (parallax-floating.tsx)
```tsx
"use client"

import {
  createContext,
  ReactNode,
  useCallback,
  useContext,
  useEffect,
  useRef,
} from "react"
import { useAnimationFrame } from "motion/react"

import { cn } from "@/lib/utils"
import { useMousePositionRef } from "@/hooks/use-mouse-position-ref"

interface FloatingContextType {
  registerElement: (id: string, element: HTMLDivElement, depth: number) => void
  unregisterElement: (id: string) => void
}

const FloatingContext = createContext<FloatingContextType | null>(null)

interface FloatingProps {
  children: ReactNode
  className?: string
  sensitivity?: number
  easingFactor?: number
}

const Floating = ({
  children,
  className,
  sensitivity = 1,
  easingFactor = 0.05,
  ...props
}: FloatingProps) => {
  const containerRef = useRef<HTMLDivElement>(null)
  const elementsMap = useRef(
    new Map<
      string,
      {
        element: HTMLDivElement
        depth: number
        currentPosition: { x: number; y: number }
      }
    >()
  )
  const mousePositionRef = useMousePositionRef(containerRef)

  const registerElement = useCallback(
    (id: string, element: HTMLDivElement, depth: number) => {
      elementsMap.current.set(id, {
        element,
        depth,
        currentPosition: { x: 0, y: 0 },
      })
    },
    []
  )

  const unregisterElement = useCallback((id: string) => {
    elementsMap.current.delete(id)
  }, [])

  useAnimationFrame(() => {
    if (!containerRef.current) return

    elementsMap.current.forEach((data) => {
      const strength = (data.depth * sensitivity) / 20

      // Calculate new target position
      const newTargetX = mousePositionRef.current.x * strength
      const newTargetY = mousePositionRef.current.y * strength

      // Check if we need to update
      const dx = newTargetX - data.currentPosition.x
      const dy = newTargetY - data.currentPosition.y

      // Update position only if we're still moving
      data.currentPosition.x += dx * easingFactor
      data.currentPosition.y += dy * easingFactor

      data.element.style.transform = `translate3d(${data.currentPosition.x}px, ${data.currentPosition.y}px, 0)`
    })
  })

  return (
    <FloatingContext.Provider value={{ registerElement, unregisterElement }}>
      <div
        ref={containerRef}
        className={cn("absolute top-0 left-0 w-full h-full", className)}
        {...props}
      >
        {children}
      </div>
    </FloatingContext.Provider>
  )
}

export default Floating

interface FloatingElementProps {
  children: ReactNode
  className?: string
  depth?: number
}

export const FloatingElement = ({
  children,
  className,
  depth = 1,
}: FloatingElementProps) => {
  const elementRef = useRef<HTMLDivElement>(null)
  const idRef = useRef(Math.random().toString(36).substring(7))
  const context = useContext(FloatingContext)

  useEffect(() => {
    if (!elementRef.current || !context) return

    const nonNullDepth = depth ?? 0.01

    context.registerElement(idRef.current, elementRef.current, nonNullDepth)
    return () => context.unregisterElement(idRef.current)
  }, [depth])

  return (
    <div
      ref={elementRef}
      className={cn("absolute will-change-transform", className)}
    >
      {children}
    </div>
  )
}
```





## Usage

There are two components exported from the source file: `Floating` and `FloatingElement`. The first one is a wrapper component that takes care of the animation, mouse position tracking and other logic. The second one is a component that you **must** use to wrap any elements you want to float.


**File**: `Floating Example`
```tsx
<Floating>
  <FloatingElement depth={0.5}>
    <div  />
  </FloatingElement>
  <FloatingElement depth={1}>
    <div  />
  </FloatingElement>
  <FloatingElement depth={2}>
    <div  />
  </FloatingElement>
</Floating>
```


The advantage of this setup is that you can style and position your elements however you want using Tailwind classes or custom CSS directly on the `FloatingElement` component, while the `Floating` wrapper component handles all the complex animation logic. Simply wrap your positioned elements with `FloatingElement`, set their `depth` value, and the floating effect will be applied while maintaining your original styling and positioning.

## Understanding the component

If you're curious how it works, here's a quick overview of the component's internals:

1. **Element Registration**: Using React Context, each `FloatingElement` child registers itself with the parent `Floating` component, providing its DOM reference and depth value.

2. **Mouse Position Tracking**: The component tracks mouse movement across the screen using a custom hook that provides normalized coordinates relative to the container.

3. **Animation Loop**: Using Framer Motion's `useAnimationFrame`, the component runs a continuous animation loop that:

   - Calculates the target position for each element based on the mouse coordinates
   - Applies linear interpolation (lerp) to smoothly transition elements to their new positions
   - Updates the transform property of each element using CSS transforms

4. **Strength**: The floating effect is customized through two main factors:
   - Individual `depth` values on each `FloatingElement` determine how far that element moves. The higher the depth, the farther the element will move.
   - The global `sensitivity` prop controls the overall intensity of the movement
5. **Lerp**: The `easingFactor` prop determines how quickly elements move toward their target positions - lower values create smoother, more gradual movements while higher values create snappier responses.

## Notes

### Z-Index Management

The `Floating` component focuses solely on movement animation and does not handle z-index stacking. You'll need to manually set appropriate z-index values on your `FloatingElement` components to achieve the desired layering effect. The `depth` prop only controls the intensity of the floating movement, not the visual stacking order.

### Performance Optimization

For better performance when dealing with multiple floating elements, you can use a grouping strategy:

1. Instead of creating individual `FloatingElement` components for each item, group related items under a single `FloatingElement`
2. All children of a `FloatingElement` will move together with the same depth value
3. This reduces the number of elements being calculated and transformed

For example, if you have 6 floating images, instead of creating 6 separate `FloatingElement` components, you could group them into 3 pairs. This reduces the animation calculations from 6 to 3.

### Directional Control

With the `depth` and `sensitivity` props, you can control the direction, and strength of the floating effect:

- **Positive Values**: Elements move toward the mouse cursor

  - Higher values create stronger movement
  - Example: `depth={2}` moves twice as far as `depth={1}`

- **Negative Values**: Elements move away from the mouse cursor
  - Creates an inverse floating effect
  - Example: `depth={-1}` moves in the opposite direction of the mouse

## Credits

Ported to Framer by [Framer University](https://framer.university/)

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The content to be displayed |
| sensitivity | `number` | `0.1` | The sensitivity of the movement |
| easingFactor | `number` | `0.05` | The easing factor of the movement |
| className | `string` | - | Additional CSS classes for styling |


### FloatingElement


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The content to be displayed |
| depth | `number` | `1` | The depth of the element |
| className | `string` | - | Additional CSS classes for styling |


---

## Filter <a name="filter"></a>

### Gooey SVG Filter <a name="gooey-svg-filter"></a>

### Demo Code (gooey-svg-filter-demo.tsx)
```tsx
import { useState } from "react"
import { AnimatePresence, motion } from "motion/react"

import useDetectBrowser from "@/hooks/use-detect-browser"
import useScreenSize from "@/hooks/use-screen-size"
import { Button } from "@/components/ui/button"
import GooeySvgFilter from "@/fancy/components/filter/gooey-svg-filter"

const TAB_CONTENT = [
  {
    title: "2024",
    files: [
      "learning-to-meditate.md",
      "spring-garden-plans.md",
      "travel-wishlist.md",
      "new-coding-projects.md",
    ],
  },
  {
    title: "2023",
    files: [
      "year-in-review.md",
      "marathon-training-log.md",
      "recipe-collection.md",
      "book-reflections.md",
    ],
  },
  {
    title: "2022",
    files: [
      "moving-to-a-new-city.md",
      "starting-a-blog.md",
      "photography-basics.md",
      "first-coding-project.md",
    ],
  },
  {
    title: "2021",
    files: [
      "goals-and-aspirations.md",
      "daily-gratitude.md",
      "learning-to-cook.md",
      "remote-work-journal.md",
    ],
  },
]

export default function GooeyDemo() {
  const [activeTab, setActiveTab] = useState(0)
  const [isGooeyEnabled, setIsGooeyEnabled] = useState(true)
  const screenSize = useScreenSize()
  const browserName = useDetectBrowser()
  const isSafari = browserName === "Safari"

  return (
    <div >
      <GooeySvgFilter
        id="gooey-filter"
        strength={screenSize.lessThan("md") ? 8 : 15}
      />

      <Button
        variant="outline"
        onClick={() => setIsGooeyEnabled(!isGooeyEnabled)}
        
      >
        {isGooeyEnabled ? "Disable filter" : "Enable filter"}
      </Button>

      <div >
        <div
          
          style={{ filter: isGooeyEnabled ? "url(#gooey-filter)" : "none" }}
        >
          <div >
            {TAB_CONTENT.map((_, index) => (
              <div key={index} >
                {activeTab === index && (
                  <motion.div
                    layoutId="active-tab"
                    
                    transition={{
                      type: "spring",
                      bounce: 0.0,
                      duration: isSafari ? 0 : 0.4,
                    }}
                  />
                )}
              </div>
            ))}
          </div>
          {/* Content panel */}
          <div >
            <AnimatePresence mode="popLayout">
              <motion.div
                key={activeTab}
                initial={{
                  opacity: 0,
                  y: 50,
                  filter: "blur(10px)",
                }}
                animate={{
                  opacity: 1,
                  y: 0,
                  filter: "blur(0px)",
                }}
                exit={{
                  opacity: 0,
                  y: -50,
                  filter: "blur(10px)",
                }}
                transition={{
                  duration: 0.2,
                  ease: "easeOut",
                }}
                
              >
                <div >
                  <ul >
                    {TAB_CONTENT[activeTab].files.map((file, index) => (
                      <li
                        key={file}
                        
                      >
                        {file}
                      </li>
                    ))}
                  </ul>
                </div>
              </motion.div>
            </AnimatePresence>
          </div>
        </div>

        {/* Interactive text overlay, no filter */}
        <div >
          {TAB_CONTENT.map((tab, index) => (
            <button
              key={index}
              onClick={() => setActiveTab(index)}
              
            >
              
                {tab.title}
              
            </button>
          ))}
        </div>
      </div>
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/gooey-svg-filter
```






### Component Source Code (gooey-svg-filter.tsx)
```tsx
const GooeySvgFilter = ({
  id = "gooey-filter",
  strength = 10,
}: {
  id?: string
  strength?: number
}) => {
  return (
    <svg >
      <defs>
        <filter id={id}>
          <feGaussianBlur
            in="SourceGraphic"
            stdDeviation={strength}
            result="blur-sm"
          />
          <feColorMatrix
            in="blur-sm"
            type="matrix"
            values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 19 -9"
            result="goo"
          />
          <feComposite in="SourceGraphic" in2="goo" operator="atop" />
        </filter>
      </defs>
    </svg>
  )
}

export default GooeySvgFilter
```





## Usage

Add the `GooeySvgFilter` component to your project, pass an `id` prop to the component (optional), then use the same `id` prop in the `filter` CSS property of the container you want to apply the filter to. High-level example:


**File**: `Goeey SVG Filter Example`
```tsx
<GooeySvgFilter id="gooey-filter" />
<div style={{ filter: "url(#gooey-filter)" }}>
 filter will be applied here
</div>
```


## Understanding the component

The filter is surprisingly simple. First, we apply a blur, which makes closer element to 'bleed' or blur into each other. Then, we just increase the contrast of our alpha channel with a color matrix. Lastly, we composite these two layers together with an `atop` operator, which will mask out anything outside the filter.  

Please refer to [this article](https://css-tricks.com/gooey-effect/) by [Lucas Bebber](https://lbebber.github.io/public/) for more details. The entire component is derived from this work, and Lucas does a much better job explaining the filter than I do:).

## Examples

### Fluid interface


### Demo Code (gooey-svg-filter-menu-demo.tsx)
```tsx
import { useState } from "react"
import { AnimatePresence, motion } from "motion/react"
import { Home, Mail, Menu, Settings, User, X } from "lucide-react"

import useDetectBrowser from "@/hooks/use-detect-browser"
import GooeySvgFilter from "@/fancy/components/filter/gooey-svg-filter"

const MENU_ITEMS = [
  { icon: Home, label: "Home" },
  { icon: Mail, label: "Contact" },
  { icon: User, label: "Profile" },
  { icon: Settings, label: "Settings" },
]

export default function GooeyDemo() {
  const [isOpen, setIsOpen] = useState(false)
  const browser = useDetectBrowser()
  const isSafari = browser === "Safari"

  return (
    <div >
      <GooeySvgFilter id="gooey-filter-menu" strength={5} />

      <div
        
        style={{ filter: "url(#gooey-filter-menu)" }}
      >
        {/* Menu Items */}
        <AnimatePresence>
          {isOpen &&
            MENU_ITEMS.map((item, index) => {
              const Icon = item.icon
              return (
                <motion.button
                  key={item.label}
                  
                  initial={{ x: 0, opacity: 0 }}
                  animate={{
                    y: (index + 1) * 44,
                    opacity: 1,
                  }}
                  exit={{
                    y: 0,
                    opacity: 0,
                    transition: {
                      delay:
                        (MENU_ITEMS.length - index) * (isSafari ? 0.0 : 0.05),
                      duration: isSafari ? 0 : 0.4,
                      type: "spring",
                      bounce: 0,
                    },
                  }}
                  transition={{
                    delay: index * (isSafari ? 0.0 : 0.05),
                    duration: isSafari ? 0 : 0.4,
                    type: "spring",
                    bounce: 0,
                  }}
                >
                  <AnimatePresence mode="wait">
                    <motion.div
                      key={item.label}
                      initial={{ opacity: 0, filter: "blur(10px)" }}
                      animate={{ opacity: 1, filter: "blur(0px)" }}
                      exit={{ opacity: 0, filter: "blur(10px)" }}
                      transition={{
                        delay: index * (isSafari ? 0.0 : 0.05),
                        duration: isSafari ? 0 : 0.2,
                        type: "spring",
                        bounce: 0,
                      }}
                    >
                      <Icon  />
                    </motion.div>
                  </AnimatePresence>
                </motion.button>
              )
            })}
        </AnimatePresence>

        {/* Main Menu Button */}
        <motion.button
          
          onClick={() => setIsOpen(!isOpen)}
        >
          <AnimatePresence mode="wait">
            {isOpen ? (
              <motion.div
                key="close"
                initial={{ opacity: 0, filter: "blur(10px)" }}
                animate={{ opacity: 1, filter: "blur(0px)" }}
                exit={{ opacity: 0, filter: "blur(10px)" }}
                transition={{ duration: isSafari ? 0 : 0.2 }}
              >
                <X  />
              </motion.div>
            ) : (
              <motion.div
                key="menu"
                initial={{ opacity: 0, filter: "blur(10px)" }}
                animate={{ opacity: 1, filter: "blur(0px)" }}
                exit={{ opacity: 0, filter: "blur(10px)" }}
                transition={{ duration: isSafari ? 0 : 0.2 }}
              >
                <Menu  />
              </motion.div>
            )}
          </AnimatePresence>
        </motion.button>
      </div>

      <p>Open the menu in the top left corner</p>
    </div>
  )
}
```


### Rounded corners

The following example combines the [PixelTrail](/docs/components/background/pixel-trail) component with this svg filter to create a rounded-at-all-corners effect. Unfortunately, the component doesn't support Safari, so you'll need to create a fallback for that.


### Demo Code (gooey-svg-filter-pixel-trail-demo.tsx)
```tsx
import useDetectBrowser from "@/hooks/use-detect-browser"
import useScreenSize from "@/hooks/use-screen-size"
import PixelTrail from "@/fancy/components/background/pixel-trail"
import GooeySvgFilter from "@/fancy/components/filter/gooey-svg-filter"

export default function GooeyDemo() {
  const screenSize = useScreenSize()
  const browserName = useDetectBrowser()
  const isSafari = browserName === "Safari"

  return (
    <div >
      <img
        src="https://images.aiscribbles.com/34fe5695dbc942628e3cad9744e8ae13.png?v=60d084"
        alt="impressionist painting"
        
      />

      <GooeySvgFilter id="gooey-filter-pixel-trail" strength={5} />

      <div
        
        style={{ filter: isSafari ? "none" : "url(#gooey-filter-pixel-trail)" }}
      >
        <PixelTrail
          pixelSize={screenSize.lessThan(`md`) ? 24 : 32}
          fadeDuration={0}
          delay={500}
          pixelClassName="bg-white"
        />
      </div>

      <p >
        Speaking things into existence
        
      </p>
    </div>
  )
}
```


## Notes

- Safari support for SVG filters is still very limited, so make sure to check compatibility, and create fallbacks (in the demos above, you can also see that the motion is disabled for Safari). For a fully supported solution, your best bet is to create a shader instead. Let us know if you would like to see a component for that!
- Keep a large enough space for the filter to avoid clipping.


## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| id | `string` | - | The id of the filter |
| strength | `number` | `15` | The strength of the gooey effect |


  ## Credits

  The component is derived from [this article](https://css-tricks.com/gooey-effect/) by [Lucas Bebber](https://lbebber.github.io/public/).


---

### Pixelate SVG Filter <a name="pixelate-svg-filter"></a>

### Demo Code (pixelate-svg-filter-demo.tsx)
```tsx
import { useRef } from "react"
import PixelateSvgFilter from "@/fancy/components/filter/pixelate-svg-filter"
import { useMousePosition } from "@/hooks/use-mouse-position"

export default function PixelateSVGFilterDemo() {
    const containerRef = useRef<HTMLDivElement>(null)
    const mousePosition = useMousePosition(containerRef)
    const pixelSize = Math.min(Math.max(mousePosition.x / 30, 1), 64)

  return (
    <div  ref={containerRef}>
      <PixelateSvgFilter id="pixelate-filter" size={pixelSize} crossLayers />
      <div 
        id="image-container"
        
        style={{ filter: "url(#pixelate-filter)" }}
      >
        <video
          src={"https://cdn.cosmos.so/96ae0b34-289d-489d-94a1-c68925ddd3a9.mp4"}
          
          autoPlay
          muted
          playsInline
          loop
          //style={{ filter: "url(#pixelate-filter)" }}

        />
      </div>
      
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/pixelate-svg-filter
```






### Component Source Code (pixelate-svg-filter.tsx)
```tsx
interface PixelateSvgFilterProps {
  id: string
  size?: number
  crossLayers?: boolean
}

export default function PixelateSvgFilter({
  id = "pixelate-filter",
  size = 16,
  crossLayers = false,
}: PixelateSvgFilterProps) {
  return (
    <svg >
      <defs>
        <filter id={id} x="0" y="0" width="1" height="1">
          {"First layer: Normal pixelation effect"}
          <feConvolveMatrix
            kernelMatrix="1 1 1
                          1 1 1
                          1 1 1"
            result="AVG"
          />
          <feFlood x="1" y="1" width="1" height="1" />
          <feComposite
            operator="arithmetic"
            k1="0"
            k2="1"
            k3="0"
            k4="0"
            width={size}
            height={size}
          />
          <feTile result="TILE" />
          <feComposite
            in="AVG"
            in2="TILE"
            operator="in"
            k1="0"
            k2="1"
            k3="0"
            k4="0"
          />
          <feMorphology operator="dilate" radius={size / 2} result={"NORMAL"} />
          {crossLayers && (
            <>
              {"Second layer: Fallback with full-width tiling"}
              <feConvolveMatrix
                kernelMatrix="1 1 1
                              1 1 1
                              1 1 1"
                result="AVG"
              />
              <feFlood x="1" y="1" width="1" height="1" />
              <feComposite
                in2="SourceGraphic"
                operator="arithmetic"
                k1="0"
                k2="1"
                k3="0"
                k4="0"
                width={size / 2}
                height={size}
              />
              <feTile result="TILE" />
              <feComposite
                in="AVG"
                in2="TILE"
                operator="in"
                k1="0"
                k2="1"
                k3="0"
                k4="0"
              />
              <feMorphology
                operator="dilate"
                radius={size / 2}
                result={"FALLBACKX"}
              />
              {"Third layer: Fallback with full-height tiling"}
              <feConvolveMatrix
                kernelMatrix="1 1 1
                              1 1 1
                              1 1 1"
                result="AVG"
              />
              <feFlood x="1" y="1" width="1" height="1" />
              <feComposite
                in2="SourceGraphic"
                operator="arithmetic"
                k1="0"
                k2="1"
                k3="0"
                k4="0"
                width={size}
                height={size / 2}
              />
              <feTile result="TILE" />
              <feComposite
                in="AVG"
                in2="TILE"
                operator="in"
                k1="0"
                k2="1"
                k3="0"
                k4="0"
              />
              <feMorphology
                operator="dilate"
                radius={size / 2}
                result={"FALLBACKY"}
              />
              <feMerge>
                <feMergeNode in="FALLBACKX" />
                <feMergeNode in="FALLBACKY" />
                <feMergeNode in="NORMAL" />
              </feMerge>
            </>
          )}
          {!crossLayers && <feMergeNode in="NORMAL" />}
        </filter>
      </defs>
    </svg>
  )
}
```





## Usage

Add the `PixelateSvgFilter` component to your project, pass an `id` prop to the component (optional), then use the same `id` prop in the `filter` CSS property of the container you want to apply the filter to. High-level example:


**File**: `Pixelate SVG Filter Example`
```tsx
<PixelateSvgFilter id="pixelate-filter" />
<div style={{ filter: "url(#pixelate-filter)" }}>
 filter will be applied here
</div>
```


## Understanding the component

The pixelation effect is achieved using SVG filters. The process works in three steps:

1. The filter divides the input into a grid using `feFlood` and `feComposite` operations, where each cell represents a future "pixel"
2. The `feTile` operation repeats this grid pattern across the entire target area
3. Finally, `feColorMatrix` and `feComposite` are used to blend the original image with the grid, creating the pixelated effect

The component accepts two optional props to customize the pixelation effect:

- `size` (default: 16): Controls the size of each "pixel" in the resulting effect. A larger value creates a more blocky appearance, while a smaller value produces finer pixelation.

- `crossLayers` (default: false): When enabled, adds two additional filter layers that help prevent visual artifacts:
  - A second layer that ensures full-width coverage by using half-width pixels
  - A third layer that ensures full-height coverage by using half-height pixels
  
  This is particularly useful when applying dynamic filters where the target area's dimensions may not perfectly align with the pixel grid, preventing unwanted "jumpiness" in the effect.

Please have a look at the following [thread](https://stackoverflow.com/questions/37451189/can-one-pixelate-images-with-an-svg-filter) for more details. Props to the folks who shared their insights and code!

## Examples

### Text

The filter can be applied to text as well. Hit the refresh button to see the effect.


### Demo Code (pixelate-svg-filter-text.tsx)
```tsx
import { useEffect, useRef, useState } from "react"
import { animate, useMotionValue, useMotionValueEvent } from "motion/react"

import PixelateSvgFilter from "@/fancy/components/filter/pixelate-svg-filter"

export default function PixelateSVGFilterDemo() {
  const containerRef = useRef<HTMLDivElement>(null)
  const pixelSize = useMotionValue(16)
  const [size, setSize] = useState(16)
  const [isAnimating, setIsAnimating] = useState(true)

  useMotionValueEvent(pixelSize, "change", (latest) => {
    setSize(latest)
  })

  useEffect(() => {
    const controls = animate(pixelSize, 1, {
      duration: 1.2,
      ease: "easeOut",
      onComplete: () => setIsAnimating(false),
    })

    return controls.stop
  }, [])

  return (
    <div
      
      ref={containerRef}
    >
      {isAnimating && (
        <PixelateSvgFilter id="pixelate-text-filter" size={size} />
      )}

      {/* Left Content */}
      <div
        
        style={{
          filter: isAnimating ? "url(#pixelate-text-filter)" : undefined,
        }}
      >
        <div >
          <h1 >Ari — Yu</h1>
          <a
            href="mailto:hello@arianexus.io"
            
          >
            hello@ariyu.co
          </a>
        </div>

        <div >
          <h2 >Creative Director</h2>
          <h2 >& Writer</h2>
        </div>

        <div >
          <h3 >
            Selected Works
          </h3>
          <p >
            Dreamweaver #93 Dec 2023 Starlight #87 "digital dreams" The Quantum
            Mirror Press Holograph Vol.5 crystal edition 2020 Byte Flow Vol.12
            Neural Canvas #9 Synthmagazin 11/2020 VOID 2020 zine Nebula #4 VOID
            量子 + Wave zines Binary Pulse volume 7 Cyber Cascade
            (self-published)
          </p>
        </div>
      </div>

      {/* Right Content - Image */}
      <div >
        <img
          
          src={
            "https://images.unsplash.com/photo-1729009704569-474ddd86ed3a?q=80&w=2043&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
          }
          style={{
            filter: isAnimating ? "url(#pixelate-text-filter)" : undefined,
          }}
        />
      </div>
    </div>
  )
}
```


## Notes

Safari is unfortunately not supported. If you have any suggestions or ideas for how to make this component work with it, please let us know!

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| id | `string` | `"pixelate-filter"` | The ID of the filter |
| size | `number` | `16` | The size of each pixel in the resulting effect |
| crossLayers | `boolean` | `false` | Whether to add two additional filter layers |


## Credits

The effect is derived from multiple people's work from this [thread](https://stackoverflow.com/questions/37451189/can-one-pixelate-images-with-an-svg-filter).


---

## Blocks <a name="blocks"></a>

### Drag Elements <a name="drag-elements"></a>

### Demo Code (drag-elements-demo.tsx)
```tsx
import React from "react"
import Image from "next/image"

import useScreenSize from "@/hooks/use-screen-size"
import DragElements from "@/fancy/components/blocks/drag-elements"

const urls = [
  "https://images.unsplash.com/photo-1683746531526-3bca2bc901b8?q=80&w=1820&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
  "https://images.unsplash.com/photo-1631561729243-9b3291efceae?q=80&w=1885&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
  "https://images.unsplash.com/photo-1635434002329-8ab192fe01e1?q=80&w=2828&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
  "https://images.unsplash.com/photo-1719586799413-3f42bb2a132d?q=80&w=2048&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
  "https://images.unsplash.com/photo-1720561467986-ca3d408ca30b?q=80&w=2048&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
  "https://images.unsplash.com/photo-1724403124996-64115f38cd3f?q=80&w=3082&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
]

const randomInt = (min: number, max: number) => {
  return Math.floor(Math.random() * (max - min + 1)) + min
}

const DragElementsDemo: React.FC = () => {
  const screenSize = useScreenSize()
  return (
    <div >
      <h1 >
        all your
        
          {" "}
          memories.{" "}
        
      </h1>
      <DragElements dragMomentum={false} >
        {urls.map((url, index) => {
          const rotation = randomInt(-12, 12)
          const width = screenSize.lessThan(`md`)
            ? randomInt(90, 120)
            : randomInt(120, 150)
          const height = screenSize.lessThan(`md`)
            ? randomInt(120, 140)
            : randomInt(150, 180)

          return (
            <div
              key={index}
              className={`flex items-start justify-center bg-white shadow-2xl p-4`}
              style={{
                transform: `rotate(${rotation}deg)`,
                width: `${width}px`,
                height: `${height}px`,
              }}
            >
              <div
                className={`relative overflow-hidden`}
                style={{
                  width: `${width - 4}px`,
                  height: `${height - 30}px`,
                }}
              >
                <Image
                  src={url}
                  fill
                  alt={`Analog photo ${index + 1}`}
                  
                  draggable={false}
                />
              </div>
            </div>
          )
        })}
      </DragElements>
    </div>
  )
}

export default DragElementsDemo
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/drag-elements
```






### Component Source Code (drag-elements.tsx)
```tsx
"use client"

import React, { useEffect, useRef, useState } from "react"
import { InertiaOptions, motion } from "motion/react"

type DragElementsProps = {
  children: React.ReactNode
  dragElastic?:
    | number
    | { top?: number; left?: number; right?: number; bottom?: number }
    | boolean
  dragConstraints?:
    | { top?: number; left?: number; right?: number; bottom?: number }
    | React.RefObject<Element | null>
  dragMomentum?: boolean
  dragTransition?: InertiaOptions
  dragPropagation?: boolean
  selectedOnTop?: boolean
  className?: string
}

const DragElements: React.FC<DragElementsProps> = ({
  children,
  dragElastic = 0.5,
  dragConstraints,
  dragMomentum = true,
  dragTransition = { bounceStiffness: 200, bounceDamping: 300 },
  dragPropagation = true,
  selectedOnTop = true,
  className,
}) => {
  const constraintsRef = useRef<HTMLDivElement>(null)
  const [zIndices, setZIndices] = useState<number[]>([])

  const [isDragging, setIsDragging] = useState(false)

  useEffect(() => {
    setZIndices(
      Array.from({ length: React.Children.count(children) }, (_, i) => i)
    )
  }, [children])

  const bringToFront = (index: number) => {
    if (selectedOnTop) {
      setZIndices((prevIndices) => {
        const newIndices = [...prevIndices]
        const currentIndex = newIndices.indexOf(index)
        newIndices.splice(currentIndex, 1)
        newIndices.push(index)
        return newIndices
      })
    }
  }

  return (
    <div ref={constraintsRef} className={`relative w-full h-full ${className}`}>
      {React.Children.map(children, (child, index) => (
        <motion.div
          key={index}
          drag
          dragElastic={dragElastic}
          dragConstraints={dragConstraints || constraintsRef}
          dragMomentum={dragMomentum}
          dragTransition={dragTransition}
          dragPropagation={dragPropagation}
          style={{
            zIndex: zIndices.indexOf(index),

            cursor: isDragging ? "grabbing" : "grab",
          }}
          onDragStart={() => {
            bringToFront(index)
            setIsDragging(true)
          }}
          onDragEnd={() => setIsDragging(false)}
          whileDrag={{ cursor: "grabbing" }}
          className={"absolute"}
        >
          {child}
        </motion.div>
      ))}
    </div>
  )
}

export default DragElements
```





## Usage

You only need to wrap your elements with the `DragElements` component, everything else is taken care of by the component itself.

Under the hood, the component wraps all the children in a `relative` container, then sets all children to `absolute` to allow free dragging. For the dragging events and logic it uses motion`s Drag gestures.

The children are constrained to move withing the container, but you can set the `dragConstraints` prop to define a custom container, or custom `top`, `bottom`, `left` and `right` value. For the other drag props, refer to motion's [Drag gestures](https://motion.dev/docs/react-gestures#drag) documentation.

In the demo above, the `dragMomentum` prop is set to `false` to disable the "physics-based" movement, but in the following example, you can see a more funky use-case where it is enabled.

## Examples

### Drag momentum


### Demo Code (drag-elements-momentum-demo.tsx)
```tsx
import React from "react"

import DragElements from "@/fancy/components/blocks/drag-elements"

const DragElementsDemo: React.FC = () => {
  return (
    <div >
      <DragElements dragMomentum={true} >
        <div >
          super fun ✿
        </div>
        <div >
          funky time! ✴
        </div>
        <div >
          awesome ✺
        </div>
      </DragElements>
    </div>
  )
}

export default DragElementsDemo
```


## Notes

If you use images, or videos, make sure to set the `draggable` attribute to `false`, as they have priority for drag events.

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The elements to be dragged |
| dragElastic | `number | { top?: number; left?: number; right?: number; bottom?: number } | boolean` | `0.5` | Determines how much the element can be dragged outside the constraints |
| dragConstraints | `{ top?: number; left?: number; right?: number; bottom?: number } | React.RefObject` | - | An object with top, left, right, bottom properties, or a ref to another element, to constrain the drag area |
| dragMomentum | `boolean` | `true` | If true, the element will continue moving when the drag gesture ends |
| dragTransition | `InertiaOptions` | `{ bounceStiffness: 200, bounceDamping: 300 }` | Specifies the spring physics for the drag end animation |
| dragPropagation | `boolean` | `true` | If true, allows dragging events to propagate to parent drag gestures |
| selectedOnTop | `boolean` | `true` | If true, brings the dragged element to the front |
| className | `string` | - | Additional CSS classes for the container |


---

### Circling Elements <a name="circling-elements"></a>

### Demo Code (circling-elements-demo.tsx)
```tsx
import React from "react"
import Image from "next/image"
import { exampleImages } from "@/utils/demo-images"

import useScreenSize from "@/hooks/use-screen-size"
import CirclingElements from "@/fancy/components/blocks/circling-elements"

const CirclingElementsDemo: React.FC = () => {
  const screenSize = useScreenSize()

  return (
    <div >
      <CirclingElements
        radius={screenSize.lessThan(`md`) ? 80 : 120}
        duration={10}
        easing="linear"
        pauseOnHover={true}
      >
        {exampleImages.map((image, index) => (
          <div
            key={index}
            
          >
            <Image src={image.url} fill alt="image"  />
          </div>
        ))}
      </CirclingElements>
    </div>
  )
}

export default CirclingElementsDemo
```


Inspiration from [Bakken & Bæck](https://www.instagram.com/p/DBG5fLdiN4Q/?img_index=1)

## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/circling-elements
```





Copy and paste the following code into your project:


### Component Source Code (circling-elements.tsx)
```tsx
"use client"

import { Children } from "react"
import { motion } from "motion/react"

import { cn } from "@/lib/utils"

type CirclingElementsProps = {
  children: React.ReactNode
  radius?: number
  duration?: number // in seconds
  easing?: string
  direction?: "normal" | "reverse"
  className?: string
  pauseOnHover?: boolean
}

const CirclingElements: React.FC<CirclingElementsProps> = ({
  children,
  radius = 100,
  duration = 10,
  easing = "linear",
  direction = "normal",
  className,
  pauseOnHover = false,
}) => {
  return (
    <div className={cn("relative z-0 group/circling", className)}>
      {Children.map(children, (child, index) => {
        const offset = (index * 360) / Children.count(children)

        const animationProps = {
          "--circling-duration": duration,
          "--circling-radius": radius,
          "--circling-offset": offset,
          "--circling-direction": direction === "reverse" ? -1 : 1,
          animation: `circling ${duration}s ${easing} infinite`,
          animationName: "circling",
          animationDuration: `${duration}s`,
          animationTimingFunction: easing,
          animationIterationCount: "infinite",
        } as React.CSSProperties

        return (
          <motion.div
            key={index}
            style={animationProps}
            className={cn(
              "transform-gpu animate-circling absolute -translate-x-1/2 -translate-y-1/2",
              pauseOnHover &&
                "group-hover/circling:![animation-play-state:paused]"
            )}
          >
            {child}
          </motion.div>
        )
      })}
    </div>
  )
}

export default CirclingElements
```


Add the following animation and keyframes to your `globals.css` file:


**File**: `globals.css`
```css
{
/* ... */
  @theme: {
    --animate-circling: circling;
    @keyframes circling {
      0% {
        transform: rotate(calc(var(--offset) * 1deg)) translate(calc(var(--radius) * 1px), 0) rotate(calc(var(--offset) * -1deg));
      }
      100% {
        transform: rotate(calc(360deg + (var(--offset) * 1deg))) translate(calc(var(--radius) * 1px), 0) rotate(calc(-360deg + (var(--offset) * -1deg)));
      }
    },
  },
};
```





## Usage

You only need to wrap your elements with the `CirclingElements` component, everything else is taken care of by the component itself.

## Understanding the component

Under the hood, the component wraps all the children in a `relative` container, then sets all children to `absolute` to allow the circling movement.

The animation is achieved through CSS keyframes that create a circular motion. At the start (0%), each element is rotated by its offset angle, translated outward by the radius, and counter-rotated to maintain orientation. At the end (100%), it completes a full 360-degree rotation while maintaining the same radius and orientation. The `--circling-direction` variable allows reversing the animation direction.

The keyframes use CSS calc() to dynamically compute the transforms based on the following variables:

- `--circling-offset` (element's starting angle)
- `--circling-radius` (circle size)
- `--circling-direction` (1 or -1 for direction)

## Examples

### Easing & direction

You can set custom easings and reverse direction for the animation, as you can see in this demo.


### Demo Code (circling-elements-easing-demo.tsx)
```tsx
import React from "react"
import Image from "next/image"
import { exampleImages } from "@/utils/demo-images"

import useScreenSize from "@/hooks/use-screen-size"
import CirclingElements from "@/fancy/components/blocks/circling-elements"

const CirclingElementsDemo: React.FC = () => {
  const screenSize = useScreenSize()

  return (
    <div >
      <CirclingElements
        radius={screenSize.lessThan(`md`) ? 100 : 180}
        duration={8}
        direction="reverse"
        easing="0.944, 0.008, 0.147, 1.002"
      >
        {[...exampleImages, ...exampleImages].map((image, index) => (
          <div
            key={index}
            
          >
            <Image
              src={image.url}
              fill
              alt="image"
              
            />
          </div>
        ))}
      </CirclingElements>
    </div>
  )
}

export default CirclingElementsDemo
```


## Credits

Ported to Framer by [Achille Ernoult](https://x.com/achilleernlt)

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `ReactNode` | - | The elements to be circled |
| radius | `number` | `100` | The radius of the circle in pixels |
| duration | `number` | `10` | The duration of one complete rotation in seconds |
| easing | `string` | `linear` | The easing function for the animation. Refer to the official [mdn         docs](https://developer.mozilla.org/en-US/docs/Web/CSS/easing-function)         for more details |
| direction | `"normal" | "reverse"` | `normal` | The direction of the animation. Set to `"reverse"` to reverse the         animation |
| pauseOnHover | `boolean` | `false` | Pause the animation on hover |
| className | `string` | - | Additional CSS classes for the container |


## Credits

Inspiration for the demo from [Bakken & Bæck](https://www.instagram.com/p/DBG5fLdiN4Q/?img_index=1)


---

### Media Between Text <a name="media-between-text"></a>

### Demo Code (media-between-text-demo.tsx)
```tsx
import useScreenSize from "@/hooks/use-screen-size"
import MediaBetweenText from "@/fancy/components/blocks/media-between-text"

export default function Preview() {
  const screenSize = useScreenSize()

  return (
    <div >
      <a
        href="https://www.instagram.com/p/C3oL4euoc2l/?img_index=1"
        target="_blank"
        rel="noreferrer"
      >
        <MediaBetweenText
          firstText="that's a nice ("
          secondText=") chair!"
          mediaUrl={
            "https://cdn.cosmos.so/90e2192e-7bd4-44af-96ae-05cd955c0cfb?format=jpeg"
          }
          mediaType="image"
          triggerType="hover"
          mediaContainerClassName="w-full h-[30px] sm:h-[100px] overflow-hidden mx-px mt-1 sm:mx-2 sm:mt-4"
          
          animationVariants={{
            initial: { width: 0 },
            animate: {
              width: screenSize.lessThan("sm") ? "30px" : "100px",
              transition: { duration: 0.4, type: "spring", bounce: 0 },
            },
          }}
        />
      </a>
    </div>
  )
}
```


Artwork by [Joffey](https://www.instagram.com/designbyjoffey/)

## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/media-between-text
```






### Component Source Code (media-between-text.tsx)
```tsx
"use client"

import { ElementType, forwardRef, useImperativeHandle, useRef, useState } from "react"
import { motion, useInView, UseInViewOptions, Variants } from "motion/react"

import { cn } from "@/lib/utils"

interface MediaBetweenTextProps {
  /**
   * The text to display before the media
   */
  firstText: string

  /**
   * The text to display after the media
   */
  secondText: string

  /**
   * URL of the media (image or video) to display
   */
  mediaUrl: string

  /**
   * Type of media to display
   */
  mediaType: "image" | "video"

  /**
   * Optional class name for the media container
   */
  mediaContainerClassName?: string

  /**
   * Fallback URL for video poster or image loading
   */
  fallbackUrl?: string

  /**
   * HTML Tag to render the text elements as
   * @default p
   */
  as?: ElementType

  /**
   * Whether video should autoplay
   * @default true
   */
  autoPlay?: boolean

  /**
   * Whether video should loop
   * @default true
   */
  loop?: boolean

  /**
   * Whether video should be muted
   * @default true
   */
  muted?: boolean

  /**
   * Whether video should play inline
   * @default true
   */
  playsInline?: boolean

  /**
   * Alt text for image
   */
  alt?: string

  /**
   * Type of animation trigger
   * @default "hover"
   */
  triggerType?: "hover" | "ref" | "inView"

  /**
   * Reference to container element for inView trigger
   */
  containerRef?: React.RefObject<HTMLDivElement | null>

  /**
   * Options for useInView hook
   */
  useInViewOptionsProp?: UseInViewOptions

  /**
   * Custom animation variants
   */
  animationVariants?: {
    initial: Variants["initial"]
    animate: Variants["animate"]
  }

  /**
   * Optional class name for the root element
   */
  className?: string

  /**
   * Optional class name for the left text element
   */
  leftTextClassName?: string

  /**
   * Optional class name for the right text element
   */
  rightTextClassName?: string
}

export type MediaBetweenTextRef = {
  animate: () => void
  reset: () => void
}

export const MediaBetweenText = forwardRef<
  MediaBetweenTextRef,
  MediaBetweenTextProps
>(
  (
    {
      firstText,
      secondText,
      mediaUrl,
      mediaType,
      mediaContainerClassName,
      fallbackUrl,
      as = "p",
      autoPlay = true,
      loop = true,
      muted = true,
      playsInline = true,
      alt,
      triggerType = "hover",
      containerRef,
      useInViewOptionsProp = {
        once: true,
        amount: 0.5,
        root: containerRef,
      },
      animationVariants = {
        initial: { width: 0, opacity: 1 },
        animate: {
          width: "auto",
          opacity: 1,
          transition: { duration: 0.4, type: "spring", bounce: 0 },
        },
      },
      className,
      leftTextClassName,
      rightTextClassName,
    },
    ref
  ) => {
    const componentRef = useRef<HTMLDivElement>(null)
    const [isAnimating, setIsAnimating] = useState(false)

    const isInView =
      triggerType === "inView"
        ? useInView(componentRef || containerRef, useInViewOptionsProp)
        : false
    const [isHovered, setIsHovered] = useState(false)

    useImperativeHandle(ref, () => ({
      animate: () => setIsAnimating(true),
      reset: () => setIsAnimating(false),
    }))

    const shouldAnimate =
      triggerType === "hover"
        ? isHovered
        : triggerType === "inView"
          ? isInView
          : triggerType === "ref"
            ? isAnimating
            : false

    const TextComponent = motion.create(as)

    return (
      <div
        className={cn("flex", className)}
        ref={componentRef}
        onMouseEnter={() => triggerType === "hover" && setIsHovered(true)}
        onMouseLeave={() => triggerType === "hover" && setIsHovered(false)}
      >
        <TextComponent layout className={leftTextClassName}>
          {firstText}
        </TextComponent>
        <motion.div
          className={mediaContainerClassName}
          variants={animationVariants}
          initial="initial"
          animate={shouldAnimate ? "animate" : "initial"}
        >
          {mediaType === "video" ? (
            <video
              
              autoPlay={autoPlay}
              loop={loop}
              muted={muted}
              playsInline={playsInline}
              poster={fallbackUrl}
            >
              <source src={mediaUrl} type="video/mp4" />
            </video>
          ) : (
            <img
              src={mediaUrl}
              alt={alt || `${firstText} ${secondText}`}
              
            />
          )}
        </motion.div>
        <TextComponent layout className={rightTextClassName}>
          {secondText}
        </TextComponent>
      </div>
    )
  }
)

MediaBetweenText.displayName = "MediaBetweenText"

export default MediaBetweenText
```





## Usage

The component is extremely simple, and only consists of two text elements and a media element (either an image or a video). The trick for the smooth animation is to use `layout` animations on the two texts, so they smoothly transition when the media element is revealed.
You can trigger the media reveal animation by `hover`, `ref`, or `inView`:

- `hover`: The media will animate when you hover over the component
- `ref`: You can call the `animate` and `reset` methods exposed via a ref to manually control the animation
- `inView`: The media will animate when the component is in view. You can pass an `useInViewOptionsProp` prop to customize the in view detection. Refer to the [motion documentation](https://motion.dev/docs/docs/react-use-in-view) for more details.

You can also customize the animation by passing a `animationVariants` prop. Please use `animate` and `initial` variants. Refer to the [motion documentation](https://motion.dev/docs/react-animation#animatable-values) for more details.

## Examples

### Scroll

Scroll down to trigger the animation. In this case, you can pass down a containerRef prop to the component to track when elements come into view within that specific container, rather than the entire viewport. This is useful when you want to trigger animations based on scrolling within a specific scrollable container.


### Demo Code (media-between-text-scroll-demo.tsx)
```tsx
import React from "react"

import useScreenSize from "@/hooks/use-screen-size"
import MediaBetweenText from "@/fancy/components/blocks/media-between-text"

const elements = [
  {
    src: "https://cdn.cosmos.so/53454cbe-a4ec-4782-923f-a82d70e12645.mp4",
    left: "Tim",
    right: "Rodenböker",
    url: "https://www.instagram.com/tim_rodenbroeker/",
  },
  {
    src: "https://cdn.cosmos.so/499ddb3b-57cf-4c07-996c-f797fadf64ab.mp4",
    left: "Simon ",
    right: "Alexander-Adams",
    url: "https://www.instagram.com/polyhop/",
  },
  {
    src: "https://cdn.cosmos.so/444e4a2a-45a6-477f-b342-6b6bc9a7c53b.mp4",
    left: "Andreion",
    right: "de Castro",
    url: "https://www.instagram.com/andreiongd/",
  },
  {
    src: "https://cdn.cosmos.so/f533f1a8-9f67-4360-b395-7abc8594cac9.mp4",
    left: "Lorraine",
    right: "Li",
    url: "https://www.instagram.com/lorrr.l/",
  },
]

export default function MediaBetweenTextScrollDemo() {
  const ref = React.useRef<HTMLDivElement>(null)
  const screenSize = useScreenSize()

  return (
    <div
      
      ref={ref}
    >
      <div >
        <h3 >
          today's inspo
        </h3>
        <p >
          Scroll down ↓
        </p>
      </div>

      <div >
        {elements.map((element, index) => (
          <a href={element.url} target="_blank" rel="noreferrer">
            <MediaBetweenText
              key={index}
              firstText={element.left}
              secondText={element.right}
              mediaUrl={element.src}
              mediaType="video"
              triggerType="inView"
              useInViewOptionsProp={{
                once: false,
                amount: 1,
                root: ref,
                margin: "-5% 0px -0% 0px",
              }}
              containerRef={ref}
              mediaContainerClassName="w-full h-[40px] sm:h-[80px] overflow-hidden mx-1 sm:mx-3 mt-1 sm:mt-4"
              
              animationVariants={{
                initial: { width: 0 },
                animate: {
                  width: screenSize.lessThan("sm") ? "40px" : "100px",
                  transition: {
                    duration: 1,
                    type: "spring",
                    bounce: 0,
                    delay: 0.1,
                  },
                },
              }}
            />
          </a>
        ))}
      </div>
    </div>
  )
}
```


Artworks by [Tim Rodenböker](https://www.instagram.com/tim_rodenbroeker/), [polyhop](https://www.instagram.com/polyhop/), [Andreion de Castro](https://www.instagram.com/andreiongd/), [Lorraine Li](https://www.instagram.com/lorrr.l/)

### Vertical & Ref

You can also style the whole container, the media element, and the text elements separately. In this example, we use a column-layout to create a vertical effect. The animation can also be triggered from outside the component, by calling the `animate` and `reset` methods exposed via a ref. Click on the "Open" button to trigger the animation.


### Demo Code (media-between-text-vertical-demo.tsx)
```tsx
import { useRef, useState } from "react"

import useScreenSize from "@/hooks/use-screen-size"
import { Button } from "@/components/ui/button"
import MediaBetweenText, {
  MediaBetweenTextRef,
} from "@/fancy/components/blocks/media-between-text"

export default function Preview() {
  const ref = useRef<MediaBetweenTextRef>(null)
  const [isOpen, setIsOpen] = useState(false)
  const screenSize = useScreenSize()

  return (
    <div >
      <Button
        onClick={() => {
          setIsOpen(!isOpen)
          if (!isOpen) {
            ref.current?.animate()
          } else {
            ref.current?.reset()
          }
        }}
        size={"sm"}
        variant={"outline"}
        
      >
        {isOpen ? "Close" : "Open"}
      </Button>

      <MediaBetweenText
        firstText="Artificial "
        secondText="Intelligence"
        mediaUrl={
          "https://cdn.cosmos.so/47c0223f-c704-4d5a-8b47-c48262ebe301?format=jpeg"
        }
        mediaType="image"
        triggerType="ref"
        ref={ref}
        mediaContainerClassName="w-full h-[60px] sm:h-[100px] overflow-hidden pt-1"
        
        leftTextClassName=""
        rightTextClassName="italic"
        animationVariants={{
          initial: {
            width: screenSize.lessThan("sm") ? "160px" : "280px",
            height: 0,
            transition: { duration: 0.7, ease: [0.944, 0.008, 0.147, 1.002] },
          },
          animate: {
            width: screenSize.lessThan("sm") ? "200px" : "330px",
            height: screenSize.lessThan("sm") ? "200px" : "300px",
            transition: { duration: 0.7, ease: [0.944, 0.008, 0.147, 1.002] },
          },
        }}
      />
    </div>
  )
}
```


Video from [chrbutler.com](https://www.chrbutler.com/what-i-want-from-the-internet)

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| firstText* | `string` | - | The text to display before the media |
| secondText* | `string` | - | The text to display after the media |
| mediaUrl* | `string` | - | URL of the media (image or video) to display |
| mediaType* | `"image" | "video"` | - | Type of media to display |
| mediaContainerClassName | `string` | - | Optional class name for the media container |
| fallbackUrl | `string` | - | Fallback URL for video poster or image loading |
| as | `ElementType` | `"p"` | HTML Tag to render the text elements as |
| autoPlay | `boolean` | `true` | Whether video should autoplay |
| loop | `boolean` | `true` | Whether video should loop |
| muted | `boolean` | `true` | Whether video should be muted |
| playsInline | `boolean` | `true` | Whether video should play inline |
| alt | `string` | - | Alt text for image |
| triggerType | `"hover" | "ref" | "inView"` | `"hover"` | Type of animation trigger |
| containerRef | `React.RefObject` | - | Reference to container element for inView trigger |
| useInViewOptionsProp | `UseInViewOptions` | `{ once: true, amount: 0.5, root: containerRef }` | Options for useInView hook |
| animationVariants | `{ initial: Variants["initial"]; animate: Variants["animate"] }` | `{ initial: { width: 0, opacity: 1 }, animate: { width: "auto", opacity: 1, transition: { duration: 0.4, type: "spring", bounce: 0 } } }` | Custom animation variants |
| className | `string` | - | Optional class name for the root element |
| leftTextClassName | `string` | - | Optional class name for the left text element |
| rightTextClassName | `string` | - | Optional class name for the right text element |


---

### CSS Box <a name="css-box"></a>

### Demo Code (css-box-demo.tsx)
```tsx
import { useRef } from "react"

import CSSBox, { CSSBoxRef } from "@/fancy/components/blocks/css-box"

export default function CSSBoxDemo() {
  const boxRef = useRef<CSSBoxRef>(null)

  // Example text face component
  const TextFace = ({
    texts,
    className,
  }: {
    texts: string[]
    className?: string
  }) => (
    <div className={`flex flex-col ${className || ""}`}>
      {texts.map((text, i) => (
        <div key={i} >
          {text}
        </div>
      ))}
    </div>
  )

  return (
    <CSSBox
      ref={boxRef}
      width={200}
      height={200}
      depth={200}
      perspective={600}
      stiffness={100}
      damping={30}
      faces={{
        front: (
          <TextFace
            texts={["YOU CAN", "JUST", "DO THINGS"]}
            
          />
        ),
        back: (
          <TextFace
            texts={["MAKE THINGS", "YOU WISH", "EXISTED"]} 
            
          />
        ),
        right: (
          <TextFace
            texts={["MAKE THINGS", "YOU WISH", "EXISTED"]}
            
          />
        ),
        left: (
          <TextFace
            texts={["BREAK", "THINGS", "MOVE", "FAST"]}
            
          />
        ),
        top: (
          <TextFace
            texts={["YOU CAN", "JUST", "DO THINGS"]}
             
          />
        ),
        bottom: (
          <TextFace
            texts={["BREAK", "THINGS", "MOVE", "FAST"]}
            
          />
        ),
      }}
      
    />
  )
}
```


Artwork inspiration from [Ignite Amsterdam](https://www.instagram.com/p/CaDHtZKrk0F/)

## Credits

The component is derived from the Box chapter of David De Sandro's [extremely awesome Intro to CSS 3D transforms tutorial](https://3dtransforms.desandro.com/box). 

Ported to Framer by [Framer University](https://framer.university/)

## Installation




  CLI
  Manual





```bash
# Install command:
npx shadcn add @fancy/3d-css-box
```







### Component Source Code (css-box.tsx)
```tsx
"use client"

import {
  forwardRef,
  ReactNode,
  useCallback,
  useEffect,
  useImperativeHandle,
  useRef,
} from "react"
import { motion, useMotionValue, useSpring, useTransform } from "motion/react"

import { cn } from "@/lib/utils"

interface FaceProps {
  transform: string
  className?: string
  showBackface?: boolean
  children?: ReactNode
  style?: React.CSSProperties
}

const CubeFace = ({
  transform,
  className,
  showBackface,
  children,
  style,
}: FaceProps) => (
  <div
    className={cn(
      "absolute",
      showBackface ? "backface-visible" : "backface-hidden",
      className
    )}
    style={{ transform, ...style }}
  >
    {children}
  </div>
)

interface CubeFaces {
  front?: ReactNode
  back?: ReactNode
  right?: ReactNode
  left?: ReactNode
  top?: ReactNode
  bottom?: ReactNode
}

export interface CSSBoxRef {
  showFront: () => void
  showBack: () => void
  showLeft: () => void
  showRight: () => void
  showTop: () => void
  showBottom: () => void
  rotateTo: (x: number, y: number) => void
  getCurrentRotation: () => { x: number; y: number }
}

interface CSSBoxProps extends React.HTMLProps<HTMLDivElement> {
  width: number
  height: number
  depth: number
  className?: string
  perspective?: number
  stiffness?: number
  damping?: number
  showBackface?: boolean
  faces?: CubeFaces
  draggable?: boolean
}

const CSSBox = forwardRef<CSSBoxRef, CSSBoxProps>(
  (
    {
      width,
      height,
      depth,
      className,
      perspective = 600,
      stiffness = 100,
      damping = 30,
      showBackface = false,
      faces = {},
      draggable = true,
      ...props
    },
    ref
  ) => {
    const isDragging = useRef(false)
    const startPosition = useRef({ x: 0, y: 0 })
    const startRotation = useRef({ x: 0, y: 0 })

    const baseRotateX = useMotionValue(0)
    const baseRotateY = useMotionValue(0)

    const springRotateX = useSpring(baseRotateX, {
      stiffness,
      damping,
      ...(isDragging.current ? { stiffness: stiffness / 2 } : {}),
    })
    const springRotateY = useSpring(baseRotateY, {
      stiffness,
      damping,
      ...(isDragging.current ? { stiffness: stiffness / 2 } : {}),
    })

    const currentRotation = useRef({ x: 0, y: 0 })

    useImperativeHandle(
      ref,
      () => ({
        showFront: () => {
          baseRotateX.set(0)
          baseRotateY.set(0)
        },
        showBack: () => {
          baseRotateX.set(0)
          baseRotateY.set(180)
        },
        showLeft: () => {
          baseRotateX.set(0)
          baseRotateY.set(-90)
        },
        showRight: () => {
          baseRotateX.set(0)
          baseRotateY.set(90)
        },
        showTop: () => {
          baseRotateX.set(-90)
          baseRotateY.set(0)
        },
        showBottom: () => {
          baseRotateX.set(90)
          baseRotateY.set(0)
        },
        rotateTo: (x: number, y: number) => {
          baseRotateX.set(x)
          baseRotateY.set(y)
        },

        getCurrentRotation: () => currentRotation.current,
      }),
      []
    )

    const transform = useTransform(
      [springRotateX, springRotateY],
      ([x, y]) =>
        `translateZ(-${depth / 2}px) rotateX(${x}deg) rotateY(${y}deg)`
    )
    const handleStart = useCallback(
      (e: React.MouseEvent | React.TouchEvent) => {
        if (!draggable) return
        isDragging.current = true
        const point = 'touches' in e ? e.touches[0] : e
        startPosition.current = { x: point.clientX, y: point.clientY }
        startRotation.current = {
          x: baseRotateX.get(),
          y: baseRotateY.get(),
        }
      },
      [draggable]
    )

    const handleMove = useCallback((e: MouseEvent | TouchEvent) => {
      if (!isDragging.current) return
      const point = 'touches' in e ? e.touches[0] : e
      const deltaX = point.clientX - startPosition.current.x
      const deltaY = point.clientY - startPosition.current.y
      baseRotateX.set(startRotation.current.x - deltaY / 2)
      baseRotateY.set(startRotation.current.y + deltaX / 2)
    }, [])

    const handleEnd = useCallback(() => {
      isDragging.current = false
    }, [])

    useEffect(() => {
      if (draggable) {
        window.addEventListener("mousemove", handleMove)
        window.addEventListener("mouseup", handleEnd)
        window.addEventListener("touchmove", handleMove)
        window.addEventListener("touchend", handleEnd)
        return () => {
          window.removeEventListener("mousemove", handleMove)
          window.removeEventListener("mouseup", handleEnd)
          window.removeEventListener("touchmove", handleMove)
          window.removeEventListener("touchend", handleEnd)
        }
      }
    }, [draggable, handleMove, handleEnd])

    useEffect(() => {
      const unsubscribeX = baseRotateX.on("change", (v) => {
        currentRotation.current.x = v
      })
      const unsubscribeY = baseRotateY.on("change", (v) => {
        currentRotation.current.y = v
      })
      return () => {
        unsubscribeX()
        unsubscribeY()
      }
    }, [])

    return (
      <div
        className={cn(draggable && "cursor-move", className)}
        style={{
          width,
          height,
          perspective: `${perspective}px`,
        }}
        onMouseDown={handleStart}
        onTouchStart={handleStart}
        {...props}
      >
        <motion.div
          
          style={{ transform }}
        >
          {/* Front and Back */}
          <CubeFace
            transform={`rotateY(0deg) translateZ(${depth / 2}px)`}
            style={{ width, height }}
            showBackface={showBackface}
          >
            {faces.front}
          </CubeFace>

          <CubeFace
            transform={`rotateY(180deg) translateZ(${depth / 2}px)`}
            style={{ width, height }}
            showBackface={showBackface}
          >
            {faces.back}
          </CubeFace>

          {/* Right and Left */}
          <CubeFace
            transform={`rotateY(90deg) translateZ(${width / 2}px)`}
            style={{
              width: depth,
              height,
              left: (width - depth) / 2,
            }}
            showBackface={showBackface}
          >
            {faces.right}
          </CubeFace>

          <CubeFace
            transform={`rotateY(-90deg) translateZ(${width / 2}px)`}
            style={{
              width: depth,
              height,
              left: (width - depth) / 2,
            }}
            showBackface={showBackface}
          >
            {faces.left}
          </CubeFace>

          {/* Top and Bottom */}
          <CubeFace
            transform={`rotateX(90deg) translateZ(${height / 2}px)`}
            style={{
              width,
              height: depth,
              top: (height - depth) / 2,
            }}
            showBackface={showBackface}
          >
            {faces.top}
          </CubeFace>

          <CubeFace
            transform={`rotateX(-90deg) translateZ(${height / 2}px)`}
            style={{
              width,
              height: depth,
              top: (height - depth) / 2,
            }}
            showBackface={showBackface}
          >
            {faces.bottom}
          </CubeFace>
        </motion.div>
      </div>
    )
  }
)

CSSBox.displayName = "CSSBox"

export default CSSBox
```





## Usage

The component renders a fully-featured 3D cube. Pass `width`, `height`, `depth` and optionally six React nodes for the faces. You may also grab the cube with a ref for programmatic control. 


**File**: `High-level cube example`
```tsx
import { useRef } from "react"
import CSSBox, { CSSBoxRef } from "@/components/blocks/css-box"

export default function CubeExample() {
  const cubeRef = useRef<CSSBoxRef>(null)

  return (
    <>
      <CSSBox
        ref={cubeRef}
        width={220}
        height={220}
        depth={220}
        perspective={800}
        draggable
        faces={{
          front:  <img src="/images/front.png"  alt="Front"  />,
          back:   <img src="/images/back.png"   alt="Back"   />,
          left:   <img src="/images/left.png"   alt="Left"   />,
          right:  <img src="/images/right.png"  alt="Right"  />,
          top:    <img src="/images/top.png"    alt="Top"    />,
          bottom: <img src="/images/bottom.png" alt="Bottom" />,
        }}
      />

      <Button onClick={() => cubeRef.current?.showTop()}>
        Show Top
      </Button>
    </>
  )
}
```


## Understanding the component

Before you dive into it, I highly recommend reading [Intro to CSS 3D transforms](https://3dtransforms.desandro.com/) by David DeSandro. It's a really great resource for understanding the basics, and this component is essentially just a react & tailwind port of the Box chapter.

### Face layout

As you know, a box is a 3D object that has six faces. Each face is an absolutely-positioned `<div>` that lives in the same 3D context (`transform-style: preserve-3d`).  
We pre-rotate every face so that their local **+Z** axis points outward and then translate it by half of the appropriate dimension:


**File**: `Face layout`
```tsx
rotateY( 0deg) translateZ(depth / 2) → front
rotateY(180deg) translateZ(depth / 2) → back
rotateY( 90deg) translateZ(width / 2) → right
rotateY(-90deg) translateZ(width / 2) → left
rotateX( 90deg) translateZ(height/ 2) → top
rotateX(-90deg) translateZ(height/ 2) → bottom
```


### Rotation mechanics

1. Two motion values `baseRotateX` and `baseRotateY` hold the raw rotation in degrees.  
2. They are piped through `useSpring` so they feel springy and configurable (`stiffness`, `damping`). See [Motion – useSpring](https://motion.dev/docs/react-use-spring) for more details.
3. We combine them into a single CSS transform:


**File**: `Rotation mechanics`
```ts
const transform = useTransform([springX, springY], ([x, y]) =>
  `translateZ(-${depth / 2}px) rotateX(${x}deg) rotateY(${y}deg)`
)
```


### Drag interaction

The box can be rotated through mouse drags or touch input. 3D rotation can be a nasty thing, especially when dealing with [Gimbal Lock](https://base.movella.com/s/article/Understanding-Gimbal-Lock-and-how-to-prevent-it?language=en_US). While the almighty, super complex [quaternions](https://www.youtube.com/watch?v=zjMuIxRvygQ) could prevent this issue (Three.js provides great utilities for that), implementing them felt like overkill here - at that point, the entire box might as well be rendered in Three.js. 

The current approach maps mouse/touch movement directly to rotation around the X and Y axes. The implementation is pretty intuitive, while the actual feel of it can be sometimes unintuitive. Apologies for my laziness here.

When `draggable` is enabled, pointer movement gets translated into smooth rotational changes:


**File**: `Mouse movement to rotation`
```ts
Δx → rotateY
Δy → rotateX
```


We do this by subscribing to `mousemove` and `touchmove` events and projecting the movement to rotation deltas. During dragging the spring’s stiffness is temporarily halved to give a slightly “looser” feel. 


**File**: `Drag interaction`
```tsx
baseRotateX.set(startRotation.current.x - deltaY / 2)
baseRotateY.set(startRotation.current.y + deltaX / 2)
```


Modify that value to adjust the sensitivity of the drag.

### Imperative API

Via `ref` you can trigger the following methods:

- `showFront | showBack | showLeft | showRight | showTop | showBottom`
- `rotateTo(x: number, y: number)` – set exact angles
- `getCurrentRotation()` – read the live values

This can be handy for syncing cube state to a carousel or step-based walkthrough. For example, you can trigger a cube rotation with hover:


### Demo Code (css-box-hover-demo.tsx)
```tsx
import { useEffect, useRef } from "react"

import { cn } from "@/lib/utils"
import CSSBox, { CSSBoxRef } from "@/fancy/components/blocks/css-box"

const BoxText = ({
  children,
  className,
  i,
}: {
  children: React.ReactNode
  className?: string
  i: number
}) => (
  <div
    className={cn(
      "w-full h-full uppercase text-white flex items-center justify-center p-0 text-2xl md:text-3xl font-bold",
      className
    )}
  >
    {children}
  </div>
)

export default function CSSBoxHoverDemo() {
  const boxRefs = useRef<(CSSBoxRef | null)[]>([])
  const isRotating = useRef<boolean[]>([])
  const currentRotations = useRef<number[]>([])

  const boxes = [
    { text: "January 15, 2025", size: 300 },
    { text: "Live Q&A", size: 200 },
    { text: "10:00", size: 120 },
    { text: "to", size: 70 },
    { text: "11:30", size: 120 },
    { text: "CET", size: 120 },
    { text: "Online", size: 180 },
    { text: "Recording Available", size: 380 },
    { text: "In English", size: 220 },
    { text: "Register Now", size: 280 },
    { text: "Free Access", size: 240 },
  ]

  useEffect(() => {
    currentRotations.current = new Array(boxes.length).fill(0)
  }, [])

  const handleHover = async (index: number) => {
    if (isRotating.current[index]) return

    isRotating.current[index] = true
    const box = boxRefs.current[index]
    if (!box) return

    const nextRotation = currentRotations.current[index] + 90
    currentRotations.current[index] = nextRotation

    box.rotateTo(0, nextRotation)

    isRotating.current[index] = false
  }

  return (
    <div >
      {boxes.map(({ text, size }, index) => (
        <CSSBox
          key={index}
          ref={(el) => {
            if (el) {
              boxRefs.current[index] = el
              isRotating.current[index] = false
              currentRotations.current[index] = 0
            }
          }}
          width={size}
          height={35}
          depth={size}
          draggable={false}
          
          onMouseEnter={() => handleHover(index)}
          faces={{
            front: <BoxText i={index}>{text}</BoxText>,
            back: (
              <BoxText i={index} >
                {text}
              </BoxText>
            ),
            left: <BoxText i={index}>{text}</BoxText>,
            right: (
              <BoxText i={index} >
                {text}
              </BoxText>
            ),
          }}
        />
      ))}
    </div>
  )
}
```


Or, tie the rotation to a scroll progress:


### Demo Code (css-box-scroll-demo.tsx)
```tsx
import { useRef } from "react"
import { useScroll, useTransform } from "motion/react"

import { cn } from "@/lib/utils"
import useScreenSize from "@/hooks/use-screen-size"
import CSSBox, { CSSBoxRef } from "@/fancy/components/blocks/css-box"

const BoxFace = ({
  imageUrl,
  className,
}: {
  imageUrl: string
  className?: string
}) => (
  <div className={cn("w-full h-full relative", className)}>
    <img src={imageUrl} alt=""  />
    <div
      
      style={{
        maskImage: "linear-gradient(to top, white 20%, transparent 100%)",
        WebkitMaskImage: "linear-gradient(to top, white 20%, transparent 100%)",
        backgroundColor: "rgba(255,255,255,0.4)",
        backdropFilter: "blur(8px)",
      }}
    >
      <div >
        <div >
          <div >JUN14</div>
          <div >
            New Arrivals
          </div>
          <div >SS25</div>
        </div>
      </div>
    </div>
  </div>
)

export default function CSSBoxScrollDemo() {
  const boxRef = useRef<CSSBoxRef>(null)
  const containerRef = useRef<HTMLDivElement>(null)
  const screenSize = useScreenSize()

  const { scrollYProgress } = useScroll({
    container: containerRef,
  })

  // Transform scroll progress (0-1) to rotation (0-360)
  const rotation = useTransform(scrollYProgress, [0, 1], [0, 360])

  // Update box rotation when scroll transform changes
  rotation.on("change", (latest) => {
    boxRef.current?.rotateTo(0, latest)
  })

  const imageUrl =
    "https://cdn.cosmos.so/276cdd4e-8a7a-4c32-955f-83c5900a0926?format=jpeg"

  const boxWidth = screenSize.lessThan("md") ? 220 : 300
  const boxHeight = screenSize.lessThan("md") ? 300 : 400
  const boxDepth = screenSize.lessThan("md") ? 220 : 300

  return (
    <div
      ref={containerRef}
      
    >
      <div >
        <div >
          <CSSBox
            ref={boxRef}
            width={boxWidth}
            height={boxHeight}
            depth={boxDepth}
            perspective={2000}
            draggable={false}
            faces={{
              front: <BoxFace imageUrl={imageUrl} />,
              back: <BoxFace imageUrl={imageUrl} />,
              left: <BoxFace imageUrl={imageUrl} />,
              right: <BoxFace imageUrl={imageUrl} />,
            }}
          />
        </div>
      </div>
    </div>
  )
}
```


## Notes

As it was pointed out above, implementing a similar component in Three.js would have been a lot easier and would give you much more flexibility and overall control over the rotation. You are still welcomed to use this component if you'd like to skip installing Three.js for whatever reason :). 

## Resources

- [Intro to CSS 3D transforms](https://3dtransforms.desandro.com/) by David DeSandro
- [Gimbal Lock](https://base.movella.com/s/article/Understanding-Gimbal-Lock-and-how-to-prevent-it?language=en_US) 
- [Quaternions explained](https://www.youtube.com/watch?v=zjMuIxRvygQ) by 3Blue1Brown

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| width* | `number` | - | Width of the cube (in&nbsp;px) |
| height* | `number` | - | Height of the cube (in&nbsp;px) |
| depth* | `number` | - | Depth of the cube (in&nbsp;px) |
| perspective | `number` | `600` | Perspective distance applied to the outer wrapper |
| stiffness | `number` | `100` | Spring stiffness for rotations |
| damping | `number` | `30` | Spring damping factor |
| className | `string` | - | Additional classes for the outer wrapper |
| showBackface | `boolean` | `false` | Reveal back-faces if you need double-sided content |
| faces | `{ front? back? left? right? top? bottom?: ReactNode }` | - | Individual React nodes for every face |
| draggable | `boolean` | `true` | Enable/disable mouse &amp; touch rotation |


## Ref Methods

The component exposes several methods through a ref that allow programmatic control of the cube's rotation:


| Method | Type | Description |
| --- | --- | --- |
| showFront | `() => void` | Rotates the cube to show the front face (0°, 0°) |
| showBack | `() => void` | Rotates the cube to show the back face (0°, 180°) |
| showLeft | `() => void` | Rotates the cube to show the left face (0°, -90°) |
| showRight | `() => void` | Rotates the cube to show the right face (0°, 90°) |
| showTop | `() => void` | Rotates the cube to show the top face (-90°, 0°) |
| showBottom | `() => void` | Rotates the cube to show the bottom face (90°, 0°) |
| rotateTo | `(x: number, y: number) => void` | Rotates the cube to specific X and Y angles in degrees |
| getCurrentRotation | `() => { x: number, y: number }` | Returns current X and Y rotation angles in degrees |


---

### Screensaver <a name="screensaver"></a>

### Demo Code (screensaver-demo.tsx)
```tsx
import React from "react"
import { exampleImages } from "@/utils/demo-images"

import Screensaver from "@/fancy/components/blocks/screensaver"

const CirclingElementsDemo: React.FC = () => {
  const containerRef = React.useRef<HTMLDivElement>(null)

  return (
    <div
      
      ref={containerRef}
    >
      <h1 >
        page not found
      </h1>
      {[...exampleImages, ...exampleImages].map((image, index) => (
        <Screensaver
          key={index}
          speed={1}
          startPosition={{ x: index * 3, y: index * 3 }}
          startAngle={40}
          containerRef={containerRef}
        >
          <div >
            <img
              src={image.url}
              alt={`Example ${index + 1}`}
              
            />
          </div>
        </Screensaver>
      ))}
    </div>
  )
}

export default CirclingElementsDemo
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/screensaver
```






*Component Source: use-dimensions (Source file not found)*



### Component Source Code (screensaver.tsx)
```tsx
"use client"

import React, { useEffect, useRef } from "react"
import {
  motion,
  useAnimationFrame,
  useMotionValue,
} from "motion/react"

import { cn } from "@/lib/utils"
import { useDimensions } from "@/hooks/use-dimensions"

type ScreensaverProps = {
  children: React.ReactNode
  containerRef: React.RefObject<HTMLElement | null>
  speed?: number
  startPosition?: { x: number; y: number } // x,y as percentages (0-100)
  startAngle?: number // in degrees
  className?: string
}

const Screensaver: React.FC<ScreensaverProps> = ({
  children,
  speed = 3,
  startPosition = { x: 0, y: 0 },
  startAngle = 45,
  containerRef,
  className,
}) => {
  const elementRef = useRef<HTMLDivElement>(null)
  const x = useMotionValue(0)
  const y = useMotionValue(0)
  const angle = useRef((startAngle * Math.PI) / 180)

  const containerDimensions = useDimensions(containerRef)
  const elementDimensions = useDimensions(elementRef)

  // Set initial position based on container dimensions and percentage
  useEffect(() => {
    if (containerDimensions.width && containerDimensions.height) {
      const initialX =
        (startPosition.x / 100) *
        (containerDimensions.width - (elementDimensions.width || 0))
      const initialY =
        (startPosition.y / 100) *
        (containerDimensions.height - (elementDimensions.height || 0))
      x.set(initialX)
      y.set(initialY)
    }
  }, [containerDimensions, elementDimensions, startPosition])

  useAnimationFrame(() => {
    const velocity = speed
    const dx = Math.cos(angle.current) * velocity
    const dy = Math.sin(angle.current) * velocity

    let newX = x.get() + dx
    let newY = y.get() + dy

    // Check for collisions with container boundaries
    if (
      newX <= 0 ||
      newX + elementDimensions.width >= containerDimensions.width
    ) {
      angle.current = Math.PI - angle.current
      newX = Math.max(
        0,
        Math.min(newX, containerDimensions.width - elementDimensions.width)
      )
    }
    if (
      newY <= 0 ||
      newY + elementDimensions.height >= containerDimensions.height
    ) {
      angle.current = -angle.current
      newY = Math.max(
        0,
        Math.min(newY, containerDimensions.height - elementDimensions.height)
      )
    }

    x.set(newX)
    y.set(newY)
  })

  return (
    <motion.div
      ref={elementRef}
      style={{
        position: "absolute",
        top: 0,
        left: 0,
        x,
        y,
      }}
      className={cn("transform will-change-transform", className)}
    >
      {children}
    </motion.div>
  )
}

export default Screensaver
```





## Usage

Just wrap your content with the component, and the animation will take care of the rest.
You also need to pass a container ref to the component — which will be used to constrain the screensaver component.

## Credits

Ported to Framer by [Achille Ernoult](https://x.com/achilleernlt)

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The content to be displayed |
| containerRef* | `React.RefObject` | - | Reference to the container for the screensaver |
| speed | `number` | `3` | Speed of the animation in pixels per second |
| startPosition | `{ x: number; y: number }` | `{ x: 0, y: 0 }` | Starting position of the element |
| startAngle | `number` | `45` | Starting angle of the element in degrees |
| className | `string` | - | Additional CSS classes for styling |


---

### Sticky Footer <a name="sticky-footer"></a>

### Demo Code (sticky-footer-demo.tsx)
```tsx
import React from "react"

const Preview: React.FC = () => {
  return (
    <div >
      {/* add relative positioning to the main conent */}
      <div >
        Scroll down ↓
      </div>

      {/* Sticky footer. The only important thing here is the z-index, the sticky position and the bottom value */}
      <div >
        <div >
          <div >
            <ul>
              <li >Home</li>
              <li >Docs</li>
              <li >Comps</li>
            </ul>
            <ul>
              <li >Github</li>
              <li >Instagram</li>
              <li >X (Twitter)</li>
            </ul>
          </div>
          <h2 >
            fancy
          </h2>
        </div>
      </div>
    </div>
  )
}

export default Preview
```


## How to

This page doesn't contain a component, as achieving a sticky footer doesn't require any complex logic abstracted into a component. Some tutorials tend to overcomplicate this, but for most cases, it's enough to add a few Tailwind classes to our elements, which you can find in this demo.

You need two things to make this work:

- A main element that will sit on top of the footer
- A footer element that will be behind the main element

1. Usually, you want the main element to be **at least** `h-[100vh]` (or `h-[100%]` if you use it inside a container, like in the demo above), so that it fully hides the footer by default
2. You also need to set the position to `relative`, so the z-index will work correctly
3. Then, set the footer element's position to `sticky` and make it stick to the bottom with `bottom-0`
4. Finally, make sure that the main element has a higher z-index than the footer element so it always sits on top of the footer

That's it! Can you believe that?

## Notes

The main drawback to be aware of is that the footer element will always be behind the main content in the viewport. This can occasionally interfere with pointer events and components that rely on z-index stacking. However, in my experience this approach works well for most common use cases.


---

### Float <a name="float"></a>

### Demo Code (float-demo.tsx)
```tsx
"use client"

import { exampleImages } from "@/utils/demo-images"
import { motion } from "motion/react"

import Float from "@/fancy/components/blocks/float"

export default function FloatDemo() {
  return (
    <div >
      <div >
        <motion.div
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.25, delay: 0.5, ease: "easeOut" }}
        >
          <Float>
            <div >
              <img
                src={exampleImages[4].url}
                
              />
            </div>
          </Float>
        </motion.div>
        <motion.h2
          
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.25, delay: 0.7, ease: "easeOut" }}
        >
          Album of the week
        </motion.h2>
      </div>
    </div>
  )
}
```


## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/float
```






### Component Source Code (float.tsx)
```tsx
"use client"

import React, { useRef } from "react"
import { motion, useAnimationFrame, useMotionValue } from "motion/react"

import { cn } from "@/lib/utils"

type FloatProps = {
  children: React.ReactNode
  speed?: number
  amplitude?: [number, number, number] // [x, y, z]
  rotationRange?: [number, number, number] // [x, y, z]
  timeOffset?: number
  className?: string
}

const Float: React.FC<FloatProps> = ({
  children,
  speed = 0.5,
  amplitude = [10, 30, 30], // Default [x, y, z] amplitudes
  rotationRange = [15, 15, 7.5], // Default [x, y, z] rotation ranges
  timeOffset = 0,
  className,
}) => {
  const x = useMotionValue(0)
  const y = useMotionValue(0)
  const z = useMotionValue(0)
  const rotateX = useMotionValue(0)
  const rotateY = useMotionValue(0)
  const rotateZ = useMotionValue(0)

  // Use refs for animation values to avoid recreating the animation frame callback
  const time = useRef(0)

  useAnimationFrame(() => {
    time.current += speed * 0.02

    // Smooth floating motion on all axes
    const newX = Math.sin(time.current * 0.7 + timeOffset) * amplitude[0]
    const newY = Math.sin(time.current * 0.6 + timeOffset) * amplitude[1]
    const newZ = Math.sin(time.current * 0.5 + timeOffset) * amplitude[2]

    // 3D rotations with different frequencies for more organic movement
    const newRotateX =
      Math.sin(time.current * 0.5 + timeOffset) * rotationRange[0]
    const newRotateY =
      Math.sin(time.current * 0.4 + timeOffset) * rotationRange[1]
    const newRotateZ =
      Math.sin(time.current * 0.3 + timeOffset) * rotationRange[2]

    x.set(newX)
    y.set(newY)
    z.set(newZ)
    rotateX.set(newRotateX)
    rotateY.set(newRotateY)
    rotateZ.set(newRotateZ)
  })

  return (
    <motion.div
      style={{
        x,
        y,
        z,
        rotateX,
        rotateY,
        rotateZ,
        transformStyle: "preserve-3d",
      }}
      className={cn("will-change-transform", className)}
    >
      {children}
    </motion.div>
  )
}

export default Float
```





## Usage

Just wrap your content you want to float with the `Float` component, and the animation will take care of the rest.

## Understanding the component

The component creates a smooth floating animation using sine waves for both movement and rotation. It accepts three main props:

1. **Movement**: The `amplitude` prop controls movement range on X, Y and Z axes in pixels.

2. **Rotation**: The `rotationRange` prop sets maximum rotation angles in degrees for each axis.

3. **Animation Speed**: The `speed` prop (default: 0.5) controls animation speed - higher is faster.

## Examples

By default, multiple Float components will move in unison. Use the `timeOffset` prop to create more organic movement.


### Demo Code (float-offset-demo.tsx)
```tsx
import { cn } from "@/lib/utils"
import Float from "@/fancy/components/blocks/float"

export default function FloatDemo() {
  const texts = [
    { text: "@mdx-js/loader", position: "top-[0%] left-[20%]" },
    { text: "@mdx-js/react", position: "top-[20%] left-[80%]" },
    { text: "@next/mdx", position: "top-[70%] left-[40%]" },
    { text: "@vercel/analytics", position: "top-[80%] left-[30%]" },
    { text: "class-variance-authority", position: "top-[40%] left-[0%]" },
    { text: "clsx", position: "top-[15%] left-[45%]" },
    { text: "flubber", position: "top-[65%] left-[85%]" },
    { text: "motion", position: "top-[85%] left-[15%]" },
    { text: "lenis", position: "top-[35%] left-[75%]" },
    { text: "lodash", position: "top-[75%] left-[55%]" },
    { text: "lucide-react", position: "top-[25%] left-[35%]" },
    { text: "matter-js", position: "top-[45%] left-[25%]" },
    { text: "mdast-util-toc", position: "top-[55%] left-[65%]" },
    { text: "next", position: "top-[90%] left-[45%]" },
    { text: "next-mdx-remote", position: "top-[10%] left-[70%]" },
    { text: "poly-decomp", position: "top-[60%] left-[10%]" },
    { text: "react", position: "top-[30%] left-[50%]" },
    { text: "react-dom", position: "top-[95%] left-[60%]" },
    { text: "react-syntax-highlighter", position: "top-[5%] left-[90%]" },
    { text: "react-wrap-balancer", position: "top-[82%] left-[75%]" },
    { text: "rehype-pretty-code", position: "top-[28%] left-[15%]" },
    { text: "remark", position: "top-[67%] left-[5%]" },
    { text: "svg-path-commander", position: "top-[92%] left-[25%]" },
    { text: "tailwind-merge", position: "top-[28%] left-[95%]" },
    { text: "tailwindcss-animate", position: "top-[73%] left-[20%]" },
    { text: "zod", position: "top-[8%] left-[40%]" },
  ]

  return (
    <div >
      {texts.map((item, i) => (
        <Float
          key={i}
          timeOffset={i * 0.8}
          amplitude={[
            15 + Math.random() * 20,
            25 + Math.random() * 30,
            20 + Math.random() * 25,
          ]}
          rotationRange={[
            10 + Math.random() * 10,
            10 + Math.random() * 10,
            5 + Math.random() * 5,
          ]}
          speed={0.3 + Math.random() * 0.4}
          className={cn(
            "absolute text-lg flex sm:text-xl md:text-2xl font-light hover:underline cursor-pointer text-primary-blue",
            item.position
          )}
        >
          <p>{item.text}</p>
        </Float>
      ))}
    </div>
  )
}
```


## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `React.ReactNode` | - | The content to be animated |
| speed | `number` | `0.5` | Speed of the floating animation |
| amplitude | `[number, number, number]` | `[10, 30, 30]` | Movement range in pixels for X, Y and Z axes |
| rotationRange | `[number, number, number]` | `[15, 15, 7.5]` | Maximum rotation in degrees for X, Y and Z axes |
| timeOffset | `number` | `0` | Offset to stagger animations between multiple instances |
| className | `string` | - | Additional CSS classes for styling |


---

### Stacking Cards <a name="stacking-cards"></a>

### Demo Code (stacking-cards-demo.tsx)
```tsx
// author: Khoa Phan <https://www.pldkhoa.dev>

"use client"

import { useRef } from "react"
import Image from "next/image"

import { cn } from "@/lib/utils"
import StackingCards, {
  StackingCardItem,
} from "@/fancy/components/blocks/stacking-cards"

const cards = [
  {
    bgColor: "bg-primary-orange",
    title: "The Guiding Light",
    description:
      "Lighthouses have stood as beacons of hope for centuries, guiding sailors safely through treacherous waters. Their glowing light and towering presence serve as a reminder of humanity’s connection to the sea.",
    image:
      "https://plus.unsplash.com/premium_vector-1739262161806-d954eb02427c?w=800&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxjb2xsZWN0aW9uLXBhZ2V8MXxxdGU5Smx2R3d0b3x8ZW58MHx8fHx8",
  },
  {
    bgColor: "bg-primary-blue",
    title: "Life Beneath the Waves",
    description:
      "From shimmering schools of fish to solitary hunters, the ocean is home to an incredible variety of marine life. Each species plays a vital role in maintaining the balance of underwater ecosystems.",
    image:
      "https://plus.unsplash.com/premium_vector-1739200616200-69a138d91627?w=800&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxjb2xsZWN0aW9uLXBhZ2V8MnxxdGU5Smx2R3d0b3x8ZW58MHx8fHx8",
  },
  {
    bgColor: "bg-primary-red",
    title: "Alone on the Open Sea",
    description:
      "Drifting across the endless horizon, traveling alone on the sea is a test of courage and resilience. With nothing but the waves and the sky, solitude becomes both a challenge and a source of deep reflection.",
    image:
      "https://plus.unsplash.com/premium_vector-1738597190290-a3b571590b9e?w=800&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxjb2xsZWN0aW9uLXBhZ2V8OHxxdGU5Smx2R3d0b3x8ZW58MHx8fHx8",
  },
  {
    bgColor: "bg-teal",
    title: "The Art of Sailing",
    description:
      "Harnessing the power of the wind, sailing is both a skill and an adventure. Whether racing across the waves or leisurely cruising, it’s a timeless way to explore the vast blue expanse.",
    image:
      "https://plus.unsplash.com/premium_vector-1738935247245-97940c74cced?w=800&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxjb2xsZWN0aW9uLXBhZ2V8MTZ8cXRlOUpsdkd3dG98fGVufDB8fHx8fA%3D%3D",
  },
  {
    bgColor: "bg-primary-blue",
    title: "The Era of Whaling",
    description:
      "Once a thriving industry, whale hunting shaped economies and cultures across the world. Today, efforts to protect these majestic creatures highlight the shift toward conservation and respect for marine life.",
    image:
      "https://plus.unsplash.com/premium_vector-1738935247692-1c2f2c924fd8?w=800&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxjb2xsZWN0aW9uLXBhZ2V8MjJ8cXRlOUpsdkd3dG98fGVufDB8fHx8fA%3D%3D",
  },
]

export default function StackingCardsDemo() {
  const container = useRef<HTMLDivElement>(null)

  return (
    <div
      
      ref={container}
    >
      <StackingCards
        totalCards={cards.length}
        scrollOptions={{ container: container }}
      >
        <div >
          Scroll down ↓
        </div>
        {cards.map(({ bgColor, description, image, title }, index) => {
          return (
            <StackingCardItem key={index} index={index} >
              <div
                className={cn(
                  bgColor,
                  "h-[80%] sm:h-[70%] flex-col sm:flex-row aspect-video px-8 py-10 flex w-11/12 rounded-3xl mx-auto relative"
                )}
              >
                <div >
                  <h3 >{title}</h3>
                  <p>{description}</p>
                </div>

                <div >
                  <Image
                    src={image}
                    alt={title}
                    
                    fill
                  />
                </div>
              </div>
            </StackingCardItem>
          )
        })}

        <div >
          <h2 >
            fancy
          </h2>
        </div>
      </StackingCards>
    </div>
  );
}
```


## Installation


  
    CLI
    Manual
  
  

  
```bash
# Install command:
npx shadcn add @fancy/stacking-cards
```


  



### Component Source Code (stacking-cards.tsx)
```tsx
// author: Khoa Phan <https://www.pldkhoa.dev>

"use client"

import {
  createContext,
  useContext,
  useRef,
  type HTMLAttributes,
  type PropsWithChildren,
} from "react"
import {
  motion,
  useScroll,
  useTransform,
  type MotionValue,
  type UseScrollOptions,
} from "motion/react"

import { cn } from "@/lib/utils"

interface StackingCardsProps
  extends PropsWithChildren,
    HTMLAttributes<HTMLDivElement> {
  scrollOptions?: UseScrollOptions
  scaleMultiplier?: number
  totalCards: number
}

interface StackingCardItemProps
  extends HTMLAttributes<HTMLDivElement>,
    PropsWithChildren {
  index: number
  topPosition?: string
}

export default function StackingCards({
  children,
  className,
  scrollOptions,
  scaleMultiplier,
  totalCards,
  ...props
}: StackingCardsProps) {
  const targetRef = useRef<HTMLDivElement>(null)
  const { scrollYProgress } = useScroll({
    offset: ["start start", "end end"],
    ...scrollOptions,
    target: targetRef,
  })

  return (
    <StackingCardsContext.Provider
      value={{ progress: scrollYProgress, scaleMultiplier, totalCards }}
    >
      <div className={cn(className)} ref={targetRef} {...props}>
        {children}
      </div>
    </StackingCardsContext.Provider>
  )
}

const StackingCardItem = ({
  index,
  topPosition,
  className,
  children,
  ...props
}: StackingCardItemProps) => {
  const {
    progress,
    scaleMultiplier,
    totalCards = 0,
  } = useStackingCardsContext() // Get from Context
  const scaleTo = 1 - (totalCards - index) * (scaleMultiplier ?? 0.03)
  const rangeScale = [index * (1 / totalCards), 1]
  const scale = useTransform(progress, rangeScale, [1, scaleTo])
  const top = topPosition ?? `${5 + index * 3}%`

  return (
    <div className={cn("h-full sticky top-0", className)} {...props}>
      <motion.div
        className={"origin-top relative h-full"}
        style={{ top, scale }}
      >
        {children}
      </motion.div>
    </div>
  )
}

const StackingCardsContext = createContext<{
  progress: MotionValue<number>
  scaleMultiplier?: number
  totalCards?: number
} | null>(null)

export const useStackingCardsContext = () => {
  const context = useContext(StackingCardsContext)
  if (!context)
    throw new Error("StackingCardItem must be used within StackingCards")
  return context
}

export { StackingCardItem }
```





## Usage

Wrap `StackingCards` around the content you want to animate and `StackingCardItem` around each card you want to animate.
The structure looks like this:


**File**: `Stacking cards usage example`
```tsx
<StackingCards>
  <StackingCardItem>
      {/* Your card goes here */}
  </StackingCardItem>
</StackingCards>
```


## Understanding the component

The component utilizes scroll progress to determine the scale of each element. The first element has the highest scale multiplier, making it the smallest when it reaches the bottom of the container's scroll area, while the last element follows the opposite pattern, creating a layered effect.

To achieve this, I use each element's index to calculate its scale multiplier. Just simple as that! 😀

## Notes

- By default, this component uses the `window` to track scroll progress. However, in some cases, you may want to wrap it inside another scrollable container. To achieve this, simply define the container for `useScroll` from `motion`. In the `Demo` above, I defined the `containerRef` and passed it to the `scrollOptions` prop of the `StackingCards` component.

- To ensure `StackingCardItem` works correctly, you need to define its height. This allows the wrapper to have a larger height than the card itself, ensuring that the `topPosition` functions properly.

## Props

### StackingCards


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| totalCards* | `number` | `0` | Total number of cards to be animated (this is for calculating the scale         intensity) |
| scaleMultiplier | `number` | `0.03` | The intensity of the card to scale |
| scrollOptons | `UseScrollOptions` | `{offset: ["start start", "end end"]}` | Scroll options for `useScroll` hook from `motion`. |
| className | `string` | - | `className` for the container |
| Other Props | - | - | All attributes for `HTMLDivElement` |


### StackingCardItem


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| index* | `number` | - | `index` value of your card (to calculate scale intensity) |
| topPosition | `string` | `5 + index * 3` | The top position of the card |
| className | `string` | - | `className` for the `StackingCardItem` element |
| Other Props | - | - | All attributes for `HTMLDivElement` |


---

### Simple Marquee <a name="simple-marquee"></a>

### Demo Code (simple-marquee-demo.tsx)
```tsx
import React, { useRef } from "react"

import SimpleMarquee from "@/fancy/components/blocks/simple-marquee"

const exampleImages = [
  "https://cdn.cosmos.so/4b771c5c-d1eb-4948-b839-255dbeb931ba?format=jpeg",
  "https://cdn.cosmos.so/a8d82afd-2293-43ad-bac3-887683d85b44?format=jpeg",
  "https://cdn.cosmos.so/49206ba5-c174-4cd5-aee8-5b744842e6c2?format=jpeg",
  "https://cdn.cosmos.so/b29bd150-6477-420f-8efb-65ed99694421?format=jpeg",
  "https://cdn.cosmos.so/e1a0313e-7617-431d-b7f1-f1b169e6bcb4?format=jpeg",
  "https://cdn.cosmos.so/ad640c12-69fb-4186-bc3d-b1cc93986a37?format=jpeg",
  "https://cdn.cosmos.so/5cf0c3d2-e785-41a3-b0c8-a073ee2f2862?format=jpeg",
  "https://cdn.cosmos.so/938ab21c-a975-41b3-b303-418290343b09?format=jpeg",
  "https://cdn.cosmos.so/2e14a9bb-27e3-40fd-b940-cfb797a1224c?format=jpeg",
  "https://cdn.cosmos.so/81841d9f-e164-4770-aebc-cfc97d72f3ab?format=jpeg",
  "https://cdn.cosmos.so/49b81db0-37ea-4569-b0d6-04afa5115a10?format=jpeg",
  "https://cdn.cosmos.so/ade1834b-9317-44fb-8dc3-b43d29acd409?format=jpeg",
  "https://cdn.cosmos.so/621c250c-3833-45f9-862a-3f400aaf8f28?format=jpeg",
  "https://cdn.cosmos.so/f9b7eae8-e5a6-4ce6-b6e1-9ef125ba7f8e?format=jpeg",
  "https://cdn.cosmos.so/bd56ed6d-1bbd-44a4-b1a1-79b7199bbebb?format=jpeg",
]

const MarqueeItem = ({ children }: { children: React.ReactNode }) => (
  <div >
    {children}
  </div>
)

export default function SimpleMarqueeDemo() {
  const firstThird = exampleImages.slice(
    0,
    Math.floor(exampleImages.length / 3)
  )
  const secondThird = exampleImages.slice(
    Math.floor(exampleImages.length / 3),
    Math.floor((2 * exampleImages.length) / 3)
  )
  const lastThird = exampleImages.slice(
    Math.floor((2 * exampleImages.length) / 3)
  )

  const container = useRef<HTMLDivElement>(null)

  return (
    <div
      
      ref={container}
    >
      <h1 >
        Weekly Finds
      </h1>
      <div >
        <SimpleMarquee
          
          baseVelocity={8}
          repeat={4}
          draggable={false}
          scrollSpringConfig={{ damping: 50, stiffness: 400 }}
          slowDownFactor={0.1}
          slowdownOnHover
          slowDownSpringConfig={{ damping: 60, stiffness: 300 }}
          scrollAwareDirection={true}
          scrollContainer={container}
          useScrollVelocity={true}
          direction="left"
        >
          {firstThird.map((src, i) => (
            <MarqueeItem key={i}>
              <img
                src={src}
                alt={`Image ${i + 1}`}
                
              />
            </MarqueeItem>
          ))}
        </SimpleMarquee>

        <SimpleMarquee
          
          baseVelocity={8}
          repeat={4}
          scrollAwareDirection={true}
          scrollSpringConfig={{ damping: 50, stiffness: 400 }}
          slowdownOnHover
          slowDownFactor={0.1}
          slowDownSpringConfig={{ damping: 60, stiffness: 300 }}
          useScrollVelocity={true}
          scrollContainer={container}
          draggable={false}
          direction="right"
        >
          {secondThird.map((src, i) => (
            <MarqueeItem key={i}>
              <img
                src={src}
                alt={`Image ${i + firstThird.length}`}
                
              />
            </MarqueeItem>
          ))}
        </SimpleMarquee>

        <SimpleMarquee
          
          baseVelocity={8}
          repeat={4}
          draggable={false}
          scrollSpringConfig={{ damping: 50, stiffness: 400 }}
          slowDownFactor={0.1}
          slowdownOnHover
          slowDownSpringConfig={{ damping: 60, stiffness: 300 }}
          scrollAwareDirection={true}
          scrollContainer={container}
          useScrollVelocity={true}
          direction="left"
        >
          {lastThird.map((src, i) => (
            <MarqueeItem key={i}>
              <img
                src={src}
                alt={`Image ${i + firstThird.length + secondThird.length}`}
                
              />
            </MarqueeItem>
          ))}
        </SimpleMarquee>
      </div>
    </div>
  );
}
```


Artworks from [Cosmos](https://www.cosmos.so/danielpetho/gradients/).

## Credits

This component is inspired by [this scroll example](https://codesandbox.io/p/sandbox/framer-motion-scroll-velocity-r1dy4u?from-embed) by Motion. 

## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/simple-marquee
```






### Component Source Code (simple-marquee.tsx)
```tsx
import { RefObject, useRef } from "react"
import {
  motion,
  SpringOptions,
  useAnimationFrame,
  useMotionValue,
  useScroll,
  useSpring,
  useTransform,
  useVelocity,
} from "motion/react"

import { cn } from "@/lib/utils"

// Custom wrap function
const wrap = (min: number, max: number, value: number): number => {
  const range = max - min
  return ((((value - min) % range) + range) % range) + min
}

interface SimpleMarqueeProps {
  children: React.ReactNode // The elements to be scrolled
  className?: string // Additional CSS classes for the container
  direction?: "left" | "right" | "up" | "down" // The direction of the marquee
  baseVelocity?: number // The base velocity of the marquee in pixels per second
  easing?: (value: number) => number // The easing function for the animation
  slowdownOnHover?: boolean // Whether to slow down the animation on hover
  slowDownFactor?: number // The factor to slow down the animation on hover
  slowDownSpringConfig?: SpringOptions // The spring config for the slow down animation
  useScrollVelocity?: boolean // Whether to use the scroll velocity to control the marquee speed
  scrollAwareDirection?: boolean // Whether to adjust the direction based on the scroll direction
  scrollSpringConfig?: SpringOptions // The spring config for the scroll velocity-based direction adjustment
  scrollContainer?: RefObject<HTMLElement | null> | HTMLElement | null // The container to use for the scroll velocity
  repeat?: number // The number of times to repeat the children.
  draggable?: boolean // Whether to allow dragging of the marquee
  dragSensitivity?: number // The sensitivity of the drag movement
  dragVelocityDecay?: number // The decay of the drag velocity. This means how fast the velocity will gradually reduce to baseVelocity when we release the drag
  dragAwareDirection?: boolean // Whether to adjust the direction based on the drag velocity
  dragAngle?: number // The angle of the drag movement in degrees. This is useful if you eg. rotating your marquee by 45 degrees
  grabCursor?: boolean // Whether to change the cursor to grabbing when dragging
}

const SimpleMarquee = ({
  children,
  className,
  direction = "right",
  baseVelocity = 5,
  slowdownOnHover = false,
  slowDownFactor = 0.3,
  slowDownSpringConfig = { damping: 50, stiffness: 400 },
  useScrollVelocity = false,
  scrollAwareDirection = false,
  scrollSpringConfig = { damping: 50, stiffness: 400 },
  scrollContainer,
  repeat = 3,
  draggable = false,
  dragSensitivity = 0.2,
  dragVelocityDecay = 0.96,
  dragAwareDirection = false,
  dragAngle = 0,
  grabCursor = false,
  easing,
}: SimpleMarqueeProps) => {
  const baseX = useMotionValue(0)
  const baseY = useMotionValue(0)

  const { scrollY } = useScroll({
    ...(scrollContainer && {
      container: scrollContainer as RefObject<HTMLDivElement>,
    }),
  })

  const scrollVelocity = useVelocity(scrollY)
  const smoothVelocity = useSpring(scrollVelocity, scrollSpringConfig)

  const hoverFactorValue = useMotionValue(1)
  const defaultVelocity = useMotionValue(1)

  // Track if user is currently dragging
  const isDragging = useRef(false)

  // Store drag velocity
  const dragVelocity = useRef(0)

  const smoothHoverFactor = useSpring(hoverFactorValue, slowDownSpringConfig)

  // Transform scroll velocity into a factor that affects marquee speed
  const velocityFactor = useTransform(
    useScrollVelocity ? smoothVelocity : defaultVelocity,
    [0, 1000],
    [0, 5],
    {
      clamp: false,
    }
  )

  // Determine if movement is horizontal or vertical.
  const isHorizontal = direction === "left" || direction === "right"

  // Convert baseVelocity to the correct direction
  const actualBaseVelocity =
    direction === "left" || direction === "up" ? -baseVelocity : baseVelocity

  // Reference to track if mouse is hovering
  const isHovered = useRef(false)

  // Direction factor for changing direction based on scroll or drag
  const directionFactor = useRef(1)

  // Transform baseX/baseY into a percentage for the transform
  // The wrap function ensures the value stays between 0 and -100
  const x = useTransform(baseX, (v) => {
    // Apply easing if provided, otherwise use linear (v directly)
    const wrappedValue = wrap(0, -100, v)
    return `${easing ? easing(wrappedValue / -100) * -100 : wrappedValue}%`
  })
  const y = useTransform(baseY, (v) => {
    // Apply easing if provided, otherwise use linear (v directly)
    const wrappedValue = wrap(0, -100, v)
    return `${easing ? easing(wrappedValue / -100) * -100 : wrappedValue}%`
  })

  useAnimationFrame((t, delta) => {
    if (isDragging.current && draggable) {
      if (isHorizontal) {
        baseX.set(baseX.get() + dragVelocity.current)
      } else {
        baseY.set(baseY.get() + dragVelocity.current)
      }

      // Add decay to dragVelocity when not moving
      // This will gradually reduce the velocity to zero when the pointer isn't moving
      dragVelocity.current *= 0.9

      // Stop completely if velocity is very small
      if (Math.abs(dragVelocity.current) < 0.01) {
        dragVelocity.current = 0
      }

      return
    }

    // Update hover factor
    if (isHovered.current) {
      hoverFactorValue.set(slowdownOnHover ? slowDownFactor : 1)
    } else {
      hoverFactorValue.set(1)
    }

    // Calculate regular movement
    let moveBy =
      directionFactor.current *
      actualBaseVelocity *
      (delta / 1000) *
      smoothHoverFactor.get()

    // Adjust movement based on scroll velocity if scrollAwareDirection is enabled
    if (scrollAwareDirection && !isDragging.current) {
      if (velocityFactor.get() < 0) {
        directionFactor.current = -1
      } else if (velocityFactor.get() > 0) {
        directionFactor.current = 1
      }
    }

    moveBy += directionFactor.current * moveBy * velocityFactor.get()

    if (draggable) {
      moveBy += dragVelocity.current

      // Update direction based on drag direction if dragAwareDirection is true
      if (dragAwareDirection && Math.abs(dragVelocity.current) > 0.1) {
        // If dragging in negative direction, set directionFactor to -1
        // If dragging in positive direction, set directionFactor to 1
        directionFactor.current = Math.sign(dragVelocity.current)
      }

      // Gradually decay drag velocity back to zero
      if (!isDragging.current && Math.abs(dragVelocity.current) > 0.01) {
        dragVelocity.current *= dragVelocityDecay
      } else if (!isDragging.current) {
        dragVelocity.current = 0
      }
    }

    if (isHorizontal) {
      baseX.set(baseX.get() + moveBy)
    } else {
      baseY.set(baseY.get() + moveBy)
    }
  })

  const lastPointerPosition = useRef({ x: 0, y: 0 })

  const handlePointerDown = (e: React.PointerEvent) => {
    if (!draggable)
      return // Capture the pointer to receive events even when pointer moves outside
    ;(e.currentTarget as HTMLElement).setPointerCapture(e.pointerId)

    if (grabCursor) {
      ;(e.currentTarget as HTMLElement).style.cursor = "grabbing"
    }

    isDragging.current = true
    lastPointerPosition.current = { x: e.clientX, y: e.clientY }

    // Pause automatic animation by setting velocity to 0
    dragVelocity.current = 0
  }

  const handlePointerMove = (e: React.PointerEvent) => {
    if (!draggable || !isDragging.current) return

    const currentPosition = { x: e.clientX, y: e.clientY }

    // Calculate delta from last position
    const deltaX = currentPosition.x - lastPointerPosition.current.x
    const deltaY = currentPosition.y - lastPointerPosition.current.y

    // Convert dragAngle from degrees to radians
    const angleInRadians = (dragAngle * Math.PI) / 180

    // Calculate the projection of the movement along the angle direction
    // Using the dot product of the movement vector and the direction vector
    const directionX = Math.cos(angleInRadians)
    const directionY = Math.sin(angleInRadians)

    // Project the movement onto the angle direction
    const projectedDelta = deltaX * directionX + deltaY * directionY

    // Update drag velocity based on the projected movement
    dragVelocity.current = projectedDelta * dragSensitivity

    // Update last position
    lastPointerPosition.current = currentPosition
  }

  const handlePointerUp = (e: React.PointerEvent) => {
    if (!draggable) return // Release pointer capture
    ;(e.currentTarget as HTMLElement).releasePointerCapture(e.pointerId)

    isDragging.current = false
  }

  return (
    <motion.div
      className={cn("flex", isHorizontal ? "flex-row" : "flex-col", className)}
      onHoverStart={() => (isHovered.current = true)}
      onHoverEnd={() => (isHovered.current = false)}
      onPointerDown={handlePointerDown}
      onPointerMove={handlePointerMove}
      onPointerUp={handlePointerUp}
      onPointerCancel={handlePointerUp}
    >
      {Array.from({ length: repeat }, (_, i) => i).map((i) => (
        <motion.div
          key={i}
          className={cn(
            "shrink-0",
            isHorizontal && "flex",
            draggable && grabCursor && "cursor-grab"
          )}
          style={isHorizontal ? { x } : { y }}
          aria-hidden={i > 0}
        >
          {children}
        </motion.div>
      ))}
    </motion.div>
  )
}

export default SimpleMarquee
```





## Usage

You only need to wrap your elements with the `SimpleMarquee` component, everything else is taken care of by the component itself.

## Understanding the component

Unlike most marquee implementations that use simple CSS animations, this component uses Motion's `useAnimationFrame` hook to provide more control over the animation. This allows for a bunch of fancy effects, such as:

- Changing velocity and direction by dragging
- Adjusting speed in response to scrolling
- Adding custom easing functions
- Creating pause/slow on hover effects

### Core Animation

The main magic of this component is the `useAnimationFrame` hook from Motion, which executes our anim code on every frame. Here's how it works:

1. We create motion values (using `useMotionValue`) to track the x or y position:


**File**: `Motion values`
```tsx
const baseX = useMotionValue(0)
const baseY = useMotionValue(0)
```


2. We define a `baseVelocity` prop that determines the default speed and direction:


**File**: `Base velocity`
```tsx
  // Convert baseVelocity to the correct direction
  const actualBaseVelocity =
    direction === "left" || direction === "up" ? -baseVelocity : baseVelocity
```


3. On each animation frame inside the `useAnimationFrame` hook, we increment the position values, by adding that velocity to the current position:


**File**: `Animation frame`
```tsx
// Inside useAnimationFrame
let moveBy = directionFactor.current * baseVelocity * (delta / 1000)

if (isHorizontal) {
  baseX.set(baseX.get() + moveBy)
} else {
  baseY.set(baseY.get() + moveBy)
}
```


4. Since we're constantly increasing/decreasing that value, at some point our elements would move out far away from the viewport. Therefore, we use the `useTransform` hook to convert that x/y value to a percentage, and wrapping it between 0 and -100. With this, we essentially force our elements to always move from 0 to -100. Once they reach -100, they will start their journey from 0% again.


**File**: `Transformation`
```tsx
const x = useTransform(baseX, (v) => {
  // wrap it between 0 and -100
  const wrappedValue = wrap(0, -100, v)
  // Apply easing if provided, otherwise use linear
  return `${easing ? easing(wrappedValue / -100) * -100 : wrappedValue}%`
})
```


5. The `wrap` helper function ensures values stay between 0 and -100:


**File**: `Wrapping`
```tsx
const wrap = (min: number, max: number, value: number): number => {
  const range = max - min
  return ((((value - min) % range) + range) % range) + min
}
```


This example demonstrates the basic mechanism:


### Demo Code (simple-marquee-explainer-demo.tsx)
```tsx
import { useState } from "react"

import { Button } from "@/components/ui/button"
import SimpleMarquee from "@/fancy/components/blocks/simple-marquee"

const MarqueeItem = ({ index }: { index: number }) => (
  <div >
    
      ITEM {index.toString().padStart(2, "0")}
    
    
      fancy
    
  </div>
)

export default function SimpleMarqueeExplainerDemo() {
  const [repeat, setRepeat] = useState(1)

  return (
    <div >
      <Button
        variant={"outline"}
        size={"sm"}
        
        onClick={() => setRepeat((prev) => (prev < 5 ? prev + 1 : 1))}
      >
        Repeat: {repeat}
      </Button>

      <div >
        <SimpleMarquee baseVelocity={30} repeat={repeat} direction="left">
          <MarqueeItem index={1} />
          <MarqueeItem index={2} />
        </SimpleMarquee>
      </div>
    </div>
  )
}
```


### Preventing "Jumps" With Repetition

As you can see above, elements eventually leave the container and jump back to the beginning when they reach -100%. This creates a visible "jump" in the animation.

We can solve this by using the `repeat` prop to duplicate all child elements multiple times inside the component:


**File**: `Repeat example`
```tsx
{
  Array.from({ length: repeat }, (_, i) => i).map((i) => (
    <motion.div
      key={i}
      className={cn(
        "shrink-0",
        isHorizontal && "flex",
        draggable && grabCursor && "cursor-grab"
      )}
      style={isHorizontal ? { x } : { y }}
      aria-hidden={i > 0}
    >
      {children}
    </motion.div>
  ))
}
```


By default, the `repeat` value is 3, which means your content is duplicated three times. With enough repetitions, new elements enter the visible area before existing ones leave, creating an illusion of continuous animation. Try increasing the `repeat` value in the demo above to see how it eliminates the jumpiness.

## Features

The marquee's final velocity and behavior are determined by combining several factors that can be enabled through props:

### Slow Down On Hover

When `slowdownOnHover` is set to `true`, the component tracks hover state and applies a slowdown factor:


**File**: `Slow down on hover`
```tsx
// Track hover state
const isHovered = useRef(false)
const hoverFactorValue = useMotionValue(1)
const smoothHoverFactor = useSpring(hoverFactorValue, slowDownSpringConfig)

// In component JSX
<motion.div
  onHoverStart={() => (isHovered.current = true)}
  onHoverEnd={() => (isHovered.current = false)}
  // ...other props
>
  {/* ... */}
</motion.div>

// In animation frame
if (isHovered.current) {
  hoverFactorValue.set(slowdownOnHover ? slowDownFactor : 1)
} else {
  hoverFactorValue.set(1)
}

// Apply the hover factor to movement calculation
let moveBy = directionFactor.current *
             actualBaseVelocity *
             (delta / 1000) *
             smoothHoverFactor.get()
```


Key props for this feature:

- `slowDownFactor` controls how much to slow down (default: 0.3 or 30% of original speed)
- `smoothHoverFactor` uses spring physics for smooth transitions between speeds. This ensures that the velocity change is not happening instantly, but with a smooth animation. For this, we use the `useSpring` hook from Motion. 
- `slowDownSpringConfig` lets you customize the spring animation parameters. Please refer to the [Motion documentation](https://motion.dev/docs/react-use-spring) for more details.

### Scroll-Based Velocity

When `useScrollVelocity` is enabled, the component tracks scroll velocity and uses it to influence the final velocity of the marquee:


**File**: `Scroll velocity`
```tsx
const { scrollY } = useScroll({
  container: (scrollContainer as RefObject<HTMLDivElement>) || innerContainer.current,
})
const scrollVelocity = useVelocity(scrollY)
const smoothVelocity = useSpring(scrollVelocity, scrollSpringConfig)

// Transform scroll velocity into a factor for marquee speed
const velocityFactor = useTransform(
  useScrollVelocity ? smoothVelocity : defaultVelocity,
  [0, 1000],
  [0, 5],
  { clamp: false }
)

// In animation frame
// Adjust movement based on scroll velocity
moveBy += directionFactor.current * moveBy * velocityFactor.get()

// Change direction based on scroll if enabled
if (scrollAwareDirection && !isDragging.current) {
  if (velocityFactor.get() < 0) {
    directionFactor.current = -1
  } else if (velocityFactor.get() > 0) {
    directionFactor.current = 1
  }
}
```


This creates an interactive effect where:

- Scrolling adds to the marquee's velocity
- If `scrollAwareDirection` is enabled, the scroll direction can reverse the marquee direction
- Similar to the hover, we interpolate between the current and scroll velocity by using Spring physics with the `useSpring` hook from Motion. You can customize the spring animation parameters using the `scrollSpringConfig` prop.

### Custom Easing Functions

The `easing` prop allows you to transform the linear animation with custom easing curves:


**File**: `Custom easing`
```tsx
const x = useTransform(baseX, (v) => {
  // Apply easing if provided, otherwise use linear
  const wrappedValue = wrap(0, -100, v)
  return `${easing ? easing(wrappedValue / -100) * -100 : wrappedValue}%`
})
```


The easing function receives a normalized value between 0 and 1 and should return a transformed value. You need to provide an actual function here, not defined keyframes.

You can find ready-to-use easing functions at [easings.net](https://easings.net/).


### Demo Code (simple-marquee-easing-demo.tsx)
```tsx
import React, { useRef } from "react"

import SimpleMarquee from "@/fancy/components/blocks/simple-marquee"

const exampleImages = [
  "https://cdn.cosmos.so/4b771c5c-d1eb-4948-b839-255dbeb931ba?format=jpeg",
  "https://cdn.cosmos.so/a8d82afd-2293-43ad-bac3-887683d85b44?format=jpeg",
  "https://cdn.cosmos.so/49206ba5-c174-4cd5-aee8-5b744842e6c2?format=jpeg",
  "https://cdn.cosmos.so/b29bd150-6477-420f-8efb-65ed99694421?format=jpeg",
  "https://cdn.cosmos.so/e1a0313e-7617-431d-b7f1-f1b169e6bcb4?format=jpeg",
  "https://cdn.cosmos.so/ad640c12-69fb-4186-bc3d-b1cc93986a37?format=jpeg",
  "https://cdn.cosmos.so/5cf0c3d2-e785-41a3-b0c8-a073ee2f2862?format=jpeg",
  "https://cdn.cosmos.so/938ab21c-a975-41b3-b303-418290343b09?format=jpeg",
  "https://cdn.cosmos.so/2e14a9bb-27e3-40fd-b940-cfb797a1224c?format=jpeg",
  "https://cdn.cosmos.so/81841d9f-e164-4770-aebc-cfc97d72f3ab?format=jpeg",
  "https://cdn.cosmos.so/49b81db0-37ea-4569-b0d6-04afa5115a10?format=jpeg",
  "https://cdn.cosmos.so/ade1834b-9317-44fb-8dc3-b43d29acd409?format=jpeg",
  "https://cdn.cosmos.so/621c250c-3833-45f9-862a-3f400aaf8f28?format=jpeg",
  "https://cdn.cosmos.so/f9b7eae8-e5a6-4ce6-b6e1-9ef125ba7f8e?format=jpeg",
  "https://cdn.cosmos.so/bd56ed6d-1bbd-44a4-b1a1-79b7199bbebb?format=jpeg",
]

const MarqueeItem = ({ children }: { children: React.ReactNode }) => (
  <div >
    {children}
  </div>
)

export default function SimpleMarqueeDemo() {
  const firstThird = exampleImages.slice(
    0,
    Math.floor(exampleImages.length / 3)
  )
  const secondThird = exampleImages.slice(
    Math.floor(exampleImages.length / 3),
    Math.floor((2 * exampleImages.length) / 3)
  )
  const lastThird = exampleImages.slice(
    Math.floor((2 * exampleImages.length) / 3)
  )

  const containerRef = useRef<HTMLDivElement>(null)

  const easeFn = (x: number) => {
    return x === 0
      ? 0
      : x === 1
        ? 1
        : x < 0.5
          ? Math.pow(2, 20 * x - 10) / 2
          : (2 - Math.pow(2, -20 * x + 10)) / 2
  }

  return (
    <div
      
      ref={containerRef}
    >
      <div >
        {/* Just fluff for the demo */}
        <div >
          <h1 >
            Welcome Back!
          </h1>

          <div >
            <div >
              <input
                type="email"
                
                placeholder="Email"
              />
            </div>

            <div >
              <input
                type="password"
                
                placeholder="Password"
              />
            </div>

            <button >
              Sign In
            </button>

            <p >
              Don't have an account?{" "}
              <a href="#" >
                Sign up
              </a>
            </p>
          </div>
        </div>

        {/* Marquee section - this is the main content */}
        <div >
          <SimpleMarquee
            
            baseVelocity={25}
            repeat={4}
            easing={easeFn}
            direction="up"
          >
            {firstThird.map((src, i) => (
              <MarqueeItem key={i}>
                <img
                  src={src}
                  alt={`Image ${i + 1}`}
                  draggable={false}
                  
                />
              </MarqueeItem>
            ))}
          </SimpleMarquee>

          <SimpleMarquee
            
            baseVelocity={25}
            repeat={4}
            easing={easeFn}
            direction="down"
          >
            {secondThird.map((src, i) => (
              <MarqueeItem key={i}>
                <img
                  src={src}
                  draggable={false}
                  alt={`Image ${i + firstThird.length}`}
                  
                />
              </MarqueeItem>
            ))}
          </SimpleMarquee>

          <SimpleMarquee
            
            baseVelocity={25}
            repeat={4}
            easing={easeFn}
            direction="up"
          >
            {lastThird.map((src, i) => (
              <MarqueeItem key={i}>
                <img
                  src={src}
                  draggable={false}
                  alt={`Image ${i + firstThird.length + secondThird.length}`}
                  
                />
              </MarqueeItem>
            ))}
          </SimpleMarquee>
        </div>
      </div>
    </div>
  )
}
```


### Draggable Marquee

The marquee can also be dragged. It uses pointer events for tracking the cursor position and applying the drag velocity:


**File**: `Dragging`
```tsx
// State for tracking dragging
const isDragging = useRef(false)
const dragVelocity = useRef(0)
const lastPointerPosition = useRef({ x: 0, y: 0 })

const handlePointerDown = (e: React.PointerEvent) => {
  if (!draggable) return
  // Capture pointer events
  (e.currentTarget as HTMLElement).setPointerCapture(e.pointerId)

  if (grabCursor) {
    (e.currentTarget as HTMLElement).style.cursor = "grabbing"
  }

  isDragging.current = true
  lastPointerPosition.current = { x: e.clientX, y: e.clientY }

  // Pause automatic animation
  dragVelocity.current = 0
}

const handlePointerMove = (e: React.PointerEvent) => {
  if (!draggable || !isDragging.current) return

  const currentPosition = { x: e.clientX, y: e.clientY }

  // Calculate movement delta
  const deltaX = currentPosition.x - lastPointerPosition.current.x
  const deltaY = currentPosition.y - lastPointerPosition.current.y

  // Support for angled dragging
  const angleInRadians = (dragAngle * Math.PI) / 180
  const directionX = Math.cos(angleInRadians)
  const directionY = Math.sin(angleInRadians)

  // Project movement along angle direction
  const projectedDelta = deltaX * directionX + deltaY * directionY

  // Set drag velocity
  dragVelocity.current = projectedDelta * dragSensitivity

  lastPointerPosition.current = currentPosition
}
```


During animation frames, dragging takes precedence over other movement factors. Meaning, when the user is dragging, the marquee will move according to the drag velocity, and we ignore all other factors (such as the hover, scroll and the basic velocity).


**File**: `Drag animation frame`
```tsx
// Inside useAnimationFrame
if (isDragging.current && draggable) {
  if (isHorizontal) {
    baseX.set(baseX.get() + dragVelocity.current)
  } else {
    baseY.set(baseY.get() + dragVelocity.current)
  }

  // Add decay to dragVelocity when not moving
  dragVelocity.current *= 0.9

  // Stop completely if velocity is very small
  if (Math.abs(dragVelocity.current) < 0.01) {
    dragVelocity.current = 0
  }

  return
}
```


When the user stops dragging, velocity gradually decays back to the base velocity. You can customize the decay factor using the `dragVelocityDecay` prop.


**File**: `Drag velocity decay`
```tsx
// Gradually decay drag velocity back to zero
if (!isDragging.current && Math.abs(dragVelocity.current) > 0.01) {
  dragVelocity.current *= dragVelocityDecay
} else if (!isDragging.current) {
  dragVelocity.current = 0
}
```


The component also supports changing direction based on drag movement:


**File**: `Drag direction`
```tsx
// Update direction based on drag direction
if (dragAwareDirection && Math.abs(dragVelocity.current) > 0.1) {
  // If dragging in negative direction, set directionFactor to -1
  // If dragging in positive direction, set directionFactor to 1
  directionFactor.current = Math.sign(dragVelocity.current)
}
```



### Demo Code (simple-marquee-drag-demo.tsx)
```tsx
import React, { useRef } from "react"
import { motion } from "motion/react"

import SimpleMarquee from "@/fancy/components/blocks/simple-marquee"
import VerticalCutReveal from "@/fancy/components/text/vertical-cut-reveal"

const imgs = [
  "https://cdn.cosmos.so/97fd931c-28cc-480f-91a8-cffb635cf832?format=jpeg",
  "https://cdn.cosmos.so/305a25f2-cc53-4ff3-95a5-6a5ca1517ff8?format=jpeg",
  "https://cdn.cosmos.so/2a024234-6713-41b2-a2f2-1d5e385ac490?format=jpeg",
  "https://cdn.cosmos.so/89cc65c1-b0bf-42f6-9afc-4db6678ae652?format=jpeg",
  "https://cdn.cosmos.so/211e0ca7-4126-4222-9de8-03aeb1e4688e?format=jpeg",
  "https://cdn.cosmos.so/b7dc0ec1-4b03-42ce-9805-1964d0f49feb?format=jpeg",
  "https://cdn.cosmos.so/43be3f32-bd6e-4fd1-93c8-d54e0d8196ee?format=jpeg",
  "https://cdn.cosmos.so/d0d146aa-b49c-48be-8b09-6b7eaf8e836d?format=jpeg",
  "https://cdn.cosmos.so/e765d51f-8be7-4618-83e2-90c13379b366?format=jpeg",
  "https://cdn.cosmos.so/c1854fe0-e974-4cb6-8bc4-ffcf1686b9e7?format=jpeg",
]

const firstRow = imgs.slice(0, 5)
const secondRow = imgs.slice(5)

const MarqueeItem = ({
  children,
  index,
}: {
  children: React.ReactNode
  index: number
}) => (
  <motion.div
    
    initial={{ opacity: 0, y: 0, filter: "blur(10px)" }}
    animate={{ opacity: 1, y: 0, filter: "blur(0px)" }}
    transition={{ duration: 0.5, ease: "easeOut", delay: 0.3 + 0.1 * index }}
    style={{
      transform: `translateZ(-150px) rotate(${index * 15}deg)`,
      transformStyle: "preserve-3d",
    }}
  >
    {children}
  </motion.div>
)

export default function SimpleMarqueeDemo() {
  const container = useRef<HTMLDivElement>(null)

  return (
    <div
      
      ref={container}
    >
      <h1 >
        <VerticalCutReveal splitBy="characters" staggerDuration={0.04}>
          New Arrivals
        </VerticalCutReveal>
      </h1>
      <div >
        <SimpleMarquee
          
          baseVelocity={8}
          repeat={2}
          draggable={true}
          dragSensitivity={0.08}
          useScrollVelocity={true}
          scrollAwareDirection={true}
          scrollSpringConfig={{ damping: 50, stiffness: 400 }}
          scrollContainer={container}
          dragAwareDirection={true}
          grabCursor
          direction="left"
        >
          {firstRow.map((src, i) => (
            <MarqueeItem key={i} index={i}>
              <motion.img
                src={src}
                alt={`Image ${i + 1}`}
                draggable={false}
                
              />
            </MarqueeItem>
          ))}
        </SimpleMarquee>

        <SimpleMarquee
          
          baseVelocity={8}
          repeat={2}
          draggable={true}
          dragSensitivity={0.08}
          useScrollVelocity={true}
          scrollAwareDirection={true}
          scrollSpringConfig={{ damping: 50, stiffness: 400 }}
          scrollContainer={container}
          dragAwareDirection={true}
          grabCursor
          direction="right"
        >
          {secondRow.map((src, i) => (
            <MarqueeItem key={i} index={i}>
              <motion.img
                src={src}
                alt={`Image ${i + 6}`}
                draggable={false}
                
              />
            </MarqueeItem>
          ))}
        </SimpleMarquee>
      </div>
    </div>
  );
}
```


Artwork credits: Artworks are from [Cosmos](https://cosmos.so/). I couldn't track down the original artists.

## 3D Transforms

To make a 3d effect, you can apply 3D CSS transforms to the marquee container or its children. The following example shows how you can apply them on the container.


### Demo Code (simple-marquee-3d-demo.tsx)
```tsx
import React, { useEffect, useRef, useState } from "react"
import { motion, Variants } from "motion/react"

import { cn } from "@/lib/utils"
import SimpleMarquee from "@/fancy/components/blocks/simple-marquee"

// Interface for album data
interface Album {
  coverArt: string
  title: string
  artist: string
}

const hardcodedAlbums: Album[] = [
  {
    coverArt:
      "https://ia600207.us.archive.org/28/items/mbid-770b9b80-10e1-4297-b1fd-46ad0dbb0305/mbid-770b9b80-10e1-4297-b1fd-46ad0dbb0305-1148987477_thumb500.jpg",
    title: "Homework",
    artist: "Daft Punk",
  },
  {
    coverArt:
      "https://ia800905.us.archive.org/5/items/mbid-9da1a863-f3f2-4618-bdce-f0c88c055ba5/mbid-9da1a863-f3f2-4618-bdce-f0c88c055ba5-8201721911_thumb500.jpg",
    title: "✝",
    artist: "Justice",
  },
  {
    coverArt:
      "https://ia800909.us.archive.org/12/items/mbid-ee618541-23df-4973-afb7-e2d9f02e03d8/mbid-ee618541-23df-4973-afb7-e2d9f02e03d8-8154031977_thumb500.jpg",
    title: "By Your Side",
    artist: "Breakbot",
  },
  {
    coverArt:
      "https://ia800804.us.archive.org/20/items/mbid-3adfe4c6-0fa2-4813-a212-058d9a99b4a8/mbid-3adfe4c6-0fa2-4813-a212-058d9a99b4a8-16639897570_thumb500.jpg",
    title: "Still Waters",
    artist: "Breakbot",
  },
  {
    coverArt:
      "https://ia803403.us.archive.org/14/items/mbid-a7fcead9-ab9d-3d15-bb0d-a2b1945517dd/mbid-a7fcead9-ab9d-3d15-bb0d-a2b1945517dd-8093147470_thumb500.jpg",
    title: "Fancy Footwork",
    artist: "Chromeo",
  },
  {
    coverArt:
      "https://ia801301.us.archive.org/18/items/mbid-8acb4d6d-2cf9-4685-b4e8-5c9937621691/mbid-8acb4d6d-2cf9-4685-b4e8-5c9937621691-5651042668_thumb500.jpg",
    title: "Trax on da Rocks Vol. 2",
    artist: "Thomas Bangalter",
  },
  {
    coverArt:
      "https://ia904509.us.archive.org/32/items/mbid-cb844a4d-c02f-3199-b949-1656b36722da/mbid-cb844a4d-c02f-3199-b949-1656b36722da-8145217760_thumb500.jpg",
    title: "1999",
    artist: "Cassius",
  },
  {
    coverArt:
      "https://ia903201.us.archive.org/6/items/mbid-747ed90c-6479-4cec-a98a-b320a5ef75be/mbid-747ed90c-6479-4cec-a98a-b320a5ef75be-18417637214_thumb500.jpg",
    title: "Woman",
    artist: "Justice",
  },
  {
    coverArt:
      "https://ia800200.us.archive.org/5/items/mbid-9d0a791d-c0ed-4b99-bb31-976fad672408/mbid-9d0a791d-c0ed-4b99-bb31-976fad672408-1959533822_thumb500.jpg",
    title: "Modjo",
    artist: "Modjo",
  },
  {
    coverArt:
      "https://ia903106.us.archive.org/23/items/mbid-bbfc83ad-826f-4957-893d-a808105c828b/mbid-bbfc83ad-826f-4957-893d-a808105c828b-25063975521_thumb500.jpg",
    title: "Random Access Memories",
    artist: "Daft Punk",
  },
]

export default function SimpleMarqueeDemo() {
  const [albums, setAlbums] = useState<Album[]>([])
  const [loading, setLoading] = useState(true)
  const container = useRef<HTMLDivElement>(null)

  useEffect(() => {
    // Simulate loading time
    const timer = setTimeout(() => {
      setAlbums(hardcodedAlbums)
      setLoading(false)
    }, 1000)

    return () => clearTimeout(timer)
  }, [])

  const firstRow = albums.slice(0, Math.floor(albums.length / 2))
  const secondRow = albums.slice(Math.floor(albums.length / 2))

  const MarqueeItem = ({ album, index }: { album: Album; index: number }) => {
    const variants = {
      initial: {
        y: "0px",
        x: "0px",
        scale: 1,
        opacity: 1,
      },
      hover: {
        y: "-12px",
        x: "-12px",
        scale: 1.05,
        transition: {
          duration: 0.15,
          ease: "easeOut",
        },
      },
    }

    const textVariants = {
      initial: {
        opacity: 0,
      },
      hover: {
        opacity: 1,
        transition: {
          duration: 0.15,
          ease: "easeOut",
        },
      },
    }

    const imageVariants = {
      initial: {
        opacity: 1,
      },
      hover: {
        opacity: 0.45,
        transition: {
          duration: 0.15,
          ease: "easeOut",
        },
      },
    }

    const containerClasses = cn(
      "mx-2 sm:mx-3 md:mx-4 cursor-pointer",
      "h-32 w-32 sm:h-40 sm:w-40 md:h-48 md:w-48",
      "relative flex shadow-white/20 shadow-md",
      "overflow-hidden flex-col transform-gpu bg-black"
    )

    const textContainerClasses = cn(
      "justify-end p-2 sm:p-2.5 md:p-3 h-full flex items-start flex-col",
      "leading-tight"
    )

    const imageClasses = cn("object-cover w-full h-full shadow-2xl absolute")

    return (
      <motion.div
        className={containerClasses}
        initial="initial"
        whileHover="hover"
        variants={variants as Variants}
      >
        <motion.div className={textContainerClasses} variants={textVariants as Variants}>
          <h3 >
            {album.title}
          </h3>
          <p >
            {album.artist}
          </p>
        </motion.div>
        <motion.img
          src={album.coverArt}
          alt={`${album.title} by ${album.artist}`}
          draggable={false}
          className={imageClasses}
          variants={imageVariants as Variants}
        />
      </motion.div>
    )
  }

  return (
    <div
      
      ref={container}
    >
      <h1 >
        Weekly Mix
      </h1>
      {loading ? (
        <div >Loading album covers...</div>
      ) : (
        <>
          <div
            
            style={{
              transform:
                "rotateX(45deg) rotateY(-15deg) rotateZ(35deg) translateZ(-200px)",
            }}
          >
            <SimpleMarquee
              
              baseVelocity={10}
              repeat={3}
              draggable={false}
              scrollSpringConfig={{ damping: 50, stiffness: 400 }}
              slowDownFactor={0.2}
              slowdownOnHover
              slowDownSpringConfig={{ damping: 60, stiffness: 300 }}
              scrollAwareDirection={true}
              scrollContainer={container}
              useScrollVelocity={true}
              direction="left"
            >
              {firstRow.map((album, i) => (
                <MarqueeItem key={i} index={i} album={album} />
              ))}
            </SimpleMarquee>

            <SimpleMarquee
              
              baseVelocity={10}
              repeat={3}
              scrollAwareDirection={true}
              scrollSpringConfig={{ damping: 50, stiffness: 400 }}
              slowdownOnHover
              slowDownFactor={0.2}
              slowDownSpringConfig={{ damping: 60, stiffness: 300 }}
              useScrollVelocity={true}
              scrollContainer={container}
              draggable={false}
              direction="right"
            >
              {secondRow.map((album, i) => (
                <MarqueeItem key={i} index={i} album={album} />
              ))}
            </SimpleMarquee>
          </div>
        </>
      )}
    </div>
  );
}
```


For angled marquees, you can also apply the `dragAngle` prop to change the direction of the drag movement. This is useful if you want to rotate the marquee e.g. by 45 degrees.


**File**: `3D transforms`
```tsx
// Convert dragAngle from degrees to radians
const angleInRadians = (dragAngle * Math.PI) / 180

// Calculate the projection of the movement along the angle direction
const directionX = Math.cos(angleInRadians)
const directionY = Math.sin(angleInRadians)

// Project the movement onto the angle direction
const projectedDelta = deltaX * directionX + deltaY * directionY
```


## Resources

- [Scroll animations from Motion](https://motion.dev/docs/react-scroll-animations)
- [Easings](https://easings.net/)
- [CSS Only implementation from Frontend FYI](https://www.youtube.com/watch?v=uw5jVO1eNF8)
- [Gradient artworks](https://www.cosmos.so/danielpetho/gradients)
- [Album covers](https://musicbrainz.org/doc/Cover_Art_Archive/API)

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `ReactNode` | - | The elements to be scrolled |
| className | `string` | - | Additional CSS classes for the container |
| direction | `"left" | "right" | "up" | "down"` | `right` | The direction of the marquee. Set to `"left"` or `"right"` to scroll         from left to right, or `"up"` or `"down"` to scroll from top to bottom |
| baseVelocity | `number` | `5` | The base velocity of the marquee in pixels per second |
| easing | `(value: number) => number` | - | The easing function for the animation |
| slowdownOnHover | `boolean` | `false` | Whether to slow down the animation on hover |
| slowDownFactor | `number` | `0.3` | The factor to slow down the animation on hover |
| slowDownSpringConfig | `SpringOptions` | `{ damping: 50, stiffness: 400 }` | The spring config for the slow down animation |
| useScrollVelocity | `boolean` | `false` | Whether to use the scroll velocity to control the marquee speed |
| scrollAwareDirection | `boolean` | `false` | Whether to adjust the direction based on the scroll direction |
| scrollSpringConfig | `SpringOptions` | `{ damping: 50, stiffness: 400 }` | The spring config for the scroll velocity-based direction adjustment |
| scrollContainer | `RefObject | HTMLElement | null` | - | The container to use for the scroll velocity. If not provided, the window will be used. |
| repeat | `number` | `3` | The number of times to repeat the children |
| draggable | `boolean` | `false` | Whether to allow dragging of the marquee |
| dragSensitivity | `number` | `0.2` | The sensitivity of the drag movement |
| dragVelocityDecay | `number` | `0.96` | The decay of the drag velocity when released |
| dragAwareDirection | `boolean` | `false` | Whether to adjust the direction based on the drag velocity |
| dragAngle | `number` | `0` | The angle of the drag movement in degrees |
| grabCursor | `boolean` | `false` | Whether to change the cursor to grabbing when dragging |


---

### Marquee Along SVG Path <a name="marquee-along-svg-path"></a>

### Demo Code (marquee-along-svg-path-demo.tsx)
```tsx
import MarqueeAlongSvgPath from "@/fancy/components/blocks/marquee-along-svg-path"

const path =
  "M1 209.434C58.5872 255.935 387.926 325.938 482.583 209.434C600.905 63.8051 525.516 -43.2211 427.332 19.9613C329.149 83.1436 352.902 242.723 515.041 267.302C644.752 286.966 943.56 181.94 995 156.5"

export default function MarqueeAlongSvgPathDemo() {
  return (
    <div >
      <MarqueeAlongSvgPath
        path={path}
        viewBox="0 0 996 330"
        baseVelocity={8}
        slowdownOnHover={true}
        draggable={true}
        repeat={2}
        dragSensitivity={0.1}
        
        responsive
        grabCursor
      >
        {imgs.map((img, i) => (
          <div
            key={i}
            
          >
            <img
              src={img.src}
              alt={`Example ${i}`}
              
              draggable={false}
            />
          </div>
        ))}
      </MarqueeAlongSvgPath>
    </div>
  )
}

const imgs = [
  {
    src: "https://cdn.cosmos.so/b9909337-7a53-48bc-9672-33fbd0f040a1?format=jpeg",
    link: "https://www.instagram.com/p/DCOl6YTS85e/?igsh=MXNvdHhyczl1djJ6ZA%3D%3D",
  },
  {
    src: "https://cdn.cosmos.so/ecdc9dd7-2862-4c28-abb1-dcc0947390f3?format=jpeg",
    link: "https://www.instagram.com/p/C4RTJvVpP4R/?igsh=MWZwOTNlYTVodGszMw%3D%3D",
  },
  {
    src: "https://cdn.cosmos.so/79de41ec-baa4-4ac0-a9a4-c090005ca640?format=jpeg",
    link: "https://pangrampangram.com/products/mori",
  },
  {
    src: "https://cdn.cosmos.so/1a18b312-21cd-4484-bce5-9fb7ed1c5e01?format=jpeg",
    link: "https://www.sergidelgado.com/selected-work/ampersand",
  },
  {
    src: "https://cdn.cosmos.so/d765f64f-7a66-462f-8b2d-3d7bc8d7db55?format=jpeg",
    link: "https://www.instagram.com/p/C40XmANsoe_/?igsh=MXFlZGx4cmw3ZW1qYw%3D%3D",
  },
  {
    src: "https://cdn.cosmos.so/6b9f08ea-f0c5-471f-a620-71221ff1fb65?format=jpeg",
    link: "https://abduzeedo.com/super-stylish-type-explorations",
  },
  {
    src: "https://cdn.cosmos.so/40a09525-4b00-4666-86f0-3c45f5d77605?format=jpeg",
    link: "https://www.instagram.com/p/CrhdrGjr9yK/?igshid=MTc4MmM1YmI2Ng%3D%3D",
  },
  {
    src: "https://cdn.cosmos.so/14f05ab6-b4d0-4605-9007-8a2190a249d0?format=jpeg",
    link: "https://www.instagram.com/julian.stiber/p/By5RBApiDzE/?img_index=1",
  },
  {
    src: "https://cdn.cosmos.so/d05009a2-a2f8-4a4c-a0de-e1b0379dddb8?format=jpeg",
    link: "https://www.instagram.com/p/CeT3COysRNN/?img_index=2",
  },
  {
    src: "https://cdn.cosmos.so/ba646e35-efc2-494a-961b-b40f597e6fc9?format=jpeg",
    link: "https://www.instagram.com/godfreydadich/",
  },
  {
    src: "https://cdn.cosmos.so/e899f9c3-ed48-4899-8c16-fbd5a60705da?format=jpeg",
    link: "https://www.instagram.com/p/Bty1U6BhTOW/?img_index=5",
  },
  {
    src: "https://cdn.cosmos.so/24e83c11-c607-45cd-88fb-5059960b56a0?format=jpeg",
    link: "https://www.instagram.com/p/C48dxn1LqhC/?igsh=dmV5ZWR0Z2Y3Zzlt&img_index=3",
  },
  {
    src: "https://cdn.cosmos.so/cd346bce-f415-4ea7-8060-99c5f7c1741a?format=jpeg",
    link: "https://www.instagram.com/p/C08ZDVyyRhK/?img_index=2&igsh=bHAyZjcxYW1jZDNu",
  },
]
```


A start-to-finish tutorial on this component is available on [Codrops](https://tympanus.net/codrops/2025/06/17/building-an-infinite-marquee-along-an-svg-path-with-react-motion/?_thumbnail_id=95755).

## Installation




  CLI
  Manual




```bash
# Install command:
npx shadcn add @fancy/marquee-along-svg-path
```






### Component Source Code (marquee-along-svg-path.tsx)
```tsx
import React, { RefObject, useCallback, useEffect, useRef } from "react"
import {
  motion,
  SpringOptions,
  useAnimationFrame,
  useMotionValue,
  useScroll,
  useSpring,
  useTransform,
  useVelocity,
} from "motion/react"

import { cn } from "@/lib/utils"

// Custom wrap function
const wrap = (min: number, max: number, value: number): number => {
  const range = max - min
  return ((((value - min) % range) + range) % range) + min
}

type PreserveAspectRatioAlign =
  | "none"
  | "xMinYMin"
  | "xMidYMin"
  | "xMaxYMin"
  | "xMinYMid"
  | "xMidYMid"
  | "xMaxYMid"
  | "xMinYMax"
  | "xMidYMax"
  | "xMaxYMax"

interface CSSVariableInterpolation {
  property: string
  from: number | string
  to: number | string
}

type PreserveAspectRatioMeetOrSlice = "meet" | "slice"

type PreserveAspectRatio =
  | PreserveAspectRatioAlign
  | `${Exclude<PreserveAspectRatioAlign, "none">} ${PreserveAspectRatioMeetOrSlice}`

interface MarqueeAlongSvgPathProps {
  children: React.ReactNode
  className?: string

  // Path properties
  path: string
  pathId?: string
  preserveAspectRatio?: PreserveAspectRatio
  showPath?: boolean

  // SVG properties
  width?: string | number
  height?: string | number
  viewBox?: string

  // Marquee properties
  baseVelocity?: number
  direction?: "normal" | "reverse"
  easing?: (value: number) => number
  slowdownOnHover?: boolean
  slowDownFactor?: number
  slowDownSpringConfig?: SpringOptions

  // Scroll properties
  useScrollVelocity?: boolean
  scrollAwareDirection?: boolean
  scrollSpringConfig?: SpringOptions
  scrollContainer?: RefObject<HTMLElement | null> | HTMLElement | null

  // Item repetition
  repeat?: number

  // Drag properties
  draggable?: boolean
  dragSensitivity?: number
  dragVelocityDecay?: number
  dragAwareDirection?: boolean
  grabCursor?: boolean

  // Z-index properties
  enableRollingZIndex?: boolean
  zIndexBase?: number
  zIndexRange?: number

  cssVariableInterpolation?: CSSVariableInterpolation[]

  // Responsive properties
  responsive?: boolean
}

const MarqueeAlongSvgPath = ({
  children,
  className,

  // Path defaults
  path,
  pathId,
  preserveAspectRatio = "xMidYMid meet",
  showPath = false,

  // SVG defaults
  width = "100%",
  height = "100%",
  viewBox = "0 0 100 100",

  // Marquee defaults
  baseVelocity = 5,
  direction = "normal",
  easing,
  slowdownOnHover = false,
  slowDownFactor = 0.3,
  slowDownSpringConfig = { damping: 50, stiffness: 400 },

  // Scroll defaults
  useScrollVelocity = false,
  scrollAwareDirection = false,
  scrollSpringConfig = { damping: 50, stiffness: 400 },
  scrollContainer,

  // Items repetition
  repeat = 3,

  // Drag defaults
  draggable = false,
  dragSensitivity = 0.2,
  dragVelocityDecay = 0.96,
  dragAwareDirection = false,
  grabCursor = false,

  // Z-index defaults
  enableRollingZIndex = true,
  zIndexBase = 1, // Base z-index value
  zIndexRange = 10, // Range of z-index values to use

  cssVariableInterpolation = [],

  // Responsive defaults
  responsive = false,
}: MarqueeAlongSvgPathProps) => {
  const container = useRef<HTMLDivElement>(null)
  const marqueeContainerRef = useRef<HTMLDivElement>(null)
  const baseOffset = useMotionValue(0)

  const pathRef = useRef<SVGPathElement>(null)

  const itemRefs = useRef<Map<string, HTMLDivElement>>(new Map())

  // Responsive scaling using direct DOM manipulation (no re-renders)
  useEffect(() => {
    if (!responsive) return

    const [, , vbWidth, vbHeight] = viewBox.split(" ").map(Number)
    const originalWidth = vbWidth || 100
    const originalHeight = vbHeight || 100

    const updateScale = () => {
      const wrapper = container.current
      const marqueeContainer = marqueeContainerRef.current
      if (!wrapper || !marqueeContainer) return

      const wrapperWidth = wrapper.clientWidth
      const wrapperHeight = wrapper.clientHeight

      const scaleX = wrapperWidth / originalWidth
      const scaleY = wrapperHeight / originalHeight
      const scale = Math.min(scaleX, scaleY)

      // Calculate the scaled dimensions
      const scaledWidth = originalWidth * scale
      const scaledHeight = originalHeight * scale

      // Center the marquee container within the wrapper
      const offsetX = (wrapperWidth - scaledWidth) / 2
      const offsetY = (wrapperHeight - scaledHeight) / 2

      // Set fixed dimensions on the container
      marqueeContainer.style.width = `${originalWidth}px`
      marqueeContainer.style.height = `${originalHeight}px`

      // Apply scale and position to center
      marqueeContainer.style.transform = `translate(${offsetX}px, ${offsetY}px) scale(${scale})`
      marqueeContainer.style.transformOrigin = "top left"
    }

    updateScale()
    window.addEventListener("resize", updateScale)
    return () => window.removeEventListener("resize", updateScale)
  }, [responsive, viewBox])

  // Create an array of items outside of the render function
  const items = React.useMemo(() => {
    const childrenArray = React.Children.toArray(children)

    return childrenArray.flatMap((child, childIndex) =>
      Array.from({ length: repeat }, (_, repeatIndex) => {
        const itemIndex = repeatIndex * childrenArray.length + childIndex
        const key = `${childIndex}-${repeatIndex}`
        return {
          child,
          childIndex,
          repeatIndex,
          itemIndex,
          key,
        }
      })
    )
  }, [children, repeat])

  // Function to calculate z-index based on offset distance
  const calculateZIndex = useCallback(
    (offsetDistance: number) => {
      if (!enableRollingZIndex) {
        return undefined
      }

      // Simple progress-based z-index
      const normalizedDistance = offsetDistance / 100
      return Math.floor(zIndexBase + normalizedDistance * zIndexRange)
    },
    [enableRollingZIndex, zIndexBase, zIndexRange]
  )

  // Generate a random ID for the path if not provided
  const id = pathId || `marquee-path-${Math.random().toString(36).substring(7)}`

  // Scroll tracking
  const { scrollY } = useScroll({
    container: (scrollContainer as RefObject<HTMLDivElement | null>) || container,
  })

  const scrollVelocity = useVelocity(scrollY)
  const smoothVelocity = useSpring(scrollVelocity, scrollSpringConfig)

  // Hover and drag state tracking
  const isHovered = useRef(false)
  const isDragging = useRef(false)
  const dragVelocity = useRef(0)

  // Direction factor for changing direction based on scroll or drag
  const directionFactor = useRef(direction === "normal" ? 1 : -1)

  // Motion values for animation
  const hoverFactorValue = useMotionValue(1)
  const defaultVelocity = useMotionValue(1)
  const smoothHoverFactor = useSpring(hoverFactorValue, slowDownSpringConfig)

  // Transform scroll velocity into a factor that affects marquee speed
  const velocityFactor = useTransform(
    useScrollVelocity ? smoothVelocity : defaultVelocity,
    [0, 1000],
    [0, 5],
    { clamp: false }
  )

  // Animation frame handler
  useAnimationFrame((_, delta) => {
    if (isDragging.current && draggable) {
      baseOffset.set(baseOffset.get() + dragVelocity.current)

      // Add decay to dragVelocity
      dragVelocity.current *= 0.9

      // Stop completely if velocity is very small
      if (Math.abs(dragVelocity.current) < 0.01) {
        dragVelocity.current = 0
      }

      return
    }

    // Update hover factor
    if (isHovered.current) {
      hoverFactorValue.set(slowdownOnHover ? slowDownFactor : 1)
    } else {
      hoverFactorValue.set(1)
    }

    // Calculate regular movement
    let moveBy =
      directionFactor.current *
      baseVelocity *
      (delta / 1000) *
      smoothHoverFactor.get()

    // Adjust movement based on scroll velocity if scrollAwareDirection is enabled
    if (scrollAwareDirection && !isDragging.current) {
      if (velocityFactor.get() < 0) {
        directionFactor.current = -1
      } else if (velocityFactor.get() > 0) {
        directionFactor.current = 1
      }
    }

    moveBy += directionFactor.current * moveBy * velocityFactor.get()

    if (draggable) {
      moveBy += dragVelocity.current

      // Update direction based on drag direction if dragAwareDirection is true
      if (dragAwareDirection && Math.abs(dragVelocity.current) > 0.1) {
        directionFactor.current = Math.sign(dragVelocity.current)
      }

      // Gradually decay drag velocity back to zero
      if (!isDragging.current && Math.abs(dragVelocity.current) > 0.01) {
        dragVelocity.current *= dragVelocityDecay
      } else if (!isDragging.current) {
        dragVelocity.current = 0
      }
    }

    baseOffset.set(baseOffset.get() + moveBy)
  })

  // Pointer event handlers for dragging
  const lastPointerPosition = useRef({ x: 0, y: 0 })

  const handlePointerDown = (e: React.PointerEvent) => {
    if (!draggable) return
    ;(e.currentTarget as HTMLElement).setPointerCapture(e.pointerId)

    if (grabCursor) {
      ;(e.currentTarget as HTMLElement).style.cursor = "grabbing"
    }

    isDragging.current = true
    lastPointerPosition.current = { x: e.clientX, y: e.clientY }

    // Pause automatic animation by setting velocity to 0
    dragVelocity.current = 0
  }

  const handlePointerMove = (e: React.PointerEvent) => {
    if (!draggable || !isDragging.current) return

    const currentPosition = { x: e.clientX, y: e.clientY }

    // Calculate movement delta - simplified for path movement
    const deltaX = currentPosition.x - lastPointerPosition.current.x
    const deltaY = currentPosition.y - lastPointerPosition.current.y

    // For path following, we use a simple magnitude of movement
    const delta = Math.sqrt(deltaX * deltaX + deltaY * deltaY)
    const projectedDelta = deltaX > 0 ? delta : -delta

    // Update drag velocity based on the projected movement
    dragVelocity.current = projectedDelta * dragSensitivity

    // Update last position
    lastPointerPosition.current = currentPosition
  }

  const handlePointerUp = (e: React.PointerEvent) => {
    if (!draggable) return
    ;(e.currentTarget as HTMLElement).releasePointerCapture(e.pointerId)
    isDragging.current = false

    if (grabCursor) {
      ;(e.currentTarget as HTMLElement).style.cursor = "grab"
    }
  }

  return (
    <div
      ref={container}
      onPointerDown={handlePointerDown}
      onPointerMove={handlePointerMove}
      onPointerUp={handlePointerUp}
      onPointerCancel={handlePointerUp}
      className={cn("relative", className)}
    >
      <div
        ref={marqueeContainerRef}
        
        style={{ contain: "layout style" }}
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width={width}
          height={height}
          viewBox={viewBox}
          preserveAspectRatio={preserveAspectRatio}
          
        >
          <path
            id={id}
            d={path}
            stroke={showPath ? "currentColor" : "none"}
            fill="none"
            ref={pathRef}
          />
        </svg>

        {items.map(({ child, repeatIndex, itemIndex, key }) => {
        // Create a unique offset transform for each item
        const itemOffset = useTransform(baseOffset, (v) => {
          const position = (itemIndex * 100) / items.length
          const wrappedValue = wrap(0, 100, v + position)
          return `${easing ? easing(wrappedValue / 100) * 100 : wrappedValue}%`
        })

        // Create a motion value for the current offset distance
        const currentOffsetDistance = useMotionValue(0)

        // Update z-index when offset distance changes
        const zIndex = useTransform(currentOffsetDistance, (value) =>
          calculateZIndex(value)
        )

        // Update current offset distance value when animation runs
        useEffect(() => {
          const unsubscribe = itemOffset.on("change", (value: string) => {
            // Parse percentage string to get numerical value
            const match = value.match(/^([\d.]+)%$/)
            if (match && match[1]) {
              currentOffsetDistance.set(parseFloat(match[1]))
            }
          })
          return unsubscribe
        }, [itemOffset, currentOffsetDistance])

        const cssVariables = Object.fromEntries(
          (cssVariableInterpolation || []).map(({ property, from, to }) => [
            property,
            useTransform(currentOffsetDistance, [0, 100], [from, to]),
          ])
        )

        return (
          <motion.div
            key={key}
            ref={(el) => {
              if (el) itemRefs.current.set(key, el)
            }}
            className={cn(
              "absolute top-0 left-0",
              draggable && grabCursor && "cursor-grab"
            )}
            style={{
              offsetPath: `path('${path}')`,
              offsetDistance: itemOffset,
              zIndex: enableRollingZIndex ? zIndex : undefined,
              willChange: "offset-distance",
              backfaceVisibility: "hidden",
              ...cssVariables,
            }}
            aria-hidden={repeatIndex > 0}
            onMouseEnter={() => (isHovered.current = true)}
            onMouseLeave={() => (isHovered.current = false)}
          >
            {child}
          </motion.div>
        )
      })}
      </div>
    </div>
  )
}

export default MarqueeAlongSvgPath
```





## Usage

1. Wrap your elements with the `MarqueeAlongSvgPath` component
2. Provide an SVG path via the required `path` prop (the `d` attribute of an SVG path)
3. Configure the SVG viewport with optional `viewBox` and `preserveAspectRatio` props for proper scaling
4. The elements are distributed evenly along the path, so you'll need to experiment with:
   - The `repeat` prop to control how many copies of your elements appear
   - The size of your elements (width/height)

The component is really similar to the [Simple Marquee Component](https://fancycomponents.dev/docs/components/blocks/simple-marquee), and has the same features and props (and a bit more:)):

- Changing velocity based on scroll velocity
- Slow down on hover
- Draggable elements
- Custom easing

## Understanding the component

Before you dive into understanding this component, please read through the [Simple Marquee](/docs/components/blocks/simple-marquee) component's documentation, as this one is almost identical.

The main difference is that we move the children along an SVG path (instead of a "straight line" positioned with `flexbox` system, as in the other component). **The magic that makes this possible is the `offsetPath` CSS property.**

> The `offset-path` CSS property specifies a path for an element to follow and determines the element's positioning within the path's parent container or the SVG coordinate system. The path is a line, a curve, or a geometrical shape along which the element gets positioned or moves.

as per the [offset-path documentation on MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/offset-path).

We also use the `offsetDistance` property to actually move/offset the element to the correct position along the path in the `offsetPath` CSS property.


**File**: `Offset path example`
```jsx
style={{
  ...
  offsetPath: `path('${path}')`,
  offsetDistance: itemOffset,
}}
```


Each item's offset is calculated separately using an `useTransform` hook from `motion/react`, by converting the `baseOffset` to a percentage value:


**File**: `Item offset calculation`
```jsx
const itemOffset = useTransform(baseOffset, (v) => {
  // evenly distribute items along the path (0-100%)
  const position = (itemIndex * 100) / items.length
  const wrappedValue = wrap(0, 100, v + position)
  return `${easing ? easing(wrappedValue / 100) * 100 : wrappedValue}%`
})
```


The items are evenly distributed along the path. The `wrap` function ensures that items surpassing `100%` are "wrapped back" to `0%`. The `baseOffset` value (the input value for the `useTransform` hook) is calculated by a bunch of different factors, such as:
- a base velocity, which moves the items along the path at a constant speed
- scroll velocity
- slowing down on hover
- direction
- drag velocity

Most of these factors are calculated inside an `useAnimationFrame` hook, which runs every frame. Most of these values are either motion values or refs to avoid unnecessary re-renders. Please refer to the [Simple Marquee Component documentation](/docs/components/blocks/simple-marquee), there is a detailed explanation for each part.

### Z-Index Management

You can enable increasing z-index based on the progress along the path by setting `enableRollingZIndex` to `true`. This is pretty useful when a path is self-crossing, so elements further along the path appear above earlier ones.

The callback function which calculates the current z-index is fairly simple. You can set the `zIndexBase` and `zIndexRange` props to control the base and range of the z-index values. The `zIndexBase` is the starting value, and the `zIndexRange` is the difference between the highest and lowest z-index values.


**File**: `Z-Index calculation`
```jsx
// Function to calculate z-index based on offset distance
const calculateZIndex = useCallback(
  (offsetDistance: number) => {
    if (!enableRollingZIndex) {
      return undefined;
    }
    
    // Simple progress-based z-index
    const normalizedDistance = offsetDistance / 100;
    return Math.floor(zIndexBase + normalizedDistance * zIndexRange);
  },
  [enableRollingZIndex, zIndexBase, zIndexRange]
);

// ...

// Inside an element:
const zIndex = useTransform(
  currentOffsetDistance,
  (value) => calculateZIndex(value)
);
```


## CSS Variable Interpolation

It's also possible to map any CSS property to the path progress using the `cssVariableInterpolation` prop. It accepts an array of objects with `property` and `from` and `to` values. High level example:


**File**: `CSS variable interpolation example`
```jsx
<MarqueeAlongSvgPath
  path="M0,0 C0,0 100,0 100,100"
  cssVariableInterpolation={[
    { property: "opacity", from: 0, to: 1.5 },
    { property: "scale", from: 0.1, to: 1 },
  ]}
>
  {/* Your content */}
</MarqueeAlongSvgPath>
```


{/* 
### Demo Code (marquee-along-svg-path-mapping-demo.tsx)
```tsx
import MarqueeAlongSvgPath from "@/fancy/components/blocks/marquee-along-svg-path"

function generateSpiralPath(turns = 5, centerX = 500, centerY = 137) {
  const points = []
  const numPoints = turns * 300 // number of points to create smooth spiral
  const spacing = 18 // controls how far apart the spiral arms are

  for (let i = 0; i < numPoints; i++) {
    const angle = (i / numPoints) * turns * 2 * Math.PI
    const radius = spacing * angle // radius increases with angle
    const x = centerX + radius * Math.cos(angle)
    const y = centerY + radius * Math.sin(angle)
    points.push(`${i === 0 ? "M" : "L"} ${x} ${y}`)
  }

  return points.join(" ")
}

const path = generateSpiralPath(4)

export default function MarqueeAlongSvgPathDemo() {
  return (
    <div >
      <h2 >fancy</h2>
      <MarqueeAlongSvgPath
        path={path}
        viewBox="0 0 400 474"
        baseVelocity={1}
        showPath={false}
        slowdownOnHover={true}
        repeat={8}
        enableRollingZIndex={false}
        dragSensitivity={0.01}
        
        responsive
        cssVariableInterpolation={[
          { property: "opacity", from: 0, to: 1.5 },
          { property: "scale", from: 0.1, to: 1 },
        ]}
        grabCursor
      >
        {imgs.map((img, i) => (
          <a
            href={img.link}
            target="_blank"
            rel="noopener noreferrer"
            
          >
            <div
              key={i}
              
            >
              <img
                src={img.src}
                alt={`Example ${i}`}
                
                draggable={false}
              />
            </div>
          </a>
        ))}
      </MarqueeAlongSvgPath>
    </div>
  )
}

// EXAMPLE IMAGES
const imgs = [
  {
    src: "https://cdn.cosmos.so/b9909337-7a53-48bc-9672-33fbd0f040a1?format=jpeg",
    link: "https://www.instagram.com/p/DCOl6YTS85e/?igsh=MXNvdHhyczl1djJ6ZA%3D%3D",
  },
  {
    src: "https://cdn.cosmos.so/ecdc9dd7-2862-4c28-abb1-dcc0947390f3?format=jpeg",
    link: "https://www.instagram.com/p/C4RTJvVpP4R/?igsh=MWZwOTNlYTVodGszMw%3D%3D",
  },
  {
    src: "https://cdn.cosmos.so/79de41ec-baa4-4ac0-a9a4-c090005ca640?format=jpeg",
    link: "https://pangrampangram.com/products/mori",
  },
  {
    src: "https://cdn.cosmos.so/1a18b312-21cd-4484-bce5-9fb7ed1c5e01?format=jpeg",
    link: "https://www.sergidelgado.com/selected-work/ampersand",
  },
  {
    src: "https://cdn.cosmos.so/d765f64f-7a66-462f-8b2d-3d7bc8d7db55?format=jpeg",
    link: "https://www.instagram.com/p/C40XmANsoe_/?igsh=MXFlZGx4cmw3ZW1qYw%3D%3D",
  },
  {
    src: "https://cdn.cosmos.so/6b9f08ea-f0c5-471f-a620-71221ff1fb65?format=jpeg",
    link: "https://abduzeedo.com/super-stylish-type-explorations",
  },
  {
    src: "https://cdn.cosmos.so/40a09525-4b00-4666-86f0-3c45f5d77605?format=jpeg",
    link: "https://www.instagram.com/p/CrhdrGjr9yK/?igshid=MTc4MmM1YmI2Ng%3D%3D",
  },
  {
    src: "https://cdn.cosmos.so/14f05ab6-b4d0-4605-9007-8a2190a249d0?format=jpeg",
    link: "https://www.instagram.com/julian.stiber/p/By5RBApiDzE/?img_index=1",
  },
  {
    src: "https://cdn.cosmos.so/d05009a2-a2f8-4a4c-a0de-e1b0379dddb8?format=jpeg",
    link: "https://www.instagram.com/p/CeT3COysRNN/?img_index=2",
  },
  {
    src: "https://cdn.cosmos.so/ba646e35-efc2-494a-961b-b40f597e6fc9?format=jpeg",
    link: "https://www.instagram.com/godfreydadich/",
  },
  {
    src: "https://cdn.cosmos.so/e899f9c3-ed48-4899-8c16-fbd5a60705da?format=jpeg",
    link: "https://www.instagram.com/p/Bty1U6BhTOW/?img_index=5",
  },
  {
    src: "https://cdn.cosmos.so/24e83c11-c607-45cd-88fb-5059960b56a0?format=jpeg",
    link: "https://www.instagram.com/p/C48dxn1LqhC/?igsh=dmV5ZWR0Z2Y3Zzlt&img_index=3",
  },
  {
    src: "https://cdn.cosmos.so/cd346bce-f415-4ea7-8060-99c5f7c1741a?format=jpeg",
    link: "https://www.instagram.com/p/C08ZDVyyRhK/?img_index=2&igsh=bHAyZjcxYW1jZDNu",
  },
]
```
 */}

## Responsivity

When `responsive` is set to `true`, the component will automatically scale and center the entire marquee (SVG + elements) to fit within its container while maintaining aspect ratio.


**File**: `Enabling responsive mode`
```jsx
<MarqueeAlongSvgPath
  path={path}
  viewBox="0 0 996 330" // Important: set viewBox to match your path's bounding box
  
  responsive
>
  {/* Your content */}
</MarqueeAlongSvgPath>
```


**How it works:**
1. The component parses the `viewBox` to get the original dimensions
2. It measures the container's current dimensions using a resize listener
3. It calculates a scale factor: `Math.min(containerWidth / originalWidth, containerHeight / originalHeight)`
4. The marquee is scaled and centered using CSS `transform: translate() scale()`

**Important notes:**
- Make sure your `viewBox` matches the bounding box of your SVG path for proper scaling
- The marquee will be automatically centered within its container
- This technique is simple and performant, but the marquee elements will also be scaled (smaller on smaller screens)

### Alternative: D3-based Path Scaling

For more advanced use cases where you want the elements to maintain their size while the path scales, you can use D3.js to parse and recreate the path at new dimensions. This technique is covered in detail in the [Codrops tutorial](https://tympanus.net/codrops/2025/06/17/building-an-infinite-marquee-along-an-svg-path-with-react-motion/).

## Notes

The component's performance may be impacted by the complexity and length of the SVG path, as well as the number of elements being animated. Keep an eye on it and tweak these factors if you experience performance issues.

## Resources

- [Simple Marquee Component](/docs/components/blocks/simple-marquee)
- [offset-path by MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/offset-path)
- [CSS motion path by MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_motion_path)
- [Motion along path by motion.dev](https://examples.motion.dev/react/motion-path)

## Credits

Click on the individual images in the 2nd demo to see the original artworks & authors.

## Props


| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children* | `ReactNode` | - | The elements to be scrolled along the path |
| path* | `string` | - | The SVG path string that defines the motion path |
| pathId | `string` | - | Optional ID for the SVG path element |
| preserveAspectRatio | `string` | `"xMidYMid meet"` | SVG preserveAspectRatio attribute value |
| showPath | `boolean` | `false` | Whether to show the SVG path |
| width | `string` | `"100%"` | Width of the SVG container |
| height | `string` | `"100%"` | Height of the SVG container |
| viewBox | `string` | `"0 0 100 100"` | SVG viewBox attribute value |
| baseVelocity | `number` | `5` | Base velocity of the animation |
| direction | `"normal" | "reverse"` | `"normal"` | Direction of the animation along the path |
| easing | `(value: number) => number` | - | Custom easing function for the animation |
| slowdownOnHover | `boolean` | `false` | Whether to slow down on hover |
| slowDownFactor | `number` | `0.3` | Factor to slow down by when hovering |
| slowDownSpringConfig | `SpringOptions` | `{ damping: 50, stiffness: 400 }` | Spring configuration for hover slowdown |
| useScrollVelocity | `boolean` | `false` | Whether to use scroll velocity |
| scrollAwareDirection | `boolean` | `false` | Whether to change direction based on scroll |
| scrollSpringConfig | `SpringOptions` | `{ damping: 50, stiffness: 400 }` | Spring configuration for scroll velocity |
| scrollContainer | `RefObject | HTMLElement | null` | - | Custom scroll container reference |
| repeat | `number` | `3` | Number of times to repeat children |
| draggable | `boolean` | `false` | Whether elements can be dragged |
| dragSensitivity | `number` | `0.2` | Sensitivity of drag movement |
| dragVelocityDecay | `number` | `0.96` | Decay rate of drag velocity |
| dragAwareDirection | `boolean` | `false` | Whether to change direction based on drag |
| grabCursor | `boolean` | `false` | Whether to show grab cursor when draggable |
| enableRollingZIndex | `boolean` | `true` | Whether to enable rolling z-index effect |
| zIndexBase | `number` | `1` | Base z-index value |
| zIndexRange | `number` | `10` | Range of z-index values |
| cssVariableInterpolation | `Array` | `[]` | CSS properties to interpolate along the path |
| responsive | `boolean` | `false` | Whether to enable responsive scaling and centering |


---
