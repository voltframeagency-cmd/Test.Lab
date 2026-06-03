# Skiper UI Creative Developer Toolbox Reference Guide

A complete, structured developer guide for **Skiper UI** — a set of clean, copy-paste React components designed with Tailwind CSS, TypeScript, Framer Motion, and GSAP.

## Table of Contents

- [Carousals](#carousals)
  - [Card swipe carousel](#card-swipe-carousel)
  - [Creative carousel 001](#creative-carousel-001)
  - [Creative carousel 002](#creative-carousel-002)
  - [Inverted perspective carousel](#inverted-perspective-carousel)
  - [Perspective carousel](#perspective-carousel)
  - [Shadcn clipPath Carousal](#shadcn-clippath-carousal)
- [Crazy Hero](#crazy-hero)
  - [Canvas crowd](#canvas-crowd)
- [Crazy Hover](#crazy-hover)
  - [ExpandOnHover](#expandonhover)
  - [ExpandOnHover vertical](#expandonhover-vertical)
- [Development tools](#development-tools)
  - [Breakpoint indicator](#breakpoint-indicator)
  - [Debug Pannel](#debug-pannel)
  - [Loop animation hook](#loop-animation-hook)
- [Minimal interactions](#minimal-interactions)
  - [Animated icons 002](#animated-icons-002)
  - [Animated number](#animated-number)
  - [Apple play button](#apple-play-button)
  - [Apple squicircle effect](#apple-squicircle-effect)
  - [CssLink](#csslink)
  - [Gooey Effect](#gooey-effect)
  - [Mouse follow animations](#mouse-follow-animations)
  - [Music toggle btn](#music-toggle-btn)
  - [SVG clip path mask](#svg-clip-path-mask)
  - [Scroll with fade effect](#scroll-with-fade-effect)
  - [Text roll navigation](#text-roll-navigation)
  - [Theme toggle btn](#theme-toggle-btn)
  - [Theme toggle buttons](#theme-toggle-buttons)
- [Out of the box](#out-of-the-box)
  - [Custom tooltip](#custom-tooltip)
- [Rauno](#rauno)
  - [Progressive Blur](#progressive-blur)
- [Scroll Effects](#scroll-effects)
  - [3D perspective text](#3d-perspective-text)
  - [Card stack scroll](#card-stack-scroll)
  - [Card stack with gsap and rotate](#card-stack-with-gsap-and-rotate)
  - [Oliver parallax](#oliver-parallax)
  - [Scroll images reveal 003](#scroll-images-reveal-003)
  - [Scroll progress 001](#scroll-progress-001)
  - [Svg follow scroll](#svg-follow-scroll)
  - [Text Scroll animation](#text-scroll-animation)
- [Video Players](#video-players)
  - [Video player 001](#video-player-001)

---

## Carousals <a name="carousals"></a>

### Card swipe carousel <a name="card-swipe-carousel"></a>

A swipeable card stack component with smooth animations and touch gestures. Features a modern card-stacking effect where users can swipe through cards with natural physics-based animations.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper48
```

#### Dependencies

- **NPM Packages**:
  - `swiper`
  - `framer-motion`
  - `lucide-react`

#### How to Use

```tsx
import { Carousel_002 } from "@/components/v1/skiper48";

// Using the carousel component with custom images
const CustomCarousel = () => {
  const images = [
    {
      src: "/path/to/image1.jpg",
      alt: "Description 1",
    },
    {
      src: "/path/to/image2.jpg",
      alt: "Description 2",
    },
    // ... more images
  ];

  return (
    <Carousel_002
      images={images}
      showPagination={true}
      showNavigation={true}
      loop={true}
      autoplay={true}
      spaceBetween={40}
    />
  );
};
```

#### Component Source Code

##### File: `skiper48.tsx`

```tsx
"use client";

import { motion } from "framer-motion";
import { ChevronLeftIcon, ChevronRightIcon } from "lucide-react";
import React from "react";
import { Autoplay, EffectCards, Navigation, Pagination } from "swiper/modules";
import { Swiper, SwiperSlide } from "swiper/react";
import "swiper/css/effect-cards";
import "swiper/css/pagination";
import "swiper/css/navigation";
import "swiper/css";

import { cn } from "@/lib/utils";

const Skiper48 = () => {
  const images = [
    {
      src: "/images/x.com/13.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/32.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/20.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/21.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/19.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/1.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/2.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/3.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/4.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/5.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/6.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
  ];

  return (
    <div className="flex h-full w-full items-center justify-center overflow-hidden bg-[#f5f4f3]">
      <Carousel_002 className="" images={images} loop />
    </div>
  );
};

export { Skiper48 };

const Carousel_002 = ({
  images,
  className,
  showPagination = false,
  showNavigation = false,
  loop = true,
  autoplay = false,
  spaceBetween = 40,
}: {
  images: { src: string; alt: string }[];
  className?: string;
  showPagination?: boolean;
  showNavigation?: boolean;
  loop?: boolean;
  autoplay?: boolean;
  spaceBetween?: number;
}) => {
  const css = `
  .Carousal_002 {
    padding-bottom: 50px !important;
  }
  `;
  return (
    <motion.div
      initial={{ opacity: 0, translateY: 20 }}
      animate={{ opacity: 1, translateY: 0 }}
      transition={{
        duration: 0.3,
        delay: 0.5,
      }}
      className={cn("relative w-full max-w-3xl", className)}
    >
      <style>{css}</style>

      <Swiper
        spaceBetween={spaceBetween}
        autoplay={
          autoplay
            ? {
                delay: 1000,
                disableOnInteraction: false,
              }
            : false
        }
        effect="cards"
        grabCursor={true}
        loop={loop}
        pagination={
          showPagination
            ? {
                clickable: true,
              }
            : false
        }
        navigation={
          showNavigation
            ? {
                nextEl: ".swiper-button-next",
                prevEl: ".swiper-button-prev",
              }
            : false
        }
        className="Carousal_002 h-[380px] w-[260px]"
        modules={[EffectCards, Autoplay, Pagination, Navigation]}
      >
        {images.map((image, index) => (
          <SwiperSlide key={index} className="rounded-3xl">
            <img
              className="h-full w-full object-cover"
              src={image.src}
              alt={image.alt}
            />
          </SwiperSlide>
        ))}
        {showNavigation && (
          <div>
            <div className="swiper-button-next after:hidden">
              <ChevronRightIcon className="h-6 w-6 text-white" />
            </div>
            <div className="swiper-button-prev after:hidden">
              <ChevronLeftIcon className="h-6 w-6 text-white" />
            </div>
          </div>
        )}
      </Swiper>
    </motion.div>
  );
};

export { Carousel_002 };

/**
 * Skiper 48 Carousel_002 — React + Swiper
 * Built with Swiper.js - Read docs to learn more https://swiperjs.com/
 * Illustrations by AarzooAly - https://x.com/AarzooAly
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### Creative carousel 001 <a name="creative-carousel-001"></a>

A beautiful creative effect carousel with smooth animations and customizable navigation. Features a stunning creative effect that creates depth and visual interest with centered slides and customizable options.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper50
```

#### Dependencies

- **NPM Packages**:
  - `swiper`
  - `framer-motion`
  - `lucide-react`

#### How to Use

```tsx
import { Carousel_004 } from "@/components/v1/skiper50";

// Using the carousel component with custom images
const CustomCarousel = () => {
  const images = [
    {
      src: "/path/to/image1.jpg",
      alt: "Description 1",
    },
    {
      src: "/path/to/image2.jpg",
      alt: "Description 2",
    },
    // ... more images
  ];

  return (
    <Carousel_004
      images={images}
      showPagination={true}
      showNavigation={true}
      loop={true}
      autoplay={true}
      spaceBetween={0}
    />
  );
};
```

#### Component Source Code

##### File: `skiper50.tsx`

```tsx
"use client";

import { motion } from "framer-motion";
import { ChevronLeftIcon, ChevronRightIcon } from "lucide-react";
import React from "react";
import { Autoplay, EffectCreative, Pagination } from "swiper/modules";
import { Swiper, SwiperSlide } from "swiper/react";
import "swiper/css";
import "swiper/css/effect-creative";
import "swiper/css/pagination";
import "swiper/css/autoplay";

import { cn } from "@/lib/utils";

const Skiper50 = () => {
  const images = [
    {
      src: "/images/x.com/13.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/32.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/20.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/21.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/19.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/1.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/2.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/3.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/4.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/5.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/6.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
  ];

  return (
    <div className="flex h-full w-full items-center justify-center overflow-hidden bg-[#f5f4f3]">
      <Carousel_004 className="" images={images} showPagination loop />
    </div>
  );
};

export { Skiper50 };

const Carousel_004 = ({
  images,
  className,
  showPagination = false,
  showNavigation = false,
  loop = true,
  autoplay = false,
  spaceBetween = 0,
}: {
  images: { src: string; alt: string }[];
  className?: string;
  showPagination?: boolean;
  showNavigation?: boolean;
  loop?: boolean;
  autoplay?: boolean;
  spaceBetween?: number;
}) => {
  const css = `
  .Carousal_004 {
    width: 100%;
    height: 440px;
    padding-bottom: 50px !important;
  }
  
  .Carousal_004 .swiper-slide {
    background-position: center;
    background-size: cover;
    width: 300px;
    border-radius: 25px;
  }
  
 
  `;
  return (
    <motion.div
      initial={{ opacity: 0, translateY: 20 }}
      animate={{ opacity: 1, translateY: 0 }}
      transition={{
        duration: 0.3,
        delay: 0.5,
      }}
      className={cn("relative w-full max-w-4xl px-5", className)}
    >
      <style>{css}</style>

      <motion.div
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        transition={{ duration: 0.3 }}
        className="w-full"
      >
        <Swiper
          spaceBetween={spaceBetween}
          autoplay={
            autoplay
              ? {
                  delay: 1500,
                  disableOnInteraction: true,
                }
              : false
          }
          effect="creative"
          grabCursor={true}
          slidesPerView="auto"
          centeredSlides={true}
          loop={loop}
          pagination={
            showPagination
              ? {
                  clickable: true,
                }
              : false
          }
          navigation={
            showNavigation
              ? {
                  nextEl: ".swiper-button-next",
                  prevEl: ".swiper-button-prev",
                }
              : false
          }
          className="Carousal_004"
          creativeEffect={{
            prev: {
              shadow: true,
              origin: "left center",
              translate: ["-5%", 0, -200],
              rotate: [0, 100, 0],
            },
            next: {
              origin: "right center",
              translate: ["5%", 0, -200],
              rotate: [0, -100, 0],
            },
          }}
          modules={[EffectCreative, Pagination, Autoplay]}
        >
          {images.map((image, index) => (
            <SwiperSlide key={index} className="">
              <img
                className="h-full w-full scale-105 rounded-3xl object-cover"
                src={image.src}
                alt={image.alt}
              />
            </SwiperSlide>
          ))}
          {showNavigation && (
            <div>
              <div className="swiper-button-next after:hidden">
                <ChevronRightIcon className="h-6 w-6 text-white" />
              </div>
              <div className="swiper-button-prev after:hidden">
                <ChevronLeftIcon className="h-6 w-6 text-white" />
              </div>
            </div>
          )}
        </Swiper>
      </motion.div>
    </motion.div>
  );
};

export { Carousel_004 };

/**
 * Skiper 50 Carousel_004 — React + Swiper
 * Built with Swiper.js - Read docs to learn more https://swiperjs.com/
 * Illustrations by AarzooAly - https://x.com/AarzooAly
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### Creative carousel 002 <a name="creative-carousel-002"></a>

A beautiful creative effect carousel with smooth animations and customizable navigation. Features a stunning creative effect that creates depth and visual interest with centered slides and customizable options.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper51
```

#### Dependencies

- **NPM Packages**:
  - `swiper`
  - `framer-motion`
  - `lucide-react`

#### How to Use

```tsx
import { Carousel_005 } from "@/components/v1/skiper51";

// Using the carousel component with custom images
const CustomCarousel = () => {
  const images = [
    {
      src: "/path/to/image1.jpg",
      alt: "Description 1",
    },
    {
      src: "/path/to/image2.jpg",
      alt: "Description 2",
    },
    // ... more images
  ];

  return (
    <Carousel_005
      images={images}
      showPagination={true}
      showNavigation={true}
      loop={true}
      autoplay={true}
      spaceBetween={0}
    />
  );
};
```

#### Component Source Code

##### File: `skiper51.tsx`

```tsx
"use client";

import { motion } from "framer-motion";
import { ChevronLeftIcon, ChevronRightIcon } from "lucide-react";
import React from "react";
import { Autoplay, EffectCreative, Pagination } from "swiper/modules";
import { Swiper, SwiperSlide } from "swiper/react";
import "swiper/css";
import "swiper/css/effect-creative";
import "swiper/css/pagination";
import "swiper/css/autoplay";

import { cn } from "@/lib/utils";

const Skiper51 = () => {
  const images = [
    {
      src: "/images/x.com/13.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/32.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/20.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/21.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/19.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/1.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/2.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/3.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/4.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/5.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/6.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
  ];

  return (
    <div className="flex h-full w-full items-center justify-center overflow-hidden bg-[#f5f4f3]">
      <Carousel_005 className="" images={images} autoplay showPagination loop />
    </div>
  );
};

export { Skiper51 };

const Carousel_005 = ({
  images,
  className,
  showPagination = false,
  showNavigation = false,
  loop = true,
  autoplay = false,
  spaceBetween = 0,
}: {
  images: { src: string; alt: string }[];
  className?: string;
  showPagination?: boolean;
  showNavigation?: boolean;
  loop?: boolean;
  autoplay?: boolean;
  spaceBetween?: number;
}) => {
  const css = `
  .Carousal_005 {
    width: 100%;
    height: 440px;
    padding-bottom: 50px !important;
  }
  
  .Carousal_005 .swiper-slide {
    background-position: center;
    background-size: cover;
     border-radius: 25px;
  }

  .Carousal_005 .swiper-pagination-bullet {
    background-color: #000 !important;
  }
 
  `;
  return (
    <motion.div
      initial={{ opacity: 0, translateY: 20 }}
      animate={{ opacity: 1, translateY: 0 }}
      transition={{
        duration: 0.3,
        delay: 0.5,
      }}
      className={cn("relative w-full max-w-4xl px-5", className)}
    >
      <style>{css}</style>

      <motion.div
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        transition={{ duration: 0.3 }}
        className="w-full"
      >
        <Swiper
          spaceBetween={spaceBetween}
          autoplay={
            autoplay
              ? {
                  delay: 1500,
                  disableOnInteraction: true,
                }
              : false
          }
          effect="creative"
          grabCursor={true}
          slidesPerView="auto"
          centeredSlides={true}
          loop={loop}
          pagination={
            showPagination
              ? {
                  clickable: true,
                }
              : false
          }
          navigation={
            showNavigation
              ? {
                  nextEl: ".swiper-button-next",
                  prevEl: ".swiper-button-prev",
                }
              : false
          }
          className="Carousal_005"
          creativeEffect={{
            prev: {
              shadow: true,
              translate: [0, 0, -400],
            },
            next: {
              translate: ["100%", 0, 0],
            },
          }}
          modules={[EffectCreative, Pagination, Autoplay]}
        >
          {images.map((image, index) => (
            <SwiperSlide key={index} className="">
              <img
                className="h-full w-full scale-105 rounded-3xl object-cover"
                src={image.src}
                alt={image.alt}
              />
            </SwiperSlide>
          ))}
          {showNavigation && (
            <div>
              <div className="swiper-button-next after:hidden">
                <ChevronRightIcon className="h-6 w-6 text-white" />
              </div>
              <div className="swiper-button-prev after:hidden">
                <ChevronLeftIcon className="h-6 w-6 text-white" />
              </div>
            </div>
          )}
        </Swiper>
      </motion.div>
    </motion.div>
  );
};

export { Carousel_005 };

/**
 * Skiper 51 Carousel_005 — React + Swiper
 * Built with Swiper.js - Read docs to learn more https://swiperjs.com/
 * Illustrations by AarzooAly - https://x.com/AarzooAly
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### Inverted perspective carousel <a name="inverted-perspective-carousel"></a>

A beautiful 3D coverflow carousel with smooth animations and customizable navigation. Features a stunning 3D effect that creates depth and visual interest with centered slides and customizable options.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper49
```

#### Dependencies

- **NPM Packages**:
  - `swiper`
  - `framer-motion`
  - `lucide-react`

#### How to Use

```tsx
import { Carousel_003 } from "@/components/v1/skiper49";

// Using the carousel component with custom images
const CustomCarousel = () => {
  const images = [
    {
      src: "/path/to/image1.jpg",
      alt: "Description 1",
    },
    {
      src: "/path/to/image2.jpg",
      alt: "Description 2",
    },
    // ... more images
  ];

  return (
    <Carousel_003
      images={images}
      showPagination={true}
      showNavigation={true}
      loop={true}
      autoplay={true}
      spaceBetween={0}
    />
  );
};
```

#### Component Source Code

##### File: `skiper49.tsx`

```tsx
"use client";

import { motion } from "framer-motion";
import { ChevronLeftIcon, ChevronRightIcon } from "lucide-react";
import React from "react";
import {
  Autoplay,
  EffectCoverflow,
  Navigation,
  Pagination,
} from "swiper/modules";
import { Swiper, SwiperSlide } from "swiper/react";
import "swiper/css/effect-coverflow";
import "swiper/css/pagination";
import "swiper/css/navigation";
import "swiper/css";
import "swiper/css/effect-cards";

import { cn } from "@/lib/utils";

const Skiper49 = () => {
  const images = [
    {
      src: "/images/x.com/13.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/32.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/20.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/21.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/19.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/1.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/2.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/3.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/4.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/5.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/6.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
  ];

  return (
    <div className="flex h-full w-full items-center justify-center overflow-hidden bg-[#f5f4f3]">
      <Carousel_003 className="" images={images} showPagination loop />
    </div>
  );
};

export { Skiper49 };

const Carousel_003 = ({
  images,
  className,
  showPagination = false,
  showNavigation = false,
  loop = true,
  autoplay = false,
  spaceBetween = 0,
}: {
  images: { src: string; alt: string }[];
  className?: string;
  showPagination?: boolean;
  showNavigation?: boolean;
  loop?: boolean;
  autoplay?: boolean;
  spaceBetween?: number;
}) => {
  const css = `
  .Carousal_003 {
    width: 100%;
    height: 350px;
    padding-bottom: 50px !important;
  }
  
  .Carousal_003 .swiper-slide {
    background-position: center;
    background-size: cover;
    width: 300px;
  }

  .swiper-pagination-bullet {
    background-color: #000 !important;
  }

`;
  return (
    <motion.div
      initial={{ opacity: 0, translateY: 20 }}
      animate={{ opacity: 1, translateY: 0 }}
      transition={{
        duration: 0.3,
        delay: 0.5,
      }}
      className={cn("relative w-full max-w-4xl px-5", className)}
    >
      <style>{css}</style>

      <motion.div
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        transition={{ duration: 0.3 }}
        className="w-full"
      >
        <Swiper
          spaceBetween={spaceBetween}
          autoplay={
            autoplay
              ? {
                  delay: 1500,
                  disableOnInteraction: true,
                }
              : false
          }
          effect="coverflow"
          grabCursor={true}
          slidesPerView="auto"
          centeredSlides={true}
          loop={loop}
          coverflowEffect={{
            rotate: 40,
            stretch: 0,
            depth: 100,
            modifier: 1,
            slideShadows: true,
          }}
          pagination={
            showPagination
              ? {
                  clickable: true,
                }
              : false
          }
          navigation={
            showNavigation
              ? {
                  nextEl: ".swiper-button-next",
                  prevEl: ".swiper-button-prev",
                }
              : false
          }
          className="Carousal_003"
          modules={[EffectCoverflow, Autoplay, Pagination, Navigation]}
        >
          {images.map((image, index) => (
            <SwiperSlide key={index} className="">
              <img
                className="h-full w-full object-cover"
                src={image.src}
                alt={image.alt}
              />
            </SwiperSlide>
          ))}
          {showNavigation && (
            <div>
              <div className="swiper-button-next after:hidden">
                <ChevronRightIcon className="h-6 w-6 text-white" />
              </div>
              <div className="swiper-button-prev after:hidden">
                <ChevronLeftIcon className="h-6 w-6 text-white" />
              </div>
            </div>
          )}
        </Swiper>
      </motion.div>
    </motion.div>
  );
};

export { Carousel_003 };

/**
 * Skiper 49 Carousel_003 — React + Swiper
 * Built with Swiper.js - Read docs to learn more https://swiperjs.com/
 * Illustrations by AarzooAly - https://x.com/AarzooAly
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### Perspective carousel <a name="perspective-carousel"></a>

A beautiful 3D coverflow carousel with smooth animations and customizable navigation. Features a stunning 3D effect that creates depth and visual interest with centered slides and customizable options.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper47
```

#### Dependencies

- **NPM Packages**:
  - `swiper`
  - `framer-motion`
  - `lucide-react`

#### How to Use

```tsx
import { Carousel_001 } from "@/components/v1/skiper47";

// Using the carousel component with custom images
const CustomCarousel = () => {
  const images = [
    {
      src: "/path/to/image1.jpg",
      alt: "Description 1",
    },
    {
      src: "/path/to/image2.jpg",
      alt: "Description 2",
    },
    // ... more images
  ];

  return (
    <Carousel_001
      images={images}
      showPagination={true}
      showNavigation={true}
      loop={true}
      autoplay={true}
      spaceBetween={40}
    />
  );
};
```

#### Component Source Code

##### File: `skiper47.tsx`

```tsx
"use client";

import { motion } from "framer-motion";
import { ChevronLeftIcon, ChevronRightIcon } from "lucide-react";
import React from "react";
import {
  Autoplay,
  EffectCoverflow,
  Navigation,
  Pagination,
} from "swiper/modules";
import { Swiper, SwiperSlide } from "swiper/react";
import "swiper/css/effect-coverflow";
import "swiper/css/pagination";
import "swiper/css/navigation";
import "swiper/css";
import "swiper/css/effect-cards";

import { cn } from "@/lib/utils";

const Skiper47 = () => {
  const images = [
    {
      src: "/images/x.com/11.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/13.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/32.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/20.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/21.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
    {
      src: "/images/x.com/19.jpeg",
      alt: "Illustrations by my fav AarzooAly",
    },
  ];

  return (
    <div className="flex h-full w-full items-center justify-center overflow-hidden bg-[#f5f4f3]">
      <Carousel_001 className="" images={images} showPagination loop />
    </div>
  );
};

export { Skiper47 };

const Carousel_001 = ({
  images,
  className,
  showPagination = false,
  showNavigation = false,
  loop = true,
  autoplay = false,
  spaceBetween = 40,
}: {
  images: { src: string; alt: string }[];
  className?: string;
  showPagination?: boolean;
  showNavigation?: boolean;
  loop?: boolean;
  autoplay?: boolean;
  spaceBetween?: number;
}) => {
  const css = `
  .Carousal_001 {
    padding-bottom: 50px !important;
  }
  `;
  return (
    <motion.div
      initial={{ opacity: 0, translateY: 20 }}
      animate={{ opacity: 1, translateY: 0 }}
      transition={{
        duration: 0.3,
        delay: 0.5,
      }}
      className={cn("w-3xl relative", className)}
    >
      <style>{css}</style>

      <Swiper
        spaceBetween={spaceBetween}
        autoplay={
          autoplay
            ? {
                delay: 1500,
                disableOnInteraction: false,
              }
            : false
        }
        effect="coverflow"
        grabCursor={true}
        centeredSlides={true}
        loop={loop}
        slidesPerView={2.43}
        coverflowEffect={{
          rotate: 0,
          slideShadows: false,
          stretch: 0,
          depth: 100,
          modifier: 2.5,
        }}
        pagination={
          showPagination
            ? {
                clickable: true,
              }
            : false
        }
        navigation={
          showNavigation
            ? {
                nextEl: ".swiper-button-next",
                prevEl: ".swiper-button-prev",
              }
            : false
        }
        className="Carousal_001"
        modules={[EffectCoverflow, Autoplay, Pagination, Navigation]}
      >
        {images.map((image, index) => (
          <SwiperSlide key={index} className="!h-[320px] w-full border">
            <img
              className="h-full w-full object-cover"
              src={image.src}
              alt={image.alt}
            />
          </SwiperSlide>
        ))}
        {showNavigation && (
          <div>
            <div className="swiper-button-next after:hidden">
              <ChevronRightIcon className="h-6 w-6 text-white" />
            </div>
            <div className="swiper-button-prev after:hidden">
              <ChevronLeftIcon className="h-6 w-6 text-white" />
            </div>
          </div>
        )}
      </Swiper>
    </motion.div>
  );
};

export { Carousel_001 };

/**
 * Skiper 47 Carousel_001 — React + Swiper
 * Built with Swiper.js - Read docs to learn more https://swiperjs.com/
 * Illustrations by AarzooAly - https://x.com/AarzooAly
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### Shadcn clipPath Carousal <a name="shadcn-clippath-carousal"></a>

A beautiful carousel with clip-path animations and smooth transitions. Features a stunning clip-path effect that creates depth and visual interest with centered slides and customizable options.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper54
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
  - `embla-carousel-autoplay`
  - `lucide-react`
- **Registry Components**:
  - `carousel`

#### How to Use

```tsx
# install dependencies
  npm embla-carousel-autoplay
  npm i framer-motion
  npx shadcn@latest add carousel
```

#### Component Source Code

##### File: `skiper54.tsx`

```tsx
"use client";

import Autoplay from "embla-carousel-autoplay";
import { AnimatePresence, motion } from "framer-motion";
import { ChevronLeft, ChevronRight } from "lucide-react";
import React, { useEffect, useState } from "react";

import { cn } from "@/lib/utils";

import {
  Carousel,
  CarouselApi,
  CarouselContent,
  CarouselItem,
} from "@/components/ui/carousel";

const Skiper54 = () => {
  const images = [
    {
      src: "/images/x.com/13.jpeg",
      alt: "Illustrations by ©AarzooAly",
      title: "Block Reader",
    },
    {
      src: "/images/x.com/9.jpeg",
      alt: "Illustrations by ©AarzooAly",
      title: "Forest Fungi",
    },
    {
      src: "/images/x.com/20.jpeg",
      alt: "Illustrations by ©AarzooAly",
      title: "Golden Dusk",
    },
    {
      src: "/images/x.com/21.jpeg",
      alt: "Illustrations by ©AarzooAly",
      title: "Silent Peaks",
    },
    {
      src: "/images/x.com/25.jpeg",
      alt: "Illustrations by ©AarzooAly",
      title: "Emerald Woods",
    },
    {
      src: "/images/x.com/32.jpeg",
      alt: "Illustrations by ©AarzooAly",
      title: "Falling Mist",
    },
    {
      src: "/images/x.com/19.jpeg",
      alt: "Illustrations by ©AarzooAly",
      title: "Midnight Veil",
    },
    {
      src: "/images/x.com/3.jpeg",
      alt: "Illustrations by ©AarzooAly",
      title: "Azure Ridge",
    },
    {
      src: "/images/x.com/2.jpeg",
      alt: "Illustrations by ©AarzooAly",
      title: "Cloud Summit",
    },
  ];
  return (
    <div className="flex h-full w-screen items-center justify-center overflow-hidden bg-[#f5f4f3]">
      <Carousel_006
        images={images}
        className=""
        loop={true}
        showNavigation={true}
        showPagination={true}
      />
    </div>
  );
};

interface Carousel_006Props {
  images: { src: string; alt: string; title: string }[];
  className?: string;
  autoplay?: boolean;
  loop?: boolean;
  showNavigation?: boolean;
  showPagination?: boolean;
}

const Carousel_006 = ({
  images,
  className,
  autoplay = false,
  loop = true,
  showNavigation = true,
  showPagination = true,
}: Carousel_006Props) => {
  const [api, setApi] = useState<CarouselApi>();
  const [current, setCurrent] = useState(0);

  useEffect(() => {
    if (!api) return;

    api.on("select", () => {
      setCurrent(api.selectedScrollSnap());
    });
  }, [api]);

  return (
    <Carousel
      setApi={setApi}
      className={cn("w-full", className)}
      opts={{
        loop,
        slidesToScroll: 1,
      }}
      plugins={
        autoplay
          ? [
              Autoplay({
                delay: 2000,
                stopOnInteraction: true,
                stopOnMouseEnter: true,
              }),
            ]
          : []
      }
    >
      <CarouselContent className="flex h-[500px] w-full">
        {images.map((img, index) => (
          <CarouselItem
            key={index}
            className="relative flex h-[81.5%] w-full basis-[73%] items-center justify-center sm:basis-[50%] md:basis-[30%] lg:basis-[25%] xl:basis-[21%]"
          >
            <motion.div
              initial={false}
              animate={{
                clipPath:
                  current !== index
                    ? "inset(15% 0 15% 0 round 2rem)"
                    : "inset(0 0 0 0 round 2rem)",
              }}
              className="h-full w-full overflow-hidden rounded-3xl"
            >
              <div className="relative h-full w-full border">
                <img
                  src={img.src}
                  alt={img.alt}
                  className="h-full w-full scale-105 object-cover"
                />
              </div>
            </motion.div>
            <AnimatePresence mode="wait">
              {current === index && (
                <motion.div
                  initial={{ opacity: 0, filter: "blur(10px)" }}
                  animate={{ opacity: 1, filter: "blur(0px)" }}
                  transition={{ duration: 0.5 }}
                  className="absolute bottom-0 left-2 flex h-[14%] w-full translate-y-full items-center justify-center p-2 text-center font-medium tracking-tight text-black/20"
                >
                  {img.title}
                </motion.div>
              )}
            </AnimatePresence>
          </CarouselItem>
        ))}
      </CarouselContent>

      {showNavigation && (
        <div className="absolute -bottom-4 right-0 flex w-full items-center justify-between gap-2 px-4">
          <button
            aria-label="Previous slide"
            onClick={() => api?.scrollPrev()}
            className="rounded-full bg-black/10 p-2"
          >
            <ChevronLeft className="text-white" />
          </button>
          <button
            aria-label="Next slide"
            onClick={() => api?.scrollNext()}
            className="rounded-full bg-black/10 p-2"
          >
            <ChevronRight className="text-white" />
          </button>
        </div>
      )}

      {showPagination && (
        <div className="flex w-full items-center justify-center">
          <div className="flex items-center justify-center gap-2">
            {Array.from({ length: images.length }).map((_, index) => (
              <button
                key={index}
                onClick={() => api?.scrollTo(index)}
                className={cn(
                  "h-2 w-2 cursor-pointer rounded-full transition-all",
                  current === index ? "bg-black" : "bg-[#D9D9D9]",
                )}
                aria-label={`Go to slide ${index + 1}`}
              />
            ))}
          </div>
        </div>
      )}
    </Carousel>
  );
};

export { Skiper54 };

/**
 * Skiper 54 Carousel_006 — React + Framer Motion
 * Built with shadcn/ui And Embla Carousel - Read docs to learn more https://ui.shadcn.com/docs/components/carousel https://embla-carousel.com/
 *
 * Illustrations by AarzooAly - https://x.com/AarzooAly
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

## Crazy Hero <a name="crazy-hero"></a>

### Canvas crowd <a name="canvas-crowd"></a>

An animated crowd canvas that creates a lively scene of walking people using HTML5 Canvas and GSAP animations. Features sprite-based character animations with realistic walking movements and crowd management.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper39
```

#### Dependencies

- **NPM Packages**:
  - `gsap`

#### How to Use

```tsx
import { CrowdCanvas, Skiper39 } from "@/components/v1/skiper39";

// Using the complete component
const DemoSkiper39 = () => {
  return <Skiper39 />;
};

// Using just the crowd canvas
const CustomCrowd = () => {
  return (
    <div className="relative h-screen w-full">
      <CrowdCanvas src="/images/peeps/all-peeps.png" rows={15} cols={7} />
    </div>
  );
};
```

#### Component Source Code

##### File: `skiper39.tsx`

```tsx
"use client";

import { gsap } from "gsap";
import React, { useEffect, useRef } from "react";

interface CrowdCanvasProps {
  src: string;
  rows?: number;
  cols?: number;
}

const CrowdCanvas = ({ src, rows = 15, cols = 7 }: CrowdCanvasProps) => {
  const canvasRef = useRef<HTMLCanvasElement>(null);

  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;

    const ctx = canvas.getContext("2d");
    if (!ctx) return;

    const config = {
      src,
      rows,
      cols,
    };

    // UTILS
    const randomRange = (min: number, max: number) =>
      min + Math.random() * (max - min);
    const randomIndex = (array: any[]) => randomRange(0, array.length) | 0;
    const removeFromArray = (array: any[], i: number) => array.splice(i, 1)[0];
    const removeItemFromArray = (array: any[], item: any) =>
      removeFromArray(array, array.indexOf(item));
    const removeRandomFromArray = (array: any[]) =>
      removeFromArray(array, randomIndex(array));
    const getRandomFromArray = (array: any[]) => array[randomIndex(array) | 0];

    // TWEEN FACTORIES
    const resetPeep = ({ stage, peep }: { stage: any; peep: any }) => {
      const direction = Math.random() > 0.5 ? 1 : -1;
      const offsetY = 100 - 250 * gsap.parseEase("power2.in")(Math.random());
      const startY = stage.height - peep.height + offsetY;
      let startX: number;
      let endX: number;

      if (direction === 1) {
        startX = -peep.width;
        endX = stage.width;
        peep.scaleX = 1;
      } else {
        startX = stage.width + peep.width;
        endX = 0;
        peep.scaleX = -1;
      }

      peep.x = startX;
      peep.y = startY;
      peep.anchorY = startY;

      return {
        startX,
        startY,
        endX,
      };
    };

    const normalWalk = ({ peep, props }: { peep: any; props: any }) => {
      const { startX, startY, endX } = props;
      const xDuration = 10;
      const yDuration = 0.25;

      const tl = gsap.timeline();
      tl.timeScale(randomRange(0.5, 1.5));
      tl.to(
        peep,
        {
          duration: xDuration,
          x: endX,
          ease: "none",
        },
        0,
      );
      tl.to(
        peep,
        {
          duration: yDuration,
          repeat: xDuration / yDuration,
          yoyo: true,
          y: startY - 10,
        },
        0,
      );

      return tl;
    };

    const walks = [normalWalk];

    // TYPES
    type Peep = {
      image: HTMLImageElement;
      rect: number[];
      width: number;
      height: number;
      drawArgs: any[];
      x: number;
      y: number;
      anchorY: number;
      scaleX: number;
      walk: any;
      setRect: (rect: number[]) => void;
      render: (ctx: CanvasRenderingContext2D) => void;
    };

    // FACTORY FUNCTIONS
    const createPeep = ({
      image,
      rect,
    }: {
      image: HTMLImageElement;
      rect: number[];
    }): Peep => {
      const peep: Peep = {
        image,
        rect: [],
        width: 0,
        height: 0,
        drawArgs: [],
        x: 0,
        y: 0,
        anchorY: 0,
        scaleX: 1,
        walk: null,
        setRect: (rect: number[]) => {
          peep.rect = rect;
          peep.width = rect[2];
          peep.height = rect[3];
          peep.drawArgs = [peep.image, ...rect, 0, 0, peep.width, peep.height];
        },
        render: (ctx: CanvasRenderingContext2D) => {
          ctx.save();
          ctx.translate(peep.x, peep.y);
          ctx.scale(peep.scaleX, 1);
          ctx.drawImage(
            peep.image,
            peep.rect[0],
            peep.rect[1],
            peep.rect[2],
            peep.rect[3],
            0,
            0,
            peep.width,
            peep.height,
          );
          ctx.restore();
        },
      };

      peep.setRect(rect);
      return peep;
    };

    // MAIN
    const img = document.createElement("img");
    const stage = {
      width: 0,
      height: 0,
    };

    const allPeeps: Peep[] = [];
    const availablePeeps: Peep[] = [];
    const crowd: Peep[] = [];

    const createPeeps = () => {
      const { rows, cols } = config;
      const { naturalWidth: width, naturalHeight: height } = img;
      const total = rows * cols;
      const rectWidth = width / rows;
      const rectHeight = height / cols;

      for (let i = 0; i < total; i++) {
        allPeeps.push(
          createPeep({
            image: img,
            rect: [
              (i % rows) * rectWidth,
              ((i / rows) | 0) * rectHeight,
              rectWidth,
              rectHeight,
            ],
          }),
        );
      }
    };

    const initCrowd = () => {
      while (availablePeeps.length) {
        addPeepToCrowd().walk.progress(Math.random());
      }
    };

    const addPeepToCrowd = () => {
      const peep = removeRandomFromArray(availablePeeps);
      const walk = getRandomFromArray(walks)({
        peep,
        props: resetPeep({
          peep,
          stage,
        }),
      }).eventCallback("onComplete", () => {
        removePeepFromCrowd(peep);
        addPeepToCrowd();
      });

      peep.walk = walk;

      crowd.push(peep);
      crowd.sort((a, b) => a.anchorY - b.anchorY);

      return peep;
    };

    const removePeepFromCrowd = (peep: Peep) => {
      removeItemFromArray(crowd, peep);
      availablePeeps.push(peep);
    };

    const render = () => {
      if (!canvas) return;
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.save();
      ctx.scale(devicePixelRatio, devicePixelRatio);

      crowd.forEach((peep) => {
        peep.render(ctx);
      });

      ctx.restore();
    };

    const resize = () => {
      if (!canvas) return;
      stage.width = canvas.clientWidth;
      stage.height = canvas.clientHeight;
      canvas.width = stage.width * devicePixelRatio;
      canvas.height = stage.height * devicePixelRatio;

      crowd.forEach((peep) => {
        peep.walk.kill();
      });

      crowd.length = 0;
      availablePeeps.length = 0;
      availablePeeps.push(...allPeeps);

      initCrowd();
    };

    const init = () => {
      createPeeps();
      resize();
      gsap.ticker.add(render);
    };

    img.onload = init;
    img.src = config.src;

    const handleResize = () => resize();
    window.addEventListener("resize", handleResize);

    return () => {
      window.removeEventListener("resize", handleResize);
      gsap.ticker.remove(render);
      crowd.forEach((peep) => {
        if (peep.walk) peep.walk.kill();
      });
    };
  }, []);
  return (
    <canvas ref={canvasRef} className="absolute bottom-0 h-[90vh] w-full" />
  );
};

const Skiper39 = () => {
  return (
    <div className="relative h-full w-full bg-white text-black">
      <div className="top-22 absolute left-1/2 grid -translate-x-1/2 content-start justify-items-center gap-6 text-center text-black">
        <span className="relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-white after:to-black after:content-['']">
          Croud Canvas
        </span>
      </div>
      <div className="absolute bottom-0 h-full w-screen">
        <CrowdCanvas src="/images/peeps/all-peeps.png" rows={15} cols={7} />
      </div>
    </div>
  );
};

export { CrowdCanvas, Skiper39 };

/**
 * Skiper 39 Canvas_Landing_004 — React + Canvas
 * Inspired by and adapted from https://codepen.io/zadvorsky/pen/xxwbBQV
 * illustration by https://www.openpeeps.com/
 * We respect the original creators. This is an inspired rebuild with our own taste and does not claim any ownership.
 * These animations aren’t associated with the codepen.io . They’re independent recreations meant to study interaction design
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

## Crazy Hover <a name="crazy-hover"></a>

### ExpandOnHover <a name="expandonhover"></a>

A beautiful hover expand gallery with smooth animations and interactive effects. Features a stunning horizontal expand effect that creates depth and visual interest with hover interactions and customizable options.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper52
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`

#### How to Use

```tsx
import { HoverExpand_001 } from "@/components/v1/skiper52";

// Using the hover expand component with custom images
const CustomHoverExpand = () => {
  const images = [
    {
      src: "/path/to/image1.jpg",
      alt: "Description 1",
      code: "# 01",
    },
    {
      src: "/path/to/image2.jpg",
      alt: "Description 2",
      code: "# 02",
    },
    // ... more images
  ];

  return <HoverExpand_001 images={images} className="" />;
};
```

#### Component Source Code

##### File: `skiper52.tsx`

```tsx
"use client";

import { AnimatePresence, motion } from "framer-motion";
import React, { useState } from "react";
import "swiper/css";
import "swiper/css/effect-creative";
import "swiper/css/pagination";
import "swiper/css/autoplay";

import { cn } from "@/lib/utils";

const Skiper52 = () => {
  const images = [
    {
      src: "/images/x.com/13.jpeg",
      alt: "Illustrations by my fav AarzooAly",
      code: "# 23",
    },
    {
      src: "/images/x.com/32.jpeg",
      alt: "Illustrations by my fav AarzooAly",
      code: "# 23",
    },
    {
      src: "/images/x.com/20.jpeg",
      alt: "Illustrations by my fav AarzooAly",
      code: "# 23",
    },
    {
      src: "/images/x.com/21.jpeg",
      alt: "Illustrations by my fav AarzooAly",
      code: "# 23",
    },
    {
      src: "/images/x.com/19.jpeg",
      alt: "Illustrations by my fav AarzooAly",
      code: "# 23",
    },
    {
      src: "/images/x.com/1.jpeg",
      alt: "Illustrations by my fav AarzooAly",
      code: "# 23",
    },
    {
      src: "/images/x.com/2.jpeg",
      alt: "Illustrations by my fav AarzooAly",
      code: "# 23",
    },
    {
      src: "/images/x.com/3.jpeg",
      alt: "Illustrations by my fav AarzooAly",
      code: "# 23",
    },
    {
      src: "/images/x.com/4.jpeg",
      alt: "Illustrations by my fav AarzooAly",
      code: "# 23",
    },
  ];

  return (
    <div className="flex h-full w-full items-center justify-center overflow-hidden bg-[#f5f4f3]">
      <HoverExpand_001 className="" images={images} />{" "}
    </div>
  );
};

export { Skiper52 };

const HoverExpand_001 = ({
  images,
  className,
}: {
  images: { src: string; alt: string; code: string }[];
  className?: string;
}) => {
  const [activeImage, setActiveImage] = useState<number | null>(1);

  return (
    <motion.div
      initial={{ opacity: 0, translateY: 20 }}
      animate={{ opacity: 1, translateY: 0 }}
      transition={{
        duration: 0.3,
        delay: 0.5,
      }}
      className={cn("relative w-full max-w-6xl px-5", className)}
    >
      <motion.div
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        transition={{ duration: 0.3 }}
        className="w-full"
      >
        <div className="flex w-full items-center justify-center gap-1">
          {images.map((image, index) => (
            <motion.div
              key={index}
              className="relative cursor-pointer overflow-hidden rounded-3xl"
              initial={{ width: "2.5rem", height: "20rem" }}
              animate={{
                width: activeImage === index ? "24rem" : "5rem",
                height: activeImage === index ? "24rem" : "24rem",
              }}
              transition={{ duration: 0.3, ease: "easeInOut" }}
              onClick={() => setActiveImage(index)}
              onHoverStart={() => setActiveImage(index)}
            >
              <AnimatePresence>
                {activeImage === index && (
                  <motion.div
                    initial={{ opacity: 0 }}
                    animate={{ opacity: 1 }}
                    exit={{ opacity: 0 }}
                    className="absolute h-full w-full bg-gradient-to-t from-black/40 to-transparent"
                  />
                )}
              </AnimatePresence>
              <AnimatePresence>
                {activeImage === index && (
                  <motion.div
                    initial={{ opacity: 0 }}
                    animate={{ opacity: 1 }}
                    exit={{ opacity: 0 }}
                    className="absolute flex h-full w-full flex-col items-end justify-end p-4"
                  >
                    <p className="text-left text-xs text-white/50">
                      {image.code}
                    </p>
                  </motion.div>
                )}
              </AnimatePresence>
              <img
                src={image.src}
                className="size-full object-cover"
                alt={image.alt}
              />
            </motion.div>
          ))}
        </div>
      </motion.div>
    </motion.div>
  );
};

export { HoverExpand_001 };

/**
 * Skiper 52 HoverExpand_001 — React + Framer Motion
 * Illustrations by AarzooAly - https://x.com/AarzooAly
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### ExpandOnHover vertical <a name="expandonhover-vertical"></a>

A beautiful vertical hover expand gallery with smooth animations and interactive effects. Features a stunning vertical expand effect that creates depth and visual interest with hover interactions and customizable options.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper53
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`

#### How to Use

```tsx
import { HoverExpand_002 } from "@/components/v1/skiper53";

// Using the hover expand component with custom images
const CustomHoverExpand = () => {
  const images = [
    {
      src: "/path/to/image1.jpg",
      alt: "Description 1",
      code: "# 01",
    },
    {
      src: "/path/to/image2.jpg",
      alt: "Description 2",
      code: "# 02",
    },
    // ... more images
  ];

  return <HoverExpand_002 images={images} className="" />;
};
```

#### Component Source Code

##### File: `skiper53.tsx`

```tsx
"use client";

import { AnimatePresence, motion } from "framer-motion";
import React, { useState } from "react";
import "swiper/css";
import "swiper/css/effect-creative";
import "swiper/css/pagination";
import "swiper/css/autoplay";

import { cn } from "@/lib/utils";

const Skiper53 = () => {
  const images = [
    {
      src: "/images/x.com/13.jpeg",
      alt: "Illustrations by my fav AarzooAly",
      code: "# 23",
    },
    {
      src: "/images/x.com/9.jpeg",
      alt: "Illustrations by ©AarzooAly",
      code: "# 23",
    },
    {
      src: "/images/x.com/20.jpeg",
      alt: "Illustrations by ©AarzooAly",
      code: "# 23",
    },
    {
      src: "/images/x.com/21.jpeg",
      alt: "Illustrations by ©AarzooAly",
      code: "# 23",
    },
    {
      src: "/images/x.com/25.jpeg",
      alt: "Illustrations by ©AarzooAly",
      code: "# 23",
    },

    {
      src: "/images/x.com/32.jpeg",
      alt: "Illustrations by ©AarzooAly",
      code: "# 23",
    },
  ];

  return (
    <div className="flex h-full w-full items-center justify-center overflow-hidden bg-[#f5f4f3]">
      <HoverExpand_002 className="" images={images} />
    </div>
  );
};

export { Skiper53 };

const HoverExpand_002 = ({
  images,
  className,
}: {
  images: { src: string; alt: string; code: string }[];
  className?: string;
}) => {
  const [activeImage, setActiveImage] = useState<number | null>(1);

  return (
    <motion.div
      initial={{ opacity: 0, translateY: 20 }}
      animate={{ opacity: 1, translateY: 0 }}
      transition={{
        duration: 0.3,
        delay: 0.5,
      }}
      className={cn("relative w-full max-w-6xl px-5", className)}
    >
      <motion.div
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        transition={{ duration: 0.3 }}
        className="w-full"
      >
        <div className="flex w-full flex-col items-center justify-center gap-1">
          {images.map((image, index) => (
            <motion.div
              key={index}
              className="group relative cursor-pointer overflow-hidden rounded-3xl"
              initial={{ height: "2.5rem", width: "24rem" }}
              animate={{
                height: activeImage === index ? "24rem" : "2.5rem",
              }}
              transition={{ duration: 0.3, ease: "easeInOut" }}
              onClick={() => setActiveImage(index)}
              onHoverStart={() => setActiveImage(index)}
            >
              <AnimatePresence>
                {activeImage === index && (
                  <motion.div
                    initial={{ opacity: 0 }}
                    animate={{ opacity: 1 }}
                    exit={{ opacity: 0 }}
                    className="absolute h-full w-full bg-gradient-to-t from-black/50 to-transparent"
                  />
                )}
              </AnimatePresence>
              <AnimatePresence>
                {activeImage === index && (
                  <motion.div
                    initial={{ opacity: 0, y: 20 }}
                    animate={{ opacity: 1, y: 0 }}
                    exit={{ opacity: 0, y: 20 }}
                    className="absolute flex h-full w-full flex-col items-end justify-end px-4 pb-5"
                  >
                    <p className="text-left text-xs text-white/50">
                      {image.code}
                    </p>
                  </motion.div>
                )}
              </AnimatePresence>
              <img
                src={image.src}
                className="size-full object-cover"
                alt={image.alt}
              />
            </motion.div>
          ))}
        </div>
      </motion.div>
    </motion.div>
  );
};

export { HoverExpand_002 };

/**
 * Skiper 53 HoverExpand_002 — React + Framer Motion
 * Illustrations by AarzooAly - https://x.com/AarzooAly
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

## Development tools <a name="development-tools"></a>

### Breakpoint indicator <a name="breakpoint-indicator"></a>

Development tool showing current Tailwind CSS breakpoint for responsive design. Features a visual indicator that displays the current breakpoint and helps with responsive development.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper65
```

#### How to Use

```tsx
import { BreakpointIndicator } from "@/components/v1/skiper65";

const Layout = ({ children }) => {
  return (
    <main>
      <BreakpointIndicator className="fixed bottom-0 left-0" />
      {children}
    </main>
  );
};
```

#### Component Source Code

##### File: `skiper65.tsx`

```tsx
import { cn } from "@/lib/utils";

const Skiper65 = () => {
  return (
    <div className="flex h-full w-full flex-col items-center justify-center">
      <div className="mb-40 grid content-start justify-items-center gap-6 text-center">
        <span className="after:to-foreground relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-transparent after:content-['']">
          minimal Breakpoint indicator
        </span>
      </div>
      <BreakpointIndicator />
    </div>
  );
};

const BreakpointIndicator = ({ className }: { className?: string }) => {
  if (process.env.NODE_ENV === "production") return null;
  return (
    <div
      className={cn(
        "bottom-1 left-1 z-50 flex size-6 items-center justify-center rounded-full bg-black p-2 font-mono text-xs text-white",
        className,
      )}
    >
      <div className="block sm:hidden">xs</div>
      <div className="hidden sm:block md:hidden">sm</div>
      <div className="hidden md:block lg:hidden">md</div>
      <div className="hidden lg:block xl:hidden">lg</div>
      <div className="hidden xl:block 2xl:hidden">xl</div>
      <div className="hidden 2xl:block">2xl</div>
    </div>
  );
};

export { BreakpointIndicator, Skiper65 };

```

---

### Debug Pannel <a name="debug-pannel"></a>

A simple debug component that accepts any object and prints it out on the screen, best to debug your code, instead of using console.logs everywhere for the interaction try this paste any prop and see the live plus it also accepts framer motion values so you dont need to render this component if the type changes

#### Installation

```bash
npx shadcn add @skiper-ui/skiper102
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`

#### How to Use

```tsx
import { Skiper102, DebugPanel } from "@/components/v1/skiper102";
import { useMotionValue } from "motion/react";
import { useState } from "react";

// Using the demo component
const DemoSkiper102 = () => {
  return (
    <div className="h-screen w-full">
      <Skiper102 />
    </div>
  );
};

// Using DebugPanel directly
const CustomDebug = () => {
  const mouseX = useMotionValue(0);
  const mouseY = useMotionValue(0);
  const [count, setCount] = useState(0);

  return (
    <div
      onMouseMove={(e) => {
        mouseX.set(e.clientX);
        mouseY.set(e.clientY);
      }}
      onClick={() => setCount((prev) => prev + 1)}
    >
      <DebugPanel mouseX={mouseX} mouseY={mouseY} count={count} />
    </div>
  );
};
```

#### Component Source Code

##### File: `skiper102.tsx`

```tsx
"use client";

import { useMotionValue } from "framer-motion";
import { motion } from "motion/react";
import { useEffect, useState } from "react";

import { cn } from "@/lib/utils";

export const Skiper102 = () => {
  const mouseX = useMotionValue(0);
  const mouseY = useMotionValue(0);

  const [count, setCount] = useState(0);

  const [keyPressed, setKeyPressed] = useState("");

  useEffect(() => {
    const handleKeyDown = (e: KeyboardEvent) => {
      setKeyPressed(e.key);
    };
    window.addEventListener("keydown", handleKeyDown);
    return () => window.removeEventListener("keydown", handleKeyDown);
  }, []);

  return (
    <div
      onMouseMove={(e) => {
        mouseX.set(e.clientX);
        mouseY.set(e.clientY);
      }}
      onClick={() => setCount((prev) => prev + 1)}
      className="flex size-full flex-col items-center justify-center"
    >
      <div className="-mt-36 mb-36 grid content-start justify-items-center gap-6 text-center">
        <span className="after:to-foreground relative max-w-[14ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-transparent after:content-['']">
          Debug Pannel
        </span>
      </div>
      <DebugPanel
        className=""
        count={count}
        mouseX={mouseX}
        mouseY={mouseY}
        keyPressed={keyPressed}
      />
    </div>
  );
};

export const DebugPanel = ({
  className,
  ...props
}: Record<string, any> & { className?: string }) => {
  return (
    <div
      className={cn(
        "z-99 left-4 top-4 font-mono text-sm text-red-500",
        className,
      )}
    >
      {"{"}

      {Object.entries(props).map(([key, value]) => (
        <div key={key} className="ml-4">
          {key}:{" "}
          {value && typeof value === "object" && "get" in value ? (
            <motion.span>{value}</motion.span>
          ) : typeof value === "boolean" ? (
            value ? (
              "true"
            ) : (
              "false"
            )
          ) : (
            String(value)
          )}
          ;
        </div>
      ))}
      {"}"}
    </div>
  );
};

```

---

### Loop animation hook <a name="loop-animation-hook"></a>

Custom hook for creating looping animations with configurable timing and transitions. Features a reusable hook that handles animation loops with customizable duration and easing.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper62
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`

#### How to Use

```tsx
import { useLoop } from "@/components/v1/skiper62";

const CustomLoopExample = () => {
  const { key } = useLoop(2000); // 2 second delay

  const items = ["Item 1", "Item 2", "Item 3"];
  const currentItem = items[key % items.length];

  return (
    <div className="flex h-screen items-center justify-center">
      <motion.div
        key={key}
        initial={{ opacity: 0, scale: 0.8 }}
        animate={{ opacity: 1, scale: 1 }}
        exit={{ opacity: 0, scale: 1.2 }}
        transition={{ duration: 0.5 }}
      >
        {currentItem}
      </motion.div>
    </div>
  );
};
```

#### Component Source Code

##### File: `skiper62.tsx`

```tsx
"use client";

import { AnimatePresence, motion } from "framer-motion";
import { useCallback, useEffect, useMemo, useState } from "react";

const useLoop = (delay = 1000) => {
  const [key, setKey] = useState(0);

  const incrementKey = useCallback(() => {
    setKey((prev) => prev + 1);
  }, []);

  useEffect(() => {
    const interval = setInterval(incrementKey, delay);
    return () => clearInterval(interval);
  }, [delay, incrementKey]);

  return { key };
};

export { useLoop };

const Skiper62 = () => {
  const { key } = useLoop();

  const array = useMemo(
    () => [
      "Tik-Tik uno",
      "Tik-Tik dos",
      "Tik-Tik tres",
      "Tik-Tik cuatro",
      "Tik-Tik cinco",
      "Tik-Tik seis",
      "Tik-Tik siete",
      "Tik-Tik ocho",
      "Tik-Tik nueve",
      "Tik-Tik diez",
    ],
    [],
  );

  const currentItem = useMemo(() => {
    return array[key % array.length];
  }, [array, key]);

  return (
    <div className="flex flex-col items-center justify-center gap-8 p-8">
      <div className="mb-20 grid content-start justify-items-center gap-6 text-center">
        <span className="after:to-foreground relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-transparent after:content-['']">
          useLoop hook
        </span>
      </div>
      <AnimatePresence mode="popLayout">
        <motion.h1
          key={key}
          initial={{ opacity: 0, y: " 100%" }}
          animate={{ opacity: 1, y: 0 }}
          exit={{ opacity: 0, y: "-100%" }}
          transition={{ duration: 0.3 }}
          className="bordr whitespace-nowrap text-center"
        >
          {currentItem}
        </motion.h1>
      </AnimatePresence>
    </div>
  );
};

export { Skiper62 };

```

---

## Minimal interactions <a name="minimal-interactions"></a>

### Animated icons 002 <a name="animated-icons-002"></a>

Collection of animated icons with smooth transitions and interactive states. more comming soon

#### Installation

```bash
npx shadcn add @skiper-ui/skiper99
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
  - `lucide-react`

#### How to Use

```tsx
import {
  Skiper99,
  ArrowIcon,
  MenuIcon,
  VolumeIcon,
} from "@/components/v1/skiper99";

const DemoSkiper99 = () => {
  return (
    <div className="flex gap-2">
      <div className="size-22 bg-background rounded-3xl">
        <ArrowIcon />
      </div>
      <div className="size-22 bg-background rounded-3xl">
        <MenuIcon />
      </div>
      <div className="size-22 bg-background rounded-3xl">
        <VolumeIcon />
      </div>
    </div>
  );
};
```

#### Component Source Code

##### File: `skiper99.tsx`

```tsx
"use client";

import { motion } from "framer-motion";
import { ChevronRight } from "lucide-react";
import React, { useState } from "react";

import { cn } from "@/lib/utils";

const Skiper99 = () => {
  return (
    <div className="bg-muted flex h-full w-full flex-col items-center justify-center">
      <div className="grid content-start justify-items-center gap-6 text-center">
        <span className="after:from-background after:to-foreground relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:content-['']">
          Try Hover/Click the icons
        </span>
      </div>
      <div className="mt-25 flex w-full max-w-3xl flex-wrap items-center justify-center gap-2 px-5 text-justify">
        <div className="size-22 bg-background rounded-3xl">
          <ArrowIcon />
        </div>
        <div className="size-22 bg-background rounded-3xl">
          <MenuIcon />
        </div>
        <div className="size-22 bg-background rounded-3xl">
          <VolumeIcon />
        </div>
      </div>
    </div>
  );
};

export { Skiper99 };

const ArrowIcon = ({ className }: { className?: string }) => {
  return (
    <div
      className={cn(
        "group flex size-full cursor-pointer items-center justify-center",
        className,
      )}
    >
      <div className="relative grid cursor-pointer items-center justify-center">
        <ChevronRight className="transition-all duration-500 ease-out group-hover:translate-x-0.5" />
        <div className="absolute right-[9px] h-[2px] w-3 origin-right scale-x-0 rounded-[1px] bg-current transition-all duration-300 ease-out group-hover:right-[7px] group-hover:scale-x-100"></div>
      </div>
    </div>
  );
};

const MenuIcon = ({ className }: { className?: string }) => {
  const [toggle, setToggle] = useState(false);

  return (
    <div
      onClick={() => setToggle((x) => !x)}
      className={cn(
        "group flex size-full cursor-pointer items-center justify-center",
        className,
      )}
    >
      <div className="relative grid size-4 cursor-pointer items-center justify-center">
        <motion.div
          animate={{ y: toggle ? 0 : "-5px", rotate: toggle ? 45 : 0 }}
          className="absolute h-0.5 w-full rounded-full bg-current"
        ></motion.div>
        <motion.div
          animate={{ opacity: toggle ? 0 : 1 }}
          transition={{ duration: 0.1 }}
          className="absolute h-0.5 w-full rounded-full bg-current"
        ></motion.div>
        <motion.div
          animate={{ y: toggle ? 0 : "5px", rotate: toggle ? -45 : 0 }}
          className="absolute h-0.5 w-full rounded-full bg-current"
        ></motion.div>
      </div>
    </div>
  );
};

const VolumeIcon = ({ className }: { className?: string }) => {
  const [isMuted, setIsMuted] = useState(false);

  return (
    <div
      onClick={() => setIsMuted((x) => !x)}
      className={cn(
        "group flex size-full cursor-pointer items-center justify-center",
        className,
      )}
    >
      <motion.div
        initial={false}
        className="relative flex size-5 items-center justify-center"
        animate={{
          rotate: isMuted ? [0, -15, 5, -2, 0] : "none",
        }}
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
          className={cn("", className)}
        >
          <path
            fill="currentColor"
            stroke="none"
            d="M11 4.702a.705.705 0 0 0-1.203-.498L6.413 7.587A1.4 1.4 0 0 1 5.416 8H3a1 1 0 0 0-1 1v6a1 1 0 0 0 1 1h2.416a1.4 1.4 0 0 1 .997.413l3.383 3.384A.705.705 0 0 0 11 19.298z"
          />

          <motion.g>
            <path
              fill="none"
              strokeWidth={2}
              strokeLinecap="round"
              strokeLinejoin="round"
              stroke="currentColor"
              d="M16 9a5 5 0 0 1 0 6"
            />
            <path
              fill="none"
              strokeWidth={2}
              strokeLinecap="round"
              strokeLinejoin="round"
              stroke="currentColor"
              d="M19.364 18.364a9 9 0 0 0 0-12.728"
            />
          </motion.g>
        </svg>
        <div className="absolute inset-0 flex items-center justify-center">
          <div className="rotate-[-40deg] overflow-hidden">
            <motion.div
              animate={{ scaleY: isMuted ? 1 : 0 }}
              transition={{
                ease: "easeInOut",
                duration: isMuted ? 0.125 : 0.05,
                delay: isMuted ? 0.15 : 0,
              }}
              style={{
                transformOrigin: "top",
              }}
              className="h-[18px] w-fit rounded-full"
            >
              <div className="bg-background flex h-full w-[3.5px] items-center justify-center rounded-full">
                <div className="bg-foreground h-full w-[1.5px] rounded-full" />
              </div>
            </motion.div>
          </div>
        </div>
      </motion.div>
    </div>
  );
};

```

---

### Animated number <a name="animated-number"></a>

A collection of animated number components featuring countdown timers, random number generators, and smooth value transitions. Built with Number Flow and Framer Motion for engaging user experiences.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper37
```

#### Dependencies

- **NPM Packages**:
  - `@number-flow/react`
  - `framer-motion`
  - `lucide-react`
  - `react-intersection-observer`

#### How to Use

```tsx
import {
  AnimatedNumber_001,
  AnimatedNumber_002,
  AnimatedNumber_003,
  AnimatedNumber_004,
} from "@/components/v1/skiper37";

// Countdown timer with pause/reset controls
const CountdownDemo = () => {
  return <AnimatedNumber_001 />;
};

// Spring-based number animation
const SpringDemo = () => {
  return <AnimatedNumber_002 />;
};

// Random number generator
const RandomDemo = () => {
  return <AnimatedNumber_003 />;
};

// while in view + Number Flow
const NumberFlowDemo = () => {
  return <AnimatedNumber_004 />;
};
```

#### Component Source Code

##### File: `skiper37.tsx`

```tsx
"use client";

import NumberFlow from "@number-flow/react";
import { AnimatePresence, motion, useSpring } from "framer-motion";
import { animate, useMotionValue } from "framer-motion";
import { Plus } from "lucide-react";
import React, { useEffect, useRef, useState } from "react";
import { useInView } from "react-intersection-observer";

const Skiper37 = () => {
  return (
    <section className="relative h-[calc(100vh-1rem)] w-full snap-y snap-mandatory overflow-y-scroll bg-[#f5f4f3]">
      <div className="snap-start">
        <AnimatedNumber_001 />
      </div>
      <div className="snap-start">
        <AnimatedNumber_002 />
      </div>
      <div className="snap-start">
        <AnimatedNumber_003 />
      </div>
      <div className="snap-start">
        <AnimatedNumber_004 />
      </div>
    </section>
  );
};

const AnimatedNumber_001 = () => {
  const [isPaused, setIsPaused] = useState(false);
  const [resetTrigger, setResetTrigger] = useState(0);

  const [count, setCount] = useState(60);

  useEffect(() => {
    if (isPaused) return;

    const id = setInterval(() => {
      setCount((c) => {
        if (c === 0) {
          return 60;
        }
        return c - 1;
      });
    }, 1000);

    return () => {
      clearInterval(id);
    };
  }, [isPaused]);

  // Reset timer when resetTrigger changes
  useEffect(() => {
    setCount(60);
  }, [resetTrigger]);

  const handleReset = () => {
    setResetTrigger((prev) => prev + 1);
  };

  return (
    <div className="relative flex h-screen w-full flex-col items-center justify-center bg-[#f5f4f3] text-black">
      <div className="top-22 absolute left-1/2 grid -translate-x-1/2 content-start justify-items-center gap-6 text-center text-black">
        <span className="relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-[#f5f4f3] after:to-black after:content-['']">
          Countdown with Number Flow
        </span>
      </div>
      <div className="font-bebas-neue text-[20vw] tracking-tight">
        <NumberFlow value={count} prefix="0:" />
      </div>
      <div className="flex w-fit items-center gap-2">
        <motion.button
          aria-label="Pause timer"
          onClick={() => setIsPaused((p) => !p)}
          whileTap={{ scale: 0.9 }}
          className="hover:bg-[#ff3828 flex h-10 w-10 items-center justify-center rounded-full bg-[#ff3828] transition-colors"
        >
          <AnimatePresence initial={false} mode="wait">
            {isPaused ? (
              <motion.svg
                key="play"
                initial={{ opacity: 0, scale: 0.5, filter: "blur(4px)" }}
                animate={{ opacity: 1, scale: 1, filter: "blur(0px)" }}
                exit={{ opacity: 0, scale: 0.5, filter: "blur(4px)" }}
                transition={{ duration: 0.1 }}
                viewBox="0 0 12 14"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
                className="h-4 w-4 fill-current text-[#f5f4f3]"
              >
                <path d="M0.9375 13.2422C1.25 13.2422 1.51562 13.1172 1.82812 12.9375L10.9375 7.67188C11.5859 7.28906 11.8125 7.03906 11.8125 6.625C11.8125 6.21094 11.5859 5.96094 10.9375 5.58594L1.82812 0.3125C1.51562 0.132812 1.25 0.015625 0.9375 0.015625C0.359375 0.015625 0 0.453125 0 1.13281V12.1172C0 12.7969 0.359375 13.2422 0.9375 13.2422Z" />
              </motion.svg>
            ) : (
              <motion.svg
                key="pause"
                initial={{ opacity: 0, scale: 0.5, filter: "blur(4px)" }}
                animate={{ opacity: 1, scale: 1, filter: "blur(0px)" }}
                exit={{ opacity: 0, scale: 0.5, filter: "blur(4px)" }}
                transition={{ duration: 0.1 }}
                viewBox="0 0 10 13"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
                className="h-4 w-4 fill-current text-[#f5f4f3]"
              >
                <path d="M1.03906 12.7266H2.82031C3.5 12.7266 3.85938 12.3672 3.85938 11.6797V1.03906C3.85938 0.328125 3.5 0 2.82031 0H1.03906C0.359375 0 0 0.359375 0 1.03906V11.6797C0 12.3672 0.359375 12.7266 1.03906 12.7266ZM6.71875 12.7266H8.49219C9.17969 12.7266 9.53125 12.3672 9.53125 11.6797V1.03906C9.53125 0.328125 9.17969 0 8.49219 0H6.71875C6.03125 0 5.67188 0.359375 5.67188 1.03906V11.6797C5.67188 12.3672 6.03125 12.7266 6.71875 12.7266Z" />
              </motion.svg>
            )}
          </AnimatePresence>
        </motion.button>
        <button
          aria-label="Reset timer"
          onClick={handleReset}
          className="flex h-10 w-10 items-center justify-center rounded-full bg-white/45 text-[#ff3828] shadow-2xl transition-colors hover:bg-white/70"
        >
          <Plus className="rotate-45" />
        </button>
      </div>
    </div>
  );
};

export { Skiper37 };

export const AnimatedNumber_002 = () => {
  const finalCount = 500;
  const [displaySubs, setDisplaySubs] = useState(0);

  // Animating sub count from 0 to subscriberCount prop
  const springSubCount = useSpring(0, {
    bounce: 0,
    duration: 1000,
  });

  springSubCount.on("change", (value) => {
    setDisplaySubs(Math.round(value));
  });

  const animate = () => {
    springSubCount.set(finalCount);
  };

  return (
    <div className="relative flex h-screen w-full flex-col items-center justify-center bg-[#f5f4f3] text-black">
      <div className="top-22 absolute left-1/2 grid -translate-x-1/2 content-start justify-items-center gap-6 text-center text-black">
        <span className="relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-[#f5f4f3] after:to-black after:content-['']">
          random numbers from x to y in view
        </span>
      </div>
      <motion.div
        onViewportEnter={animate}
        onViewportLeave={() => {
          springSubCount.set(0);
        }}
        className="font-bebas-neue text-[20vw] tracking-tight"
      >
        {displaySubs}
      </motion.div>
    </div>
  );
};

export const AnimatedNumber_003 = () => {
  const [displayNumber, setDisplayNumber] = useState(1000000);
  const [isAnimating, setIsAnimating] = useState(false);
  const hasAnimated = useRef(false);

  const formatNumber = (num: any) => {
    return new Intl.NumberFormat("en-US").format(num);
  };

  const animate = () => {
    if (hasAnimated.current || isAnimating) return;

    setIsAnimating(true);
    hasAnimated.current = true;

    const steps = 12;
    let currentStep = 0;

    const interval = setInterval(() => {
      currentStep++;

      if (currentStep <= steps) {
        const min = 1000000 + currentStep * (1000000 / steps);
        const max = 2200000;
        const randomNum = Math.floor(min + Math.random() * (max - min));
        setDisplayNumber(randomNum);
      } else {
        setDisplayNumber(2146000);
        setIsAnimating(false);
        clearInterval(interval);
      }
    }, 80);
  };
  return (
    <div className="relative flex h-screen w-full flex-col items-center justify-center bg-[#f5f4f3] text-black">
      <div className="top-22 absolute left-1/2 grid -translate-x-1/2 content-start justify-items-center gap-6 text-center text-black">
        <span className="relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-[#f5f4f3] after:to-black after:content-['']">
          random numbers from x to y in view
        </span>
      </div>
      <div className="font-bebas-neue text-[20vw] tracking-tight">
        <motion.div
          onViewportEnter={animate}
          onViewportLeave={() => {
            setDisplayNumber(1000000);
            hasAnimated.current = false;
            setIsAnimating(false);
          }}
        >
          ${formatNumber(displayNumber)}
        </motion.div>
      </div>
    </div>
  );
};

function AnimatedNumber_004() {
  const [displayValue, setDisplayValue] = useState(0);
  const count = useMotionValue(3);
  const { ref, inView } = useInView({ triggerOnce: false });

  useEffect(() => {
    if (inView) {
      animate(count, 100, {
        duration: 1,
        ease: "easeInOut",
        onUpdate: (latest) => setDisplayValue(Math.round(latest)),
        onComplete: () => {
          console.log("complete");
        },
      });
    } else {
      setDisplayValue(3);
    }
  }, [inView, count]);

  return (
    <div className="relative flex h-screen w-full flex-col items-center justify-center bg-[#f5f4f3] text-black">
      <div className="top-22 absolute left-1/2 grid -translate-x-1/2 content-start justify-items-center gap-6 text-center text-black">
        <span className="relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-[#f5f4f3] after:to-black after:content-['']">
          from x to y value in view [number-flow]
        </span>
      </div>
      <div ref={ref} className="font-bebas-neue text-[20vw] tracking-tight">
        <NumberFlow value={displayValue} prefix="$" suffix="K USD" />
      </div>
    </div>
  );
}

/**
 * Skiper 37 AnimatedNumber — React + Number Flow + Framer Motion
 * Design Inspired by https://number-flow.barvian.me/
 * Code Inspired by https://number-flow.barvian.me/
 * We respect the original creators. This is an inspired rebuild with our own taste and does not claim any ownership.
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### Apple play button <a name="apple-play-button"></a>

Elegant play button with Apple-inspired design and smooth hover animations. Features a dynamic toggle that expands from a circular button to a full-width player interface with smooth spring animations and glassmorphism effects.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper3
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`

#### How to Use

```tsx
const DemoSkiper3 = () => {
  const [toggle, setToggle] = useState(false);

  return (
    <div className="flex h-full w-full items-center justify-center bg-[#121212]">
      <motion.div layout>
        <motion.div
          className="relative flex items-center justify-between overflow-hidden"
          style={{ borderRadius: 9999, width: 60 }}
          initial={{ scale: 0, y: "100%" }}
          animate={{ scale: 1, y: 0, width: !toggle ? 60 : 330 }}
          transition={{ type: "spring", bounce: 0.16 }}
        >
          {/* Your play button content */}
        </motion.div>
      </motion.div>
      <button onClick={() => setToggle((x) => !x)}>Toggle</button>
    </div>
  );
};
```

#### Component Source Code

##### File: `skiper3.tsx`

```tsx
"use client";

import { motion } from "framer-motion";
import React, { useState } from "react";

import { cn } from "@/lib/utils";

const Skiper3 = () => {
  const [toggle, setToggle] = useState(false);

  return (
    <div className="flex h-full w-full items-center justify-center rounded-full">
      <motion.div layout>
        <motion.div
          className={cn(
            "h-15 relative flex items-center justify-between overflow-hidden rounded-full",
          )}
          style={{ borderRadius: 9999, width: 60 }}
          initial={{ scale: 0, y: "100%" }}
          transition={{ type: "spring", bounce: 0.16 }}
          animate={{ scale: 1, y: 0, width: !toggle ? 60 : 330 }}
        >
          <div className="bg-foreground/20 flex h-full w-[260px] items-center justify-center gap-2 rounded-full">
            {toggle && (
              <motion.div
                animate={{ opacity: 1 }}
                initial={{ opacity: 0 }}
                exit={{ opacity: 0 }}
                transition={{ delay: 0.25 }}
                className="flex items-center justify-center gap-2"
              >
                <span className="bg-foreground/20 h-[10px] w-[60px] rounded-full" />
                <span className="bg-foreground/20 size-[10px] rounded-full" />
                <span className="bg-foreground/20 size-[10px] rounded-full" />
                <span className="bg-foreground/20 size-[10px] rounded-full" />
                <span className="bg-foreground/20 size-[10px] rounded-full" />
              </motion.div>
            )}
          </div>
          {toggle && (
            <div className="bg-foreground/20 flex h-full w-[60px] items-center justify-center gap-2 rounded-full">
              <motion.div
                initial={{ opacity: 0, scale: 0.5, filter: "blur(4px)" }}
                animate={{ opacity: 1, scale: 1, filter: "blur(0px)" }}
                exit={{ opacity: 0, scale: 0.5, filter: "blur(4px)" }}
                className="flex items-center justify-center gap-2"
              >
                <motion.svg
                  key="play"
                  initial={{ opacity: 0, scale: 0.5, filter: "blur(4px)" }}
                  animate={{ opacity: 1, scale: 1, filter: "blur(0px)" }}
                  exit={{ opacity: 0, scale: 0.5, filter: "blur(4px)" }}
                  transition={{ delay: 0.25 }}
                  viewBox="-1 0 12 14"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                  className="text-foreground/8 h-5 w-5 fill-current"
                >
                  <path d="M0.9375 13.2422C1.25 13.2422 1.51562 13.1172 1.82812 12.9375L10.9375 7.67188C11.5859 7.28906 11.8125 7.03906 11.8125 6.625C11.8125 6.21094 11.5859 5.96094 10.9375 5.58594L1.82812 0.3125C1.51562 0.132812 1.25 0.015625 0.9375 0.015625C0.359375 0.015625 0 0.453125 0 1.13281V12.1172C0 12.7969 0.359375 13.2422 0.9375 13.2422Z" />
                </motion.svg>
              </motion.div>
            </div>
          )}
        </motion.div>
      </motion.div>
      <button
        onClick={() => setToggle((x) => !x)}
        className="bg-foreground/8 absolute bottom-10 my-10 rounded-full px-7 py-1 active:scale-95"
      >
        Toggle
      </button>
    </div>
  );
};

export { Skiper3 };

/**
 * Dynamic Toggle button Component — v1.0.0
 * Built with Motion fo rounded-full React
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper rounded-full UI is required when using the free version.
 * - No attribution required with Skiper rounded-full UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### Apple squicircle effect <a name="apple-squicircle-effect"></a>

use the famous apple squicircle effect on any element, make them feel more than just rounded corners with just one line of code and no extra dependencies

#### Installation

```bash
npx shadcn add @skiper-ui/skiper63
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
  - `lucide-react`

#### How to Use

```tsx
// layout.tsx or component parent just need to use the div outside the element
import {
  SquiCircleFilterStatic,
  SquiCircleFilter,
} from "@/components/v1/skiper63";

const Layout = ({ children }) => {
  return (
    <div>
      <SquiCircleFilter blurValue={10} colorMatrixValue={20} alphaValue={-7} />
      {children}
    </div>
  );
};
```

#### Component Source Code

##### File: `skiper63.tsx`

```tsx
"use client";

import { motion } from "framer-motion";
import { GripHorizontal, RefreshCcw } from "lucide-react";
import React, { useState } from "react";

import { cn } from "@/lib/utils";

// to use the filter just add this to your layout.tsx
// <SkiperSquiCircleFilterLayout/>
// {children}

// on element you need to add squicircle just add the filter id SkiperSquiCircleFilter
//<div style={{filter: "url(#SquiCircleFilter)"}}></div>

// thats it you can use the filter now no extra rerenders no complications just pure css filter

export const SquiCircleFilterStatic = () => {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      className="absolute bottom-0 left-0"
      version="1.1"
    >
      <defs>
        <filter id="SkiperSquiCircleFilterLayout">
          <feGaussianBlur in="SourceGraphic" stdDeviation="10" result="blur" />
          <feColorMatrix
            in="blur"
            mode="matrix"
            values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 20 -7"
            result="goo"
          />
          <feBlend in="SourceGraphic" in2="goo" />
        </filter>
      </defs>
    </svg>
  );
};

// ------------------------------------------------------------
// use this to toggle the values no need to use this anywhere else
// else bcz it will just add more reRenders and you probably wont need that
// ------------------------------------------------------------

const Skiper63 = () => {
  const [toggle, setToggle] = useState(true);
  const [height, setHeight] = useState(200);
  const [width, setWidth] = useState(300);
  const [blurValue, setBlurValue] = useState(10);
  const [colorMatrixValue, setColorMatrixValue] = useState(20);
  const [alphaValue, setAlphaValue] = useState(-7);

  return (
    <div className="flex h-full w-full flex-col items-center justify-center">
      <div className="mb-20 grid content-start justify-items-center gap-6 text-center">
        <span className="after:to-foreground relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-transparent after:content-['']">
          squicircle with svg filter
        </span>
      </div>
      <SquiCircleFilter
        blurValue={blurValue}
        colorMatrixValue={colorMatrixValue}
        alphaValue={alphaValue}
      />
      <Options
        toggle={toggle}
        setToggle={setToggle}
        height={height}
        setHeight={setHeight}
        width={width}
        setWidth={setWidth}
        blurValue={blurValue}
        setBlurValue={setBlurValue}
        colorMatrixValue={colorMatrixValue}
        setColorMatrixValue={setColorMatrixValue}
        alphaValue={alphaValue}
        setAlphaValue={setAlphaValue}
      />
      <div
        className="bg-foreground rounded-2xl"
        style={{
          height: `${height}px`,
          width: `${width}px`,
          filter: toggle ? "url(#SquiCircleFilter)" : "none",
        }}
      ></div>
    </div>
  );
};

export { Skiper63 };

const SquiCircleFilter = ({
  blurValue = 10,
  colorMatrixValue = 20,
  alphaValue = -7,
}: {
  blurValue?: number;
  colorMatrixValue?: number;
  alphaValue?: number;
}) => {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      className="absolute bottom-0 left-0"
      version="1.1"
    >
      <defs>
        <filter id="SquiCircleFilter">
          <feGaussianBlur
            in="SourceGraphic"
            stdDeviation={blurValue}
            result="blur"
          />
          <feColorMatrix
            in="blur"
            mode="matrix"
            values={`1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 ${colorMatrixValue} ${alphaValue}`}
            result="goo"
          />
          <feBlend in="SourceGraphic" in2="goo" />
        </filter>
      </defs>
    </svg>
  );
};

const Options = ({
  toggle,
  setToggle,
  height,
  setHeight,
  width,
  setWidth,
  blurValue,
  setBlurValue,
  colorMatrixValue,
  setColorMatrixValue,
  alphaValue,
  setAlphaValue,
}: {
  toggle: boolean;
  setToggle: (value: boolean) => void;
  height: number;
  setHeight: (value: number) => void;
  width: number;
  setWidth: (value: number) => void;
  blurValue: number;
  setBlurValue: (value: number) => void;
  colorMatrixValue: number;
  setColorMatrixValue: (value: number) => void;
  alphaValue: number;
  setAlphaValue: (value: number) => void;
}) => {
  const [isDragging, setIsDragging] = useState(false);

  const resetValues = () => {
    setToggle(false);
    setHeight(200);
    setWidth(300);
    setBlurValue(10);
    setColorMatrixValue(20);
    setAlphaValue(-7);
  };

  return (
    <motion.div
      className="top-30 border-foreground/10 bg-muted2 absolute right-1/2 flex w-[300px] translate-x-1/2 flex-col gap-3 rounded-3xl border p-3 backdrop-blur-sm lg:right-4 lg:translate-x-0"
      drag={isDragging}
      dragMomentum={false}
    >
      <div className="flex items-center justify-between">
        <span
          onPointerDown={() => setIsDragging(true)}
          onPointerUp={() => setIsDragging(false)}
          className="size-4 cursor-grab active:cursor-grabbing"
        >
          <GripHorizontal className="size-4 opacity-50" />
        </span>

        <p
          onClick={resetValues}
          className="hover:bg-foreground/10 group flex cursor-pointer items-center justify-center gap-2 rounded-lg px-2 py-1 text-sm opacity-50"
        >
          Reset
          <span className="group-active:-rotate-360 rotate-0 cursor-pointer transition-all duration-300 group-hover:rotate-90">
            <RefreshCcw className="size-4 opacity-50" />
          </span>
        </p>
      </div>

      <div className="flex w-full flex-col gap-3">
        {/* Toggle Control */}
        <div className="grid grid-cols-3 items-center gap-2 py-1">
          <p className="text-sm opacity-50">Filter :</p>
          <button
            onClick={() => setToggle(true)}
            className={cn(
              "bg-muted3 flex items-center justify-center rounded-lg py-1 text-left text-xs opacity-25 transition-colors",
              toggle && "opacity-100",
            )}
          >
            ON
          </button>
          <button
            onClick={() => setToggle(false)}
            className={cn(
              "bg-muted3 flex items-center justify-center rounded-lg py-1 text-left text-xs opacity-25 transition-colors",
              !toggle && "opacity-100",
            )}
          >
            OFF
          </button>
        </div>

        {/* Height Control */}
        <div className="grid w-full grid-cols-3 items-center py-1">
          <p className="text-sm opacity-50">Height :</p>
          <div className="flex w-full items-center justify-between gap-2">
            <input
              type="range"
              min={50}
              max={500}
              value={height}
              onChange={(e) => setHeight(Number(e.target.value))}
              className="bg-muted [&::-webkit-slider-runnable-track]:to-background [&::-webkit-slider-thumb]:bg-muted-foreground h-1.5 w-[150px] cursor-pointer appearance-none overflow-clip rounded-lg [&::-moz-range-thumb]:h-4 [&::-moz-range-thumb]:w-4 [&::-moz-range-thumb]:appearance-none [&::-moz-range-thumb]:rounded-full [&::-moz-range-thumb]:border-0 [&::-moz-range-thumb]:bg-white [&::-moz-range-track]:bg-gradient-to-r [&::-moz-range-track]:from-blue-500 [&::-moz-range-track]:to-[#4F4F4E] [&::-moz-range-track]:bg-[length:var(--range-progress)_100%] [&::-moz-range-track]:bg-no-repeat [&::-webkit-slider-runnable-track]:bg-gradient-to-r [&::-webkit-slider-runnable-track]:from-blue-500 [&::-webkit-slider-runnable-track]:bg-[length:var(--range-progress)_100%] [&::-webkit-slider-runnable-track]:bg-no-repeat [&::-webkit-slider-thumb]:h-4 [&::-webkit-slider-thumb]:w-4 [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:rounded-full"
              style={
                {
                  "--range-progress": `${((height - 50) / (500 - 50)) * 100}%`,
                } as React.CSSProperties
              }
            />
            <span className="w-8 text-right text-xs opacity-50">
              {height}px
            </span>
          </div>
        </div>

        {/* Width Control */}
        <div className="grid w-full grid-cols-3 items-center py-1">
          <p className="text-sm opacity-50">Width :</p>
          <div className="flex items-center justify-between gap-2">
            <input
              type="range"
              min={100}
              max={600}
              value={width}
              onChange={(e) => setWidth(Number(e.target.value))}
              className="bg-muted [&::-webkit-slider-runnable-track]:to-background [&::-webkit-slider-thumb]:bg-muted-foreground h-1.5 w-[150px] cursor-pointer appearance-none overflow-clip rounded-lg [&::-moz-range-thumb]:h-4 [&::-moz-range-thumb]:w-4 [&::-moz-range-thumb]:appearance-none [&::-moz-range-thumb]:rounded-full [&::-moz-range-thumb]:border-0 [&::-moz-range-thumb]:bg-white [&::-moz-range-track]:bg-gradient-to-r [&::-moz-range-track]:from-blue-500 [&::-moz-range-track]:to-[#4F4F4E] [&::-moz-range-track]:bg-[length:var(--range-progress)_100%] [&::-moz-range-track]:bg-no-repeat [&::-webkit-slider-runnable-track]:bg-gradient-to-r [&::-webkit-slider-runnable-track]:from-blue-500 [&::-webkit-slider-runnable-track]:bg-[length:var(--range-progress)_100%] [&::-webkit-slider-runnable-track]:bg-no-repeat [&::-webkit-slider-thumb]:h-4 [&::-webkit-slider-thumb]:w-4 [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:rounded-full"
              style={
                {
                  "--range-progress": `${((width - 100) / (600 - 100)) * 100}%`,
                } as React.CSSProperties
              }
            />
            <span className="w-8 text-right text-xs opacity-50">{width}px</span>
          </div>
        </div>

        {/* Blur Control */}
        <div className="grid w-full grid-cols-3 items-center py-1">
          <p className="text-sm opacity-50">Blur :</p>
          <div className="flex items-center justify-between gap-2">
            <input
              type="range"
              min={0}
              max={50}
              value={blurValue}
              onChange={(e) => setBlurValue(Number(e.target.value))}
              className="bg-muted [&::-webkit-slider-runnable-track]:to-background [&::-webkit-slider-thumb]:bg-muted-foreground h-1.5 w-[150px] cursor-pointer appearance-none overflow-clip rounded-lg [&::-moz-range-thumb]:h-4 [&::-moz-range-thumb]:w-4 [&::-moz-range-thumb]:appearance-none [&::-moz-range-thumb]:rounded-full [&::-moz-range-thumb]:border-0 [&::-moz-range-thumb]:bg-white [&::-moz-range-track]:bg-gradient-to-r [&::-moz-range-track]:from-blue-500 [&::-moz-range-track]:to-[#4F4F4E] [&::-moz-range-track]:bg-[length:var(--range-progress)_100%] [&::-moz-range-track]:bg-no-repeat [&::-webkit-slider-runnable-track]:bg-gradient-to-r [&::-webkit-slider-runnable-track]:from-blue-500 [&::-webkit-slider-runnable-track]:bg-[length:var(--range-progress)_100%] [&::-webkit-slider-runnable-track]:bg-no-repeat [&::-webkit-slider-thumb]:h-4 [&::-webkit-slider-thumb]:w-4 [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:rounded-full"
              style={
                {
                  "--range-progress": `${(blurValue / 50) * 100}%`,
                } as React.CSSProperties
              }
            />
            <span className="w-8 text-right text-xs opacity-50">
              {blurValue}
            </span>
          </div>
        </div>

        {/* Color Matrix Control */}
        <div className="grid w-full grid-cols-3 items-center py-1">
          <p className="text-sm opacity-50">Matrix :</p>
          <div className="flex items-center justify-between gap-2">
            <input
              type="range"
              min={1}
              max={50}
              value={colorMatrixValue}
              onChange={(e) => setColorMatrixValue(Number(e.target.value))}
              className="bg-muted [&::-webkit-slider-runnable-track]:to-background [&::-webkit-slider-thumb]:bg-muted-foreground h-1.5 w-[150px] cursor-pointer appearance-none overflow-clip rounded-lg [&::-moz-range-thumb]:h-4 [&::-moz-range-thumb]:w-4 [&::-moz-range-thumb]:appearance-none [&::-moz-range-thumb]:rounded-full [&::-moz-range-thumb]:border-0 [&::-moz-range-thumb]:bg-white [&::-moz-range-track]:bg-gradient-to-r [&::-moz-range-track]:from-blue-500 [&::-moz-range-track]:to-[#4F4F4E] [&::-moz-range-track]:bg-[length:var(--range-progress)_100%] [&::-moz-range-track]:bg-no-repeat [&::-webkit-slider-runnable-track]:bg-gradient-to-r [&::-webkit-slider-runnable-track]:from-blue-500 [&::-webkit-slider-runnable-track]:bg-[length:var(--range-progress)_100%] [&::-webkit-slider-runnable-track]:bg-no-repeat [&::-webkit-slider-thumb]:h-4 [&::-webkit-slider-thumb]:w-4 [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:rounded-full"
              style={
                {
                  "--range-progress": `${((colorMatrixValue - 1) / (50 - 1)) * 100}%`,
                } as React.CSSProperties
              }
            />
            <span className="w-8 text-right text-xs opacity-50">
              {colorMatrixValue}
            </span>
          </div>
        </div>

        {/* Alpha Control */}
        <div className="grid w-full grid-cols-3 items-center py-1">
          <p className="text-sm opacity-50">Alpha :</p>
          <div className="flex items-center justify-between gap-2">
            <input
              type="range"
              min={-20}
              max={0}
              value={alphaValue}
              onChange={(e) => setAlphaValue(Number(e.target.value))}
              className="bg-muted [&::-webkit-slider-runnable-track]:to-background [&::-webkit-slider-thumb]:bg-muted-foreground h-1.5 w-[150px] cursor-pointer appearance-none overflow-clip rounded-lg [&::-moz-range-thumb]:h-4 [&::-moz-range-thumb]:w-4 [&::-moz-range-thumb]:appearance-none [&::-moz-range-thumb]:rounded-full [&::-moz-range-thumb]:border-0 [&::-moz-range-thumb]:bg-white [&::-moz-range-track]:bg-gradient-to-r [&::-moz-range-track]:from-blue-500 [&::-moz-range-track]:to-[#4F4F4E] [&::-moz-range-track]:bg-[length:var(--range-progress)_100%] [&::-moz-range-track]:bg-no-repeat [&::-webkit-slider-runnable-track]:bg-gradient-to-r [&::-webkit-slider-runnable-track]:from-blue-500 [&::-webkit-slider-runnable-track]:bg-[length:var(--range-progress)_100%] [&::-webkit-slider-runnable-track]:bg-no-repeat [&::-webkit-slider-thumb]:h-4 [&::-webkit-slider-thumb]:w-4 [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:rounded-full"
              style={
                {
                  "--range-progress": `${((alphaValue + 20) / 20) * 100}%`,
                } as React.CSSProperties
              }
            />
            <span className="w-8 text-right text-xs opacity-50">
              {alphaValue}
            </span>
          </div>
        </div>
      </div>
    </motion.div>
  );
};

const LOGO_SPRING = {
  type: "spring",
  stiffness: 300,
  damping: 30,
};

```

---

### CssLink <a name="csslink"></a>

A collection of beautifully animated link components with lightweight CSS-only hover effects. Features five unique link animations including underline reveals, icon transitions, and blend mode effects. Perfect for creating engaging navigation and call-to-action elements.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper40
```

#### How to Use

```tsx
import {
  Link000,
  Link001,
  Link002,
  Link003,
  Link004,
  Link005,
} from "@/components/v1/skiper40";

const Skiper40Demo = () => {
  return (
    <div className="flex flex-col gap-4">
      {/* no animation */}
      <Link000 href="#">Contact Us</Link000>

      {/* underline left to right */}
      <Link001 href="mailto:contact@example.com">Contact Us</Link001>

      {/* underline right to left */}
      <Link002 href="https://example.com">Visit Website</Link002>

      {/* underline center */}
      <Link003 href="/about">About Page</Link003>

      {/* height 0 - full */}
      <Link004 href="/services">Our Services</Link004>

      {/* width 0 - full width*/}
      <Link005 href="/contact">Get in Touch</Link005>
    </div>
  );
};
```

#### Component Source Code

##### File: `skiper40.tsx`

```tsx
"use client";

import Link from "next/link";
import React from "react";

import { cn } from "@/lib/utils";

const Skiper40 = () => {
  return (
    <section className="h-full snap-y snap-mandatory overflow-y-scroll">
      <div className="relative flex h-full w-full flex-col items-center justify-center gap-5">
        <Link001 href="mailto:hi@skiper-ui.com">hi@skiper-ui.com</Link001>
        <Link002 href="mailto:hi@skiper-ui.com">hi@skiper-ui.com</Link002>
        <Link003 href="mailto:hi@skiper-ui.com">hi@skiper-ui.com</Link003>
        <Link004 href="mailto:hi@skiper-ui.com">hi@skiper-ui.com</Link004>
        <Link005 href="mailto:hi@skiper-ui.com">hi@skiper-ui.com</Link005>
      </div>
    </section>
  );
};

export { Link000, Link001, Link002, Link003, Link004, Link005, Skiper40 };

const Link000 = ({
  children,
  href,
  className,
}: {
  children: React.ReactNode;
  href: string;
  className?: string;
}) => {
  return (
    <Link
      href={href}
      className={cn(
        "group relative flex items-center",
        className,
        "before:pointer-events-none before:absolute before:bottom-0 before:left-0 before:h-[0.05em] before:w-full before:bg-current before:content-['']",
        "before:origin-right before:scale-x-0 before:transition-transform before:duration-300 before:ease-[cubic-bezier(0.4,0,0.2,1)]",
        "hover:before:origin-left hover:before:scale-x-100",
      )}
    >
      {children}
    </Link>
  );
};
const Link001 = ({
  children,
  href,
  className,
}: {
  children: React.ReactNode;
  href: string;
  className?: string;
}) => {
  return (
    <a
      href={href}
      target="_blank"
      className={cn(
        "group relative flex items-center",
        "before:pointer-events-none before:absolute before:left-0 before:top-[1.5em] before:h-[0.05em] before:w-full before:bg-current before:content-['']",
        "before:origin-right before:scale-x-0 before:transition-transform before:duration-300 before:ease-[cubic-bezier(0.4,0,0.2,1)]",
        "hover:before:origin-left hover:before:scale-x-100",
        className,
      )}
    >
      {children}
      <svg
        className="ml-[0.3em] mt-[0em] size-[0.55em] translate-y-1 opacity-0 transition-all duration-300 [motion-reduce:transition-none] group-hover:translate-y-0 group-hover:opacity-100 motion-reduce:transition-none"
        fill="none"
        viewBox="0 0 10 10"
        xmlns="http://www.w3.org/2000/svg"
        aria-hidden="true"
      >
        <path
          d="M1.004 9.166 9.337.833m0 0v8.333m0-8.333H1.004"
          stroke="currentColor"
          strokeWidth="1.25"
          strokeLinecap="round"
          strokeLinejoin="round"
        ></path>
      </svg>
    </a>
  );
};
const Link002 = ({
  children,
  href,
  className,
}: {
  children: React.ReactNode;
  href: string;
  className?: string;
}) => {
  return (
    <a
      href={href}
      className={cn(
        "group relative flex items-center",
        className,
        "before:pointer-events-none before:absolute before:left-0 before:top-[1.5em] before:h-[0.05em] before:w-full before:bg-current before:content-['']",
        "before:origin-right before:scale-x-0 before:transition-transform before:duration-300 before:ease-[cubic-bezier(0.4,0,0.2,1)]",
        "before:origin-left",
        "hover:before:origin-right hover:before:scale-x-100",
      )}
    >
      {children}
      <svg
        className="ml-[0.3em] mt-[0em] size-[0.55em] translate-y-1 opacity-0 transition-all duration-300 [motion-reduce:transition-none] group-hover:translate-y-0 group-hover:opacity-100 motion-reduce:transition-none"
        fill="none"
        viewBox="0 0 10 10"
        xmlns="http://www.w3.org/2000/svg"
        aria-hidden="true"
      >
        <path
          d="M1.004 9.166 9.337.833m0 0v8.333m0-8.333H1.004"
          stroke="currentColor"
          strokeWidth="1.25"
          strokeLinecap="round"
          strokeLinejoin="round"
        ></path>
      </svg>
    </a>
  );
};
const Link003 = ({
  children,
  href,
  className,
}: {
  children: React.ReactNode;
  href: string;
  className?: string;
}) => {
  return (
    <a
      href={href}
      className={cn(
        "group relative flex items-center",
        className,
        "before:pointer-events-none before:absolute before:left-0 before:top-[1.5em] before:h-[0.05em] before:w-full before:bg-current before:content-['']",
        "before:origin-right before:scale-x-0 before:transition-transform before:duration-300 before:ease-[cubic-bezier(0.4,0,0.2,1)]",
        "before:origin-center",
        "hover:before:scale-x-100",
      )}
    >
      {children}
      <svg
        className="ml-[0.3em] mt-[0em] size-[0.55em] translate-y-1 opacity-0 transition-all duration-300 [motion-reduce:transition-none] group-hover:translate-y-0 group-hover:opacity-100 motion-reduce:transition-none"
        fill="none"
        viewBox="0 0 10 10"
        xmlns="http://www.w3.org/2000/svg"
        aria-hidden="true"
      >
        <path
          d="M1.004 9.166 9.337.833m0 0v8.333m0-8.333H1.004"
          stroke="currentColor"
          strokeWidth="1.25"
          strokeLinecap="round"
          strokeLinejoin="round"
        ></path>
      </svg>
    </a>
  );
};

const Link004 = ({
  children,
  href,
  className,
}: {
  children: React.ReactNode;
  href: string;
  className?: string;
}) => {
  return (
    <a
      href={href}
      className={cn(
        "group relative flex items-center",
        className,
        "before:pointer-events-none before:absolute before:left-0 before:w-full before:bg-white before:content-['']",
        "before:origin-right before:scale-x-0 before:transition-all before:duration-300 before:ease-[cubic-bezier(0.4,0,0.2,1)]",
        "before:origin-center md:before:bottom-0",
        "before:z-1 px-2 before:h-0 before:scale-x-100 before:mix-blend-difference hover:before:h-[1.4em]",
      )}
    >
      {children}
      <svg
        className="z-0 ml-[0.6em] mt-[0em] size-[0.55em] translate-y-1 opacity-0 transition-all duration-300 [motion-reduce:transition-none] group-hover:translate-y-0 group-hover:rotate-45 group-hover:opacity-100 motion-reduce:transition-none"
        fill="none"
        viewBox="0 0 10 10"
        xmlns="http://www.w3.org/2000/svg"
        aria-hidden="true"
      >
        <path
          d="M1.004 9.166 9.337.833m0 0v8.333m0-8.333H1.004"
          stroke="currentColor"
          strokeWidth="1.25"
          strokeLinecap="round"
          strokeLinejoin="round"
        ></path>
      </svg>
    </a>
  );
};
const Link005 = ({
  children,
  href,
  className,
}: {
  children: React.ReactNode;
  href: string;
  className?: string;
}) => {
  return (
    <a
      href={href}
      className={cn(
        className,
        "group relative flex items-center",
        "before:pointer-events-none before:absolute before:left-0 before:w-full before:bg-white before:content-['']",
        "before:scale-x-1 before:transition-all before:duration-300 before:ease-[cubic-bezier(0.4,0,0.2,1)]",
        "before:origin-left md:before:top-0",
        "before:z-1 px-2 before:h-full before:scale-x-0 before:mix-blend-difference hover:before:scale-x-100",
      )}
    >
      {children}
      <svg
        className="z-0 ml-[0.6em] mt-[0em] size-[0.55em] -translate-x-1 rotate-45 opacity-0 transition-all duration-300 [motion-reduce:transition-none] group-hover:translate-x-0 group-hover:opacity-100 motion-reduce:transition-none"
        fill="none"
        viewBox="0 0 10 10"
        xmlns="http://www.w3.org/2000/svg"
        aria-hidden="true"
      >
        <path
          d="M1.004 9.166 9.337.833m0 0v8.333m0-8.333H1.004"
          stroke="currentColor"
          strokeWidth="1.25"
          strokeLinecap="round"
          strokeLinejoin="round"
        ></path>
      </svg>
    </a>
  );
};

/**
 * Skiper 40 Animated Link — React
 * Inspired by and adapted from https://cursor.com/?from=home
 * We respect the original creators. This is an inspired rebuild with our own taste and does not claim any ownership.
 * These animations aren’t associated with the cursor.com . They’re independent recreations meant to study interaction design
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### Gooey Effect <a name="gooey-effect"></a>

Use the famous Gooey effect on elements to make them feel like dripping liquid and apply it with just one line of code and no extra dependencies

#### Installation

```bash
npx shadcn add @skiper-ui/skiper64
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`

#### How to Use

```tsx
// layout.tsx or component parent just need to use the div outside the element
import { SkiperGooeyFilterProvider } from "@/components/v1/skiper64";

const Layout = ({ children }) => {
  return (
    <div>
      <SkiperGooeyFilterProvider />
      {children}
    </div>
  );
};
```

#### Component Source Code

##### File: `skiper64.tsx`

```tsx
"use client";

import { motion } from "framer-motion";
import React from "react";

const Skiper64 = () => {
  return (
    <div className="flex h-full w-full flex-col items-center justify-center">
      <SkiperGooeyFilterProvider />
      <div className="absolute top-[20%] grid content-start justify-items-center gap-6 text-center">
        <span className="after:to-foreground relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-transparent after:content-['']">
          Drag the elements to see the effect
        </span>
      </div>
      <ul
        className="flex flex-col justify-end rounded-2xl"
        style={{
          filter: "url(#SkiperGooeyFilter)",
        }}
      >
        <motion.li
          drag
          initial={INITIAL_STATE}
          animate={ANIMATED_STATE}
          className="bg-foreground absolute"
        ></motion.li>
        <motion.li
          drag
          className="bg-foreground size-12 rounded-full"
        ></motion.li>
      </ul>
    </div>
  );
};

export { Skiper64, SkiperGooeyFilterProvider };

const SkiperGooeyFilterProvider = () => {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      className="absolute bottom-0 left-0"
      version="1.1"
    >
      <defs>
        <filter id="SkiperGooeyFilter">
          <feGaussianBlur in="SourceGraphic" stdDeviation="4.4" result="blur" />
          <feColorMatrix
            in="blur"
            mode="matrix"
            values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 20 -7"
            result="SkiperGooeyFilter"
          />
          <feBlend in="SourceGraphic" in2="SkiperGooeyFilter" />
        </filter>
      </defs>
    </svg>
  );
};

const LOGO_SPRING = {
  type: "spring",
  stiffness: 300,
  damping: 30,
};

const INITIAL_STATE = {
  y: 0,
  width: 50,
  height: 50,
  borderRadius: 40,
};

const ANIMATED_STATE = {
  y: -60,
  width: 200,
  height: 100,
  borderRadius: 10,
  transition: {
    ...LOGO_SPRING,
    delay: 0.15,
    y: {
      ...LOGO_SPRING,
      delay: 0,
    },
  },
};

```

---

### Mouse follow animations <a name="mouse-follow-animations"></a>

Interactive mouse follow animations with simple and spring-based tracking effects. Features multiple cursor followers with different animation styles and spring physics.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper61
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`

#### How to Use

```tsx
import { SimpleMouseFollow, SpringMouseFollow } from "@/components/v1/skiper61";

const DemoSkiper61 = () => {
  return (
    <main>
      <SimpleMouseFollow />
      <SpringMouseFollow />
    </main>
  );
};
```

#### Component Source Code

##### File: `skiper61.tsx`

```tsx
"use client";

// TODO create a how to collection and plce it in them

import { motion, useMotionValue, useSpring } from "framer-motion";
import React from "react";

const SPRING = {
  mass: 0.1, // avoid Controls inertia (how sluggish or responsive the object feels). Lower mass = snappier motion; higher mass = lethargic motion
  damping: 10, // its like the weight of the ball heavier the ball less it will bounce or harder the rubber band the more it will bounce
  stiffness: 131, // like rubber Band the more you strech the more speed it goes back to the original position
};

const SimpleMouseFollow = () => {
  const x = useMotionValue(0);
  const y = useMotionValue(0);
  const opacity = useMotionValue(0);

  const handlePointerMove = (e: React.PointerEvent<HTMLDivElement>) => {
    const bounds = e.currentTarget.getBoundingClientRect();
    x.set(e.clientX - bounds.left);
    y.set(e.clientY - bounds.top);
  };

  return (
    <div
      onPointerMove={(e) => {
        handlePointerMove(e);
      }}
      onPointerEnter={() => {
        opacity.set(1);
      }}
      onPointerLeave={() => {
        opacity.set(0);
      }}
      className="rounded-4xl bg-background mt-20 size-[500px] cursor-none overflow-hidden"
    >
      <motion.div
        style={{
          x,
          y,
          opacity,
        }}
        className="rounded-4xl size-5 bg-[#ccc]"
      ></motion.div>
    </div>
  );
};

const SpringMouseFollow = () => {
  const xSpring = useSpring(0, SPRING);
  const ySpring = useSpring(0, SPRING);
  const opacitySpring = useSpring(0, SPRING);
  const scaleSpring = useSpring(0, SPRING);

  return (
    <div
      onPointerMove={(e) => {
        const bounds = e.currentTarget.getBoundingClientRect();
        xSpring.set(e.clientX - bounds.left);
        ySpring.set(e.clientY - bounds.top);
      }}
      onPointerEnter={() => {
        opacitySpring.set(1);
        scaleSpring.set(1);
      }}
      onPointerLeave={() => {
        opacitySpring.set(0);
        scaleSpring.set(0);
      }}
      className="rounded-4xl bg-background mt-20 size-[500px] overflow-hidden"
    >
      <motion.div
        style={{
          x: xSpring,
          y: ySpring,
          opacity: opacitySpring,
          scale: scaleSpring,
        }}
        className="rounded-4xl size-10 bg-orange-500"
      ></motion.div>
    </div>
  );
};

const Skiper61 = () => {
  return (
    <section className="h-screen w-full snap-y snap-mandatory overflow-y-scroll">
      <div className="flex h-screen w-full snap-start flex-col items-center justify-center px-5">
        <div className="grid content-start justify-items-center gap-6 text-center">
          <span className="after:to-foreground relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-transparent after:content-['']">
            Mouse follow simple
          </span>
        </div>
        <SimpleMouseFollow />
      </div>
      <div className="flex h-screen w-full snap-start flex-col items-center justify-center px-5">
        <div className="grid content-start justify-items-center gap-6 text-center">
          <span className="after:to-foreground relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-transparent after:content-['']">
            Mouse follow with Spring
          </span>
        </div>
        <SpringMouseFollow />
      </div>
    </section>
  );
};

export { SimpleMouseFollow, Skiper61, SpringMouseFollow };

```

---

### Music toggle btn <a name="music-toggle-btn"></a>

Interactive music player with animated waveform visualization. Features a clickable button that plays/pauses audio with real-time animated bars that respond to the music state. Perfect for creating engaging audio controls with smooth spring animations.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper25
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
  - `use-sound`

#### How to Use

```tsx
import { MusicToggleButton } from "@/components/v1/skiper25";

// Using just the music toggle button
const DemoSkiper25Button = () => {
  return (
    <main className="flex h-screen w-full items-center justify-center">
      <MusicToggleButton />
    </main>
  );
};
```

#### Component Source Code

##### File: `skiper25.tsx`

```tsx
"use client";

import { motion } from "framer-motion";
import React, { useEffect, useState } from "react";
import useSound from "use-sound";

const Skiper25 = () => {
  return (
    <div className="flex h-full w-full flex-col items-center justify-center">
      <div className="text-foreground absolute top-[20%] grid content-start justify-items-center gap-6 py-20 text-center">
        <span className="after:from-background after:to-foreground relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:content-['']">
          Click to play the music
        </span>
      </div>
      <MusicToggleButton />
    </div>
  );
};

export { Skiper25 };

export const MusicToggleButton = () => {
  const bars = 5;

  const getRandomHeights = () => {
    return Array.from({ length: bars }, () => Math.random() * 0.8 + 0.2);
  };

  const [heights, setHeights] = useState(getRandomHeights());

  const [isPlaying, setIsPlaying] = useState(false);

  const [play, { pause, sound }] = useSound("/audio/audio.m4a", {
    loop: true,
    onplay: () => setIsPlaying(true),
    onend: () => setIsPlaying(false),
    onpause: () => setIsPlaying(false),
    onstop: () => setIsPlaying(false),
    soundEnabled: true,
  });

  useEffect(() => {
    if (isPlaying) {
      const waveformIntervalId = setInterval(() => {
        setHeights(getRandomHeights());
      }, 100);

      return () => {
        clearInterval(waveformIntervalId);
      };
    }
    setHeights(Array(bars).fill(0.1));
  }, [isPlaying]);

  const handleClick = () => {
    if (isPlaying) {
      pause();
      setIsPlaying(false);
      return;
    }
    play();
    setIsPlaying(true);
  };

  return (
    <>
      <motion.div
        onClick={handleClick}
        key="audio"
        initial={{ padding: "14px 14px " }}
        whileHover={{ padding: "18px 22px " }}
        whileTap={{ padding: "18px 22px " }}
        transition={{ duration: 1, bounce: 0.6, type: "spring" }}
        className="bg-background cursor-pointer rounded-full p-2"
      >
        <motion.div
          initial={{ opacity: 0, filter: "blur(4px)" }}
          animate={{
            opacity: 1,
            filter: "blur(0px)",
          }}
          exit={{ opacity: 0, filter: "blur(4px)" }}
          transition={{ type: "spring", bounce: 0.35 }}
          className="flex h-[18px] w-full items-center gap-1 rounded-full"
        >
          {/* Waveform visualization */}
          {heights.map((height, index) => (
            <motion.div
              key={index}
              className="bg-foreground w-[1px] rounded-full"
              initial={{ height: 1 }}
              animate={{
                height: Math.max(4, height * 14),
              }}
              transition={{
                type: "spring",
                stiffness: 300,
                damping: 10,
              }}
            />
          ))}
        </motion.div>
      </motion.div>
    </>
  );
};

/**
 * Skiper 25 Micro Interactions_005 — React + framer motion + use-sound
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### SVG clip path mask <a name="svg-clip-path-mask"></a>

SVG clip path mask with custom geometric shapes for image masking. Features advanced SVG clip path techniques for creating unique image masks with clean geometric cuts.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper66
```

#### How to Use

```tsx
import { ClipDiv } from "@/components/v1/skiper66";

const CustomMaskedContent = () => {
  return (
    <div className="h-screen w-full">
      <ClipDiv>
        <h1 className="text-4xl font-bold text-white">Custom Content</h1>
      </ClipDiv>
    </div>
  );
};
```

#### Component Source Code

##### File: `skiper66.tsx`

```tsx
import React from "react";

import { cn } from "@/lib/utils";

const Skiper66 = () => {
  return (
    <div className="relative flex h-full w-full items-center justify-center bg-[#f5f4f3]">
      <ClipDiv imgSrc="/skiperv1/common/img_p11.png">
        <h1 className="font-cal-sans text-4xl text-red-500">Hover Me </h1>
      </ClipDiv>
    </div>
  );
};

const ClipDiv = ({
  children,
  imgSrc,
  className,
}: {
  children: React.ReactNode;
  imgSrc: string;
  className?: string;
}) => {
  return (
    <>
      <SvgMask />
      <div
        style={{ clipPath: "url(#customMask001)" }}
        className={cn(
          "group relative flex aspect-video w-full items-center justify-center overflow-hidden lg:w-[80%]",
          className,
        )}
      >
        <img
          src={imgSrc}
          alt=""
          className="duration-400 absolute inset-0 h-full w-full object-cover transition-all ease-in-out group-hover:scale-110"
        />

        {/* overlay */}
        <div className="absolute size-full bg-black/15" />

        {/* children */}
        {children && <div className="absolute">{children}</div>}
      </div>
    </>
  );
};

export { ClipDiv, Skiper66 };

//Use clipPath for clean geometric cuts.
//Use mask for blurry, faded, or complex visual effects.

const SvgMask = () => {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      fill="none"
      viewBox="0 0 1836 1053"
      className="absolute inset-0 size-full"
    >
      <clipPath id="customMask001" clipPathUnits="objectBoundingBox">
        <path
          fill="currentColor"
          d="M457.525 1.148c-20.789-3.198-193.979 1.16-283.854 2.496 11.104-.178 1.297-2.868-81.146-2.496-103.5.468-86 102.499-86 109.999s-7 524.5-6.5 547.5 10 59 6.5 99c-2.8 32-1.167 234.667 0 332.003.5 75 62.5 66.5 67 68.5s38.5 0 81.5 0 436 6 526 10.5 438.995-.5 505.495 0 330.01-12.5 417.51-12.5 230.99 2 270.99 0 40.5-16 51-31.5 12.5-61 12.5-105.5c0-44.503 7.01-274.504 7.01-348.004s-3.51-159.998-7.01-230.998 0-256.002 0-318.002 7.01-92.998-22.5-110.999c-18.79-11.471-81.99-9.999-133.49-9.999H853.525c-29 0-370 4-396 0Z"
          transform="scale(0.0005139987561, 0.0008543065594)"
        ></path>
      </clipPath>
    </svg>
  );
};

```

---

### Scroll with fade effect <a name="scroll-with-fade-effect"></a>

A top/bottom scrollable fade effect that creates smooth visual transitions at the top and bottom edges. And the best part is it doesn't use any JS or functions, just simple pure CSS animation.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper87
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
- **Registry Components**:
  - `scroll-area`

#### How to Use

```tsx
/* Scroll Fade Effect */
@supports (animation-timeline: scroll()) {
  @property --ft {
    syntax: "<length>";
    inherits: false;
    initial-value: 0px;
  }

  @property --fb {
    syntax: "<length>";
    inherits: false;
    initial-value: 40px;
  }

  .scroll-fade-y,
  [data-slot="scroll-area-viewport"] {
    --ft: 0px;
    --fb: 40px;
    mask-image: linear-gradient(
      to bottom,
      transparent 0,
      #000 var(--ft),
      #000 calc(100% - var(--fb)),
      transparent 100%
    );
    mask-size: 100% 100%;
    mask-repeat: no-repeat;
    animation:
      fadeTop 1 linear both,
      fadeBottom 1 linear both;
    animation-timeline: scroll(self), scroll(self);
    animation-range:
      0% 12%,
      88% 100%;
  }

  @keyframes fadeTop {
    from {
      --ft: 0px;
    }
    to {
      --ft: 40px;
    }
  }

  @keyframes fadeBottom {
    from {
      --fb: 40px;
    }
    to {
      --fb: 0px;
    }
  }
}
```

#### Component Source Code

##### File: `skiper87.tsx`

```tsx
import React from "react";

import { ScrollArea } from "@/components/ui/scroll-area";

const Skiper87 = () => {
  return (
    <div className="bg-muted flex h-full w-full flex-col items-center justify-center gap-10">
      <div className="-mt-10 mb-20 grid content-start justify-items-center gap-6 text-center">
        <span className="after:to-foreground relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-transparent after:content-['']">
          see the fade while scroll
        </span>
      </div>
      <div className="rounded-xl border">
        <ScrollArea className="w-62 h-72 rounded-xl">
          <div className="space-y-1 p-1">
            {Array.from({ length: 11 }).map((_, index) => (
              <div
                key={index}
                className="text-foreground/30 hover:bg-foreground/10 bg-foreground/5 flex h-10 w-full items-center gap-2 rounded-lg px-4"
              >
                00{index} <div className="bg-foreground/10 h-px flex-1"></div>
              </div>
            ))}
          </div>
        </ScrollArea>
      </div>
    </div>
  );
};

export { Skiper87 };

```

---

### Text roll navigation <a name="text-roll-navigation"></a>

Text roll hover effects and staggered character animations. Features smooth text transitions where characters roll and animate individually on hover.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper58
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`

#### How to Use

```tsx
import { Skiper58, TextRoll } from "@/components/v1/skiper58";

const CustomTextRoll = () => {
  return (
    <div>
      <TextRoll className="text-2xl font-bold">Hover me</TextRoll>

      <TextRoll className="text-4xl font-extrabold" center>
        Centered Animation
      </TextRoll>
    </div>
  );
};
```

#### Component Source Code

##### File: `skiper58.tsx`

```tsx
"use client";

import { motion } from "framer-motion";
import React from "react";

import { cn } from "@/lib/utils";

const navigationItems = [
  {
    name: "Home",
    href: "/",
    description: "[0]",
  },
  {
    name: "Components",
    href: "/components",
    description: "[1]",
  },
  {
    name: "Pricing",
    href: "/pricing",
    description: "[2]",
  },
  {
    name: "How to use",
    href: "/docs/quick-start",
    description: "[3]",
  },
  {
    name: "Account",
    href: "/user",
    description: "[4]",
  },
  {
    name: "Login",
    href: "/login",
    description: "[7]",
  },
];

export const Skiper58 = () => {
  return (
    <ul className="bs flex min-h-full w-full flex-1 flex-col items-center justify-center gap-1.5 rounded-2xl px-7 py-3 backdrop-blur-sm">
      {navigationItems.map((item, index) => (
        <li
          className="relative flex cursor-pointer flex-col items-center overflow-visible"
          key={index}
        >
          <div className="relative flex items-start">
            <TextRoll
              center
              className="text-4xl font-extrabold uppercase leading-[0.8] tracking-[-0.03em] transition-colors lg:text-5xl"
            >
              {item.name}
            </TextRoll>
          </div>
        </li>
      ))}
    </ul>
  );
};

const STAGGER = 0.035;

const TextRoll: React.FC<{
  children: string;
  className?: string;
  center?: boolean;
}> = ({ children, className, center = false }) => {
  return (
    <motion.span
      initial="initial"
      whileHover="hovered"
      className={cn("relative block overflow-hidden", className)}
      style={{
        lineHeight: 0.75,
      }}
    >
      <div>
        {children.split("").map((l, i) => {
          const delay = center
            ? STAGGER * Math.abs(i - (children.length - 1) / 2)
            : STAGGER * i;

          return (
            <motion.span
              variants={{
                initial: {
                  y: 0,
                },
                hovered: {
                  y: "-100%",
                },
              }}
              transition={{
                ease: "easeInOut",
                delay,
              }}
              className="inline-block"
              key={i}
            >
              {l}
            </motion.span>
          );
        })}
      </div>
      <div className="absolute inset-0">
        {children.split("").map((l, i) => {
          const delay = center
            ? STAGGER * Math.abs(i - (children.length - 1) / 2)
            : STAGGER * i;

          return (
            <motion.span
              variants={{
                initial: {
                  y: "100%",
                },
                hovered: {
                  y: 0,
                },
              }}
              transition={{
                ease: "easeInOut",
                delay,
              }}
              className="inline-block"
              key={i}
            >
              {l}
            </motion.span>
          );
        })}
      </div>
    </motion.span>
  );
};

export { TextRoll };

```

---

### Theme toggle btn <a name="theme-toggle-btn"></a>

Advanced theme toggle component with View Transition API animations. Features multiple animation variants including circle, circle-blur, bottom-up, and GIF-based transitions.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper26
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
  - `lucide-react`
  - `next-themes`

#### How to Use

```tsx
// components/theme-provider.tsx

"use client";

import * as React from "react";
import { ThemeProvider as NextThemesProvider } from "next-themes";

export function ThemeProvider({
  children,
  ...props
}: React.ComponentProps<typeof NextThemesProvider>) {
  return <NextThemesProvider {...props}>{children}</NextThemesProvider>;
}
```

#### Component Source Code

##### File: `skiper26.tsx`

```tsx
"use client";

import { motion } from "framer-motion";
import { GripHorizontal } from "lucide-react";
import { useTheme } from "next-themes";
import React, { useCallback, useEffect, useState } from "react";

import { cn } from "@/lib/utils";

const Skiper26 = () => {
  const [variant, setVariant] = useState<AnimationVariant>("rectangle");
  const [start, setStart] = useState<AnimationStart>("bottom-up");
  const [blur, setBlur] = useState<boolean>(false);
  const [gifType, setGifType] = useState<"1" | "2" | "3" | "custom">("1");
  const [gifUrl, setGifUrl] = useState<string>(
    "https://media.giphy.com/media/KBbr4hHl9DSahKvInO/giphy.gif?cid=790b76112m5eeeydoe7et0cr3j3ekb1erunxozyshuhxx2vl&ep=v1_stickers_search&rid=giphy.gif&ct=s",
  );

  return (
    <div className="relative flex h-full w-full flex-col items-center justify-center">
      <div className="mx-auto max-w-lg space-y-5">
        <h2 className="mt-36 text-4xl font-medium tracking-tight">
          07.09.2025 <br />
          Skiper ui is live now
        </h2>
        <p>
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Nihil, ex
          eligendi veniam praesentium temporibus natus quae laborum nemo
          repellendus cum!
        </p>
        <p>
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Nihil, ex
          eligendi veniam praesentium temporibus natus quae laborum nemo
          repellendus cum!
        </p>
        <p>
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Nihil, ex
          eligendi veniam praesentium temporibus natus quae laborum nemo
          repellendus cum!
        </p>
      </div>

      <div className="text-foreground grid content-start justify-items-center gap-6 py-20 text-center">
        <span className="after:from-background after:to-foreground relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:content-['']">
          Click to toggle the theme
        </span>
      </div>

      <ThemeToggleButton
        variant={variant}
        start={start}
        blur={blur}
        gifUrl={gifUrl}
      />
      <Options
        variant={variant}
        start={start}
        blur={blur}
        gifType={gifType}
        gifUrl={gifUrl}
        setVariant={setVariant}
        setStart={setStart}
        setBlur={setBlur}
        setGifType={setGifType}
        setGifUrl={setGifUrl}
      />
    </div>
  );
};

export { Skiper26 };

const Options = ({
  variant,
  start,
  blur,
  gifType,
  gifUrl,
  setVariant,
  setStart,
  setBlur,
  setGifType,
  setGifUrl,
}: {
  variant: AnimationVariant;
  start: AnimationStart;
  blur: boolean;
  gifType: "1" | "2" | "3" | "custom";
  gifUrl: string;
  setVariant: (variant: AnimationVariant) => void;
  setStart: (start: AnimationStart) => void;
  setBlur: (blur: boolean) => void;
  setGifType: (type: "1" | "2" | "3" | "custom") => void;
  setGifUrl: (url: string) => void;
}) => {
  return (
    <motion.div
      drag
      className="top-30 border-foreground/10 bg-muted2 absolute right-1/2 flex w-[245px] translate-x-1/2 flex-col gap-3 rounded-3xl border p-3 backdrop-blur-sm lg:right-4 lg:translate-x-0"
    >
      <div className="flex items-center justify-between">
        <span className="size-4 cursor-grab active:cursor-grabbing">
          <GripHorizontal className="size-4 opacity-50" />
        </span>

        <p className="group flex cursor-pointer items-center justify-center gap-1 rounded-lg px-2 py-1 text-sm opacity-50">
          Options
        </p>
      </div>

      <div className="flex flex-col">
        <div className="mt-1 flex justify-between py-1">
          <p className="w-20 whitespace-nowrap text-sm opacity-50">variant :</p>
          <div className="flex flex-wrap items-center justify-end gap-1">
            <button
              onClick={() => setVariant("circle")}
              className={cn(
                "cursor-pointer px-1 text-sm transition-opacity",
                variant === "circle"
                  ? "opacity-100"
                  : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
              )}
            >
              circle
            </button>
            <button
              onClick={() => setVariant("rectangle")}
              className={cn(
                "cursor-pointer px-1 text-sm transition-opacity",
                variant === "rectangle"
                  ? "opacity-100"
                  : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
              )}
            >
              rectangle
            </button>
            <button
              onClick={() => setVariant("gif")}
              className={cn(
                "cursor-pointer px-1 text-sm transition-opacity",
                variant === "gif"
                  ? "opacity-100"
                  : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
              )}
            >
              gif
            </button>
            <button
              onClick={() => setVariant("polygon")}
              className={cn(
                "cursor-pointer px-1 text-sm transition-opacity",
                variant === "polygon"
                  ? "opacity-100"
                  : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
              )}
            >
              polygon
            </button>
            <button
              onClick={() => setVariant("circle-blur")}
              className={cn(
                "cursor-pointer px-1 text-sm transition-opacity",
                variant === "circle-blur"
                  ? "opacity-100"
                  : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
              )}
            >
              circle-blur
            </button>
          </div>
        </div>

        <div className="mt-1 flex justify-between py-1">
          <p className="w-20 whitespace-nowrap text-sm opacity-50">blur :</p>
          <div className="flex flex-wrap items-center justify-end gap-1">
            <button
              onClick={() => setBlur(false)}
              className={cn(
                "cursor-pointer px-1 text-sm transition-opacity",
                !blur
                  ? "opacity-100"
                  : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
              )}
            >
              off
            </button>
            <button
              onClick={() => setBlur(true)}
              className={cn(
                "cursor-pointer px-1 text-sm transition-opacity",
                blur
                  ? "opacity-100"
                  : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
              )}
            >
              on
            </button>
          </div>
        </div>

        {/* Show start options for circle, rectangle, polygon, and circle-blur */}
        {(variant === "circle" ||
          variant === "rectangle" ||
          variant === "polygon" ||
          variant === "circle-blur") && (
          <div className="mt-1 flex justify-between py-1">
            <p className="w-20 whitespace-nowrap text-sm opacity-50">start :</p>
            <div className="flex flex-wrap items-center justify-end gap-1">
              {/* Show center option only for circle and circle-blur */}
              {(variant === "circle" || variant === "circle-blur") && (
                <button
                  onClick={() => setStart("center")}
                  className={cn(
                    "cursor-pointer px-1 text-sm transition-opacity",
                    start === "center"
                      ? "opacity-100"
                      : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
                  )}
                >
                  center
                </button>
              )}

              {/* Show directional options for rectangle */}
              {variant === "rectangle" && (
                <>
                  <button
                    onClick={() => setStart("bottom-up")}
                    className={cn(
                      "cursor-pointer px-1 text-sm transition-opacity",
                      start === "bottom-up"
                        ? "opacity-100"
                        : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
                    )}
                  >
                    bottom-up
                  </button>
                  <button
                    onClick={() => setStart("top-down")}
                    className={cn(
                      "cursor-pointer px-1 text-sm transition-opacity",
                      start === "top-down"
                        ? "opacity-100"
                        : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
                    )}
                  >
                    top-down
                  </button>
                  <button
                    onClick={() => setStart("left-right")}
                    className={cn(
                      "cursor-pointer px-1 text-sm transition-opacity",
                      start === "left-right"
                        ? "opacity-100"
                        : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
                    )}
                  >
                    left-right
                  </button>
                  <button
                    onClick={() => setStart("right-left")}
                    className={cn(
                      "cursor-pointer px-1 text-sm transition-opacity",
                      start === "right-left"
                        ? "opacity-100"
                        : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
                    )}
                  >
                    right-left
                  </button>
                </>
              )}

              {/* Show corner options for circle, polygon, and circle-blur variants */}
              {(variant === "circle" ||
                variant === "polygon" ||
                variant === "circle-blur") && (
                <>
                  <button
                    onClick={() => setStart("top-left")}
                    className={cn(
                      "cursor-pointer px-1 text-sm transition-opacity",
                      start === "top-left"
                        ? "opacity-100"
                        : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
                    )}
                  >
                    top-left
                  </button>
                  <button
                    onClick={() => setStart("top-right")}
                    className={cn(
                      "cursor-pointer px-1 text-sm transition-opacity",
                      start === "top-right"
                        ? "opacity-100"
                        : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
                    )}
                  >
                    top-right
                  </button>
                  {/* Only show bottom corners for circle, not polygon */}
                  {variant !== "polygon" && (
                    <>
                      <button
                        onClick={() => setStart("bottom-left")}
                        className={cn(
                          "cursor-pointer px-1 text-sm transition-opacity",
                          start === "bottom-left"
                            ? "opacity-100"
                            : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
                        )}
                      >
                        bottom-left
                      </button>
                      <button
                        onClick={() => setStart("bottom-right")}
                        className={cn(
                          "cursor-pointer px-1 text-sm transition-opacity",
                          start === "bottom-right"
                            ? "opacity-100"
                            : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
                        )}
                      >
                        bottom-right
                      </button>
                    </>
                  )}
                </>
              )}

              {/* Show center options for circle and circle-blur */}
              {(variant === "circle" || variant === "circle-blur") && (
                <>
                  <button
                    onClick={() => setStart("top-center")}
                    className={cn(
                      "cursor-pointer px-1 text-sm transition-opacity",
                      start === "top-center"
                        ? "opacity-100"
                        : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
                    )}
                  >
                    top-center
                  </button>
                  <button
                    onClick={() => setStart("bottom-center")}
                    className={cn(
                      "cursor-pointer px-1 text-sm transition-opacity",
                      start === "bottom-center"
                        ? "opacity-100"
                        : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
                    )}
                  >
                    bottom-center
                  </button>
                </>
              )}
            </div>
          </div>
        )}

        {/* Show gif type options only for gif variant */}
        {variant === "gif" && (
          <div className="mt-1 flex justify-between py-1">
            <p className="w-20 text-sm opacity-50">gif type :</p>
            <div className="flex flex-wrap items-center justify-end gap-1">
              <button
                onClick={() => {
                  setGifType("1");
                  setGifUrl(
                    "https://media.giphy.com/media/KBbr4hHl9DSahKvInO/giphy.gif?cid=790b76112m5eeeydoe7et0cr3j3ekb1erunxozyshuhxx2vl&ep=v1_stickers_search&rid=giphy.gif&ct=s",
                  );
                }}
                className={cn(
                  "cursor-pointer px-1 text-sm transition-opacity",
                  gifType === "1"
                    ? "opacity-100"
                    : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
                )}
              >
                1
              </button>
              <button
                onClick={() => {
                  setGifType("2");
                  setGifUrl(
                    "https://media.giphy.com/media/5PncuvcXbBuIZcSiQo/giphy.gif?cid=ecf05e47j7vdjtytp3fu84rslaivdun4zvfhej6wlvl6qqsz&ep=v1_stickers_search&rid=giphy.gif&ct=s",
                  );
                }}
                className={cn(
                  "cursor-pointer px-1 text-sm transition-opacity",
                  gifType === "2"
                    ? "opacity-100"
                    : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
                )}
              >
                2
              </button>
              <button
                onClick={() => {
                  setGifType("3");
                  setGifUrl(
                    "https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExZ3JwcXdzcHd5MW92NWprZXVpcTBtNXM5cG9obWh0N3I4NzFpaDE3byZlcD12MV9zdGlja2Vyc19zZWFyY2gmY3Q9cw/WgsVx6C4N8tjy/giphy.gif",
                  );
                }}
                className={cn(
                  "cursor-pointer px-1 text-sm transition-opacity",
                  gifType === "3"
                    ? "opacity-100"
                    : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
                )}
              >
                3
              </button>
              <button
                onClick={() => setGifType("custom")}
                className={cn(
                  "cursor-pointer px-1 text-sm transition-opacity",
                  gifType === "custom"
                    ? "opacity-100"
                    : "hover:bg-foreground/10 opacity-50 hover:opacity-100",
                )}
              >
                custom
              </button>
            </div>
          </div>
        )}

        {/* Show input only when gif variant and custom type are selected */}
        {variant === "gif" && gifType === "custom" && (
          <div className="mt-1 flex flex-col gap-1 py-1">
            <p className="text-sm opacity-50">gif url :</p>
            <input
              type="text"
              value={gifUrl}
              onChange={(e) => setGifUrl(e.target.value)}
              placeholder="Enter GIF URL"
              className="text-foreground placeholder:text-foreground/50 w-full rounded-lg bg-transparent px-2 py-1 text-xs focus:outline-none"
            />
          </div>
        )}
      </div>
    </motion.div>
  );
};

// ///////////////////////////////////////////////////////////////////////////
// Custom hook for theme toggle functionality
export const useThemeToggle = ({
  variant = "circle",
  start = "center",
  blur = false,
  gifUrl = "",
}: {
  variant?: AnimationVariant;
  start?: AnimationStart;
  blur?: boolean;
  gifUrl?: string;
} = {}) => {
  const { theme, setTheme, resolvedTheme } = useTheme();

  const [isDark, setIsDark] = useState(false);

  // Sync isDark state with resolved theme after hydration
  useEffect(() => {
    setIsDark(resolvedTheme === "dark");
  }, [resolvedTheme]);

  const styleId = "theme-transition-styles";

  const updateStyles = useCallback((css: string, name: string) => {
    if (typeof window === "undefined") return;

    let styleElement = document.getElementById(styleId) as HTMLStyleElement;

    if (!styleElement) {
      styleElement = document.createElement("style");
      styleElement.id = styleId;
      document.head.appendChild(styleElement);
    }

    styleElement.textContent = css;
  }, []);

  const toggleTheme = useCallback(() => {
    setIsDark(!isDark);

    const animation = createAnimation(variant, start, blur, gifUrl);

    updateStyles(animation.css, animation.name);

    if (typeof window === "undefined") return;

    const switchTheme = () => {
      setTheme(theme === "light" ? "dark" : "light");
    };

    if (!document.startViewTransition) {
      switchTheme();
      return;
    }

    document.startViewTransition(switchTheme);
  }, [
    theme,
    setTheme,
    variant,
    start,
    blur,
    gifUrl,
    updateStyles,
    isDark,
    setIsDark,
  ]);

  const setCrazyLightTheme = useCallback(() => {
    setIsDark(false);

    const animation = createAnimation(variant, start, blur, gifUrl);

    updateStyles(animation.css, animation.name);

    if (typeof window === "undefined") return;

    const switchTheme = () => {
      setTheme("light");
    };

    if (!document.startViewTransition) {
      switchTheme();
      return;
    }

    document.startViewTransition(switchTheme);
  }, [setTheme, variant, start, blur, gifUrl, updateStyles, setIsDark]);

  const setCrazyDarkTheme = useCallback(() => {
    setIsDark(true);

    const animation = createAnimation(variant, start, blur, gifUrl);

    updateStyles(animation.css, animation.name);

    if (typeof window === "undefined") return;

    const switchTheme = () => {
      setTheme("dark");
    };

    if (!document.startViewTransition) {
      switchTheme();
      return;
    }

    document.startViewTransition(switchTheme);
  }, [setTheme, variant, start, blur, gifUrl, updateStyles, setIsDark]);

  const setCrazySystemTheme = useCallback(() => {
    if (typeof window === "undefined") return;

    // Check system preference for dark mode
    const prefersDark = window.matchMedia(
      "(prefers-color-scheme: dark)",
    ).matches;
    setIsDark(prefersDark);

    const animation = createAnimation(variant, start, blur, gifUrl);

    updateStyles(animation.css, animation.name);

    const switchTheme = () => {
      setTheme("system");
    };

    if (!document.startViewTransition) {
      switchTheme();
      return;
    }

    document.startViewTransition(switchTheme);
  }, [setTheme, variant, start, blur, gifUrl, updateStyles, setIsDark]);

  return {
    isDark,
    setIsDark,
    toggleTheme,
    setCrazyLightTheme,
    setCrazyDarkTheme,
    setCrazySystemTheme,
  };
};

// ///////////////////////////////////////////////////////////////////////////

export const ThemeToggleButton = ({
  className = "",
  variant = "circle",
  start = "center",
  blur = false,
  gifUrl = "",
}: {
  className?: string;
  variant?: AnimationVariant;
  start?: AnimationStart;
  blur?: boolean;
  gifUrl?: string;
}) => {
  const { isDark, toggleTheme } = useThemeToggle({
    variant,
    start,
    blur,
    gifUrl,
  });

  return (
    <button
      type="button"
      className={cn(
        "size-10 cursor-pointer rounded-full bg-black p-0 transition-all duration-300 active:scale-95",
        className,
      )}
      onClick={toggleTheme}
      aria-label="Toggle theme"
    >
      <span className="sr-only">Toggle theme</span>
      <svg viewBox="0 0 240 240" fill="none" xmlns="http://www.w3.org/2000/svg">
        <motion.g
          animate={{ rotate: isDark ? -180 : 0 }}
          transition={{ ease: "easeInOut", duration: 0.5 }}
        >
          <path
            d="M120 67.5C149.25 67.5 172.5 90.75 172.5 120C172.5 149.25 149.25 172.5 120 172.5"
            fill="white"
          />
          <path
            d="M120 67.5C90.75 67.5 67.5 90.75 67.5 120C67.5 149.25 90.75 172.5 120 172.5"
            fill="black"
          />
        </motion.g>
        <motion.path
          animate={{ rotate: isDark ? 180 : 0 }}
          transition={{ ease: "easeInOut", duration: 0.5 }}
          d="M120 3.75C55.5 3.75 3.75 55.5 3.75 120C3.75 184.5 55.5 236.25 120 236.25C184.5 236.25 236.25 184.5 236.25 120C236.25 55.5 184.5 3.75 120 3.75ZM120 214.5V172.5C90.75 172.5 67.5 149.25 67.5 120C67.5 90.75 90.75 67.5 120 67.5V25.5C172.5 25.5 214.5 67.5 214.5 120C214.5 172.5 172.5 214.5 120 214.5Z"
          fill="white"
        />
      </svg>
    </button>
  );
};

// ///////////////////////////////////////////////////////////////////////////

export type AnimationVariant =
  | "circle"
  | "rectangle"
  | "gif"
  | "polygon"
  | "circle-blur";
export type AnimationStart =
  | "top-left"
  | "top-right"
  | "bottom-left"
  | "bottom-right"
  | "center"
  | "top-center"
  | "bottom-center"
  | "bottom-up"
  | "top-down"
  | "left-right"
  | "right-left";

interface Animation {
  name: string;
  css: string;
}

const getPositionCoords = (position: AnimationStart) => {
  switch (position) {
    case "top-left":
      return { cx: "0", cy: "0" };
    case "top-right":
      return { cx: "40", cy: "0" };
    case "bottom-left":
      return { cx: "0", cy: "40" };
    case "bottom-right":
      return { cx: "40", cy: "40" };
    case "top-center":
      return { cx: "20", cy: "0" };
    case "bottom-center":
      return { cx: "20", cy: "40" };
    // For directional positions, default to center (these are used for rectangle variant)
    case "bottom-up":
    case "top-down":
    case "left-right":
    case "right-left":
      return { cx: "20", cy: "20" };
  }
};

const generateSVG = (variant: AnimationVariant, start: AnimationStart) => {
  // circle-blur variant handles center case differently, so check it first
  if (variant === "circle-blur") {
    if (start === "center") {
      return `data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 40 40"><defs><filter id="blur"><feGaussianBlur stdDeviation="2"/></filter></defs><circle cx="20" cy="20" r="18" fill="white" filter="url(%23blur)"/></svg>`;
    }
    const positionCoords = getPositionCoords(start);
    if (!positionCoords) {
      throw new Error(`Invalid start position: ${start}`);
    }
    const { cx, cy } = positionCoords;
    return `data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 40 40"><defs><filter id="blur"><feGaussianBlur stdDeviation="2"/></filter></defs><circle cx="${cx}" cy="${cy}" r="18" fill="white" filter="url(%23blur)"/></svg>`;
  }

  if (start === "center") return;

  // Rectangle variant doesn't use SVG masks, so return early
  if (variant === "rectangle") return "";

  const positionCoords = getPositionCoords(start);
  if (!positionCoords) {
    throw new Error(`Invalid start position: ${start}`);
  }
  const { cx, cy } = positionCoords;

  if (variant === "circle") {
    return `data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 40 40"><circle cx="${cx}" cy="${cy}" r="20" fill="white"/></svg>`;
  }

  return "";
};

const getTransformOrigin = (start: AnimationStart) => {
  switch (start) {
    case "top-left":
      return "top left";
    case "top-right":
      return "top right";
    case "bottom-left":
      return "bottom left";
    case "bottom-right":
      return "bottom right";
    case "top-center":
      return "top center";
    case "bottom-center":
      return "bottom center";
    // For directional positions, default to center
    case "bottom-up":
    case "top-down":
    case "left-right":
    case "right-left":
      return "center";
  }
};

export const createAnimation = (
  variant: AnimationVariant,
  start: AnimationStart = "center",
  blur = false,
  url?: string,
): Animation => {
  const svg = generateSVG(variant, start);
  const transformOrigin = getTransformOrigin(start);

  if (variant === "rectangle") {
    const getClipPath = (direction: AnimationStart) => {
      switch (direction) {
        case "bottom-up":
          return {
            from: "polygon(0% 100%, 100% 100%, 100% 100%, 0% 100%)",
            to: "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
          };
        case "top-down":
          return {
            from: "polygon(0% 0%, 100% 0%, 100% 0%, 0% 0%)",
            to: "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
          };
        case "left-right":
          return {
            from: "polygon(0% 0%, 0% 0%, 0% 100%, 0% 100%)",
            to: "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
          };
        case "right-left":
          return {
            from: "polygon(100% 0%, 100% 0%, 100% 100%, 100% 100%)",
            to: "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
          };
        case "top-left":
          return {
            from: "polygon(0% 0%, 0% 0%, 0% 0%, 0% 0%)",
            to: "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
          };
        case "top-right":
          return {
            from: "polygon(100% 0%, 100% 0%, 100% 0%, 100% 0%)",
            to: "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
          };
        case "bottom-left":
          return {
            from: "polygon(0% 100%, 0% 100%, 0% 100%, 0% 100%)",
            to: "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
          };
        case "bottom-right":
          return {
            from: "polygon(100% 100%, 100% 100%, 100% 100%, 100% 100%)",
            to: "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
          };
        default:
          return {
            from: "polygon(0% 100%, 100% 100%, 100% 100%, 0% 100%)",
            to: "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
          };
      }
    };

    const clipPath = getClipPath(start);

    return {
      name: `${variant}-${start}${blur ? "-blur" : ""}`,
      css: `
       ::view-transition-group(root) {
        animation-duration: 0.7s;
        animation-timing-function: var(--expo-out);
      }
            
      ::view-transition-new(root) {
        animation-name: reveal-light-${start}${blur ? "-blur" : ""};
        ${blur ? "filter: blur(2px);" : ""}
      }

      ::view-transition-old(root),
      .dark::view-transition-old(root) {
        animation: none;
        z-index: -1;
      }
      .dark::view-transition-new(root) {
        animation-name: reveal-dark-${start}${blur ? "-blur" : ""};
        ${blur ? "filter: blur(2px);" : ""}
      }

      @keyframes reveal-dark-${start}${blur ? "-blur" : ""} {
        from {
          clip-path: ${clipPath.from};
          ${blur ? "filter: blur(8px);" : ""}
        }
        ${blur ? "50% { filter: blur(4px); }" : ""}
        to {
          clip-path: ${clipPath.to};
          ${blur ? "filter: blur(0px);" : ""}
        }
      }

      @keyframes reveal-light-${start}${blur ? "-blur" : ""} {
        from {
          clip-path: ${clipPath.from};
          ${blur ? "filter: blur(8px);" : ""}
        }
        ${blur ? "50% { filter: blur(4px); }" : ""}
        to {
          clip-path: ${clipPath.to};
          ${blur ? "filter: blur(0px);" : ""}
        }
      }
      `,
    };
  }
  if (variant === "circle" && start == "center") {
    return {
      name: `${variant}-${start}${blur ? "-blur" : ""}`,
      css: `
       ::view-transition-group(root) {
        animation-duration: 0.7s;
        animation-timing-function: var(--expo-out);
      }
            
      ::view-transition-new(root) {
        animation-name: reveal-light${blur ? "-blur" : ""};
        ${blur ? "filter: blur(2px);" : ""}
      }

      ::view-transition-old(root),
      .dark::view-transition-old(root) {
        animation: none;
        z-index: -1;
      }
      .dark::view-transition-new(root) {
        animation-name: reveal-dark${blur ? "-blur" : ""};
        ${blur ? "filter: blur(2px);" : ""}
      }

      @keyframes reveal-dark${blur ? "-blur" : ""} {
        from {
          clip-path: circle(0% at 50% 50%);
          ${blur ? "filter: blur(8px);" : ""}
        }
        ${blur ? "50% { filter: blur(4px); }" : ""}
        to {
          clip-path: circle(100.0% at 50% 50%);
          ${blur ? "filter: blur(0px);" : ""}
        }
      }

      @keyframes reveal-light${blur ? "-blur" : ""} {
        from {
           clip-path: circle(0% at 50% 50%);
           ${blur ? "filter: blur(8px);" : ""}
        }
        ${blur ? "50% { filter: blur(4px); }" : ""}
        to {
          clip-path: circle(100.0% at 50% 50%);
          ${blur ? "filter: blur(0px);" : ""}
        }
      }
      `,
    };
  }
  if (variant === "gif") {
    return {
      name: `${variant}-${start}`,
      css: `
      ::view-transition-group(root) {
  animation-timing-function: var(--expo-in);
}

::view-transition-new(root) {
  mask: url('${url}') center / 0 no-repeat;
  animation: scale 3s;
}

::view-transition-old(root),
.dark::view-transition-old(root) {
  animation: scale 3s;
}

@keyframes scale {
  0% {
    mask-size: 0;
  }
  10% {
    mask-size: 50vmax;
  }
  90% {
    mask-size: 50vmax;
  }
  100% {
    mask-size: 2000vmax;
  }
}`,
    };
  }

  if (variant === "circle-blur") {
    if (start === "center") {
      return {
        name: `${variant}-${start}`,
        css: `
        ::view-transition-group(root) {
          animation-timing-function: var(--expo-out);
        }

        ::view-transition-new(root) {
          mask: url('${svg}') center / 0 no-repeat;
          mask-origin: content-box;
          animation: scale 1s;
          transform-origin: center;
        }

        ::view-transition-old(root),
        .dark::view-transition-old(root) {
          animation: scale 1s;
          transform-origin: center;
          z-index: -1;
        }

        @keyframes scale {
          to {
            mask-size: 350vmax;
          }
        }
        `,
      };
    }

    return {
      name: `${variant}-${start}`,
      css: `
      ::view-transition-group(root) {
        animation-timing-function: var(--expo-out);
      }

      ::view-transition-new(root) {
        mask: url('${svg}') ${start.replace("-", " ")} / 0 no-repeat;
        mask-origin: content-box;
        animation: scale 1s;
        transform-origin: ${transformOrigin};
      }

      ::view-transition-old(root),
      .dark::view-transition-old(root) {
        animation: scale 1s;
        transform-origin: ${transformOrigin};
        z-index: -1;
      }

      @keyframes scale {
        to {
          mask-size: 350vmax;
        }
      }
      `,
    };
  }

  if (variant === "polygon") {
    const getPolygonClipPaths = (position: AnimationStart) => {
      switch (position) {
        case "top-left":
          return {
            darkFrom: "polygon(50% -71%, -50% 71%, -50% 71%, 50% -71%)",
            darkTo: "polygon(50% -71%, -50% 71%, 50% 171%, 171% 50%)",
            lightFrom: "polygon(171% 50%, 50% 171%, 50% 171%, 171% 50%)",
            lightTo: "polygon(171% 50%, 50% 171%, -50% 71%, 50% -71%)",
          };
        case "top-right":
          return {
            darkFrom: "polygon(150% -71%, 250% 71%, 250% 71%, 150% -71%)",
            darkTo: "polygon(150% -71%, 250% 71%, 50% 171%, -71% 50%)",
            lightFrom: "polygon(-71% 50%, 50% 171%, 50% 171%, -71% 50%)",
            lightTo: "polygon(-71% 50%, 50% 171%, 250% 71%, 150% -71%)",
          };
        default:
          // Default to top-left behavior
          return {
            darkFrom: "polygon(50% -71%, -50% 71%, -50% 71%, 50% -71%)",
            darkTo: "polygon(50% -71%, -50% 71%, 50% 171%, 171% 50%)",
            lightFrom: "polygon(171% 50%, 50% 171%, 50% 171%, 171% 50%)",
            lightTo: "polygon(171% 50%, 50% 171%, -50% 71%, 50% -71%)",
          };
      }
    };

    const clipPaths = getPolygonClipPaths(start);

    return {
      name: `${variant}-${start}${blur ? "-blur" : ""}`,
      css: `
      ::view-transition-group(root) {
        animation-duration: 0.7s;
        animation-timing-function: var(--expo-out);
      }
            
      ::view-transition-new(root) {
        animation-name: reveal-light-${start}${blur ? "-blur" : ""};
        ${blur ? "filter: blur(2px);" : ""}
      }

      ::view-transition-old(root),
      .dark::view-transition-old(root) {
        animation: none;
        z-index: -1;
      }
      .dark::view-transition-new(root) {
        animation-name: reveal-dark-${start}${blur ? "-blur" : ""};
        ${blur ? "filter: blur(2px);" : ""}
      }

      @keyframes reveal-dark-${start}${blur ? "-blur" : ""} {
        from {
          clip-path: ${clipPaths.darkFrom};
          ${blur ? "filter: blur(8px);" : ""}
        }
        ${blur ? "50% { filter: blur(4px); }" : ""}
        to {
          clip-path: ${clipPaths.darkTo};
          ${blur ? "filter: blur(0px);" : ""}
        }
      }

      @keyframes reveal-light-${start}${blur ? "-blur" : ""} {
        from {
          clip-path: ${clipPaths.lightFrom};
          ${blur ? "filter: blur(8px);" : ""}
        }
        ${blur ? "50% { filter: blur(4px); }" : ""}
        to {
          clip-path: ${clipPaths.lightTo};
          ${blur ? "filter: blur(0px);" : ""}
        }
      }
      `,
    };
  }

  // Handle circle variants with start positions using clip-path
  if (variant === "circle" && start !== "center") {
    const getClipPathPosition = (position: AnimationStart) => {
      switch (position) {
        case "top-left":
          return "0% 0%";
        case "top-right":
          return "100% 0%";
        case "bottom-left":
          return "0% 100%";
        case "bottom-right":
          return "100% 100%";
        case "top-center":
          return "50% 0%";
        case "bottom-center":
          return "50% 100%";
        default:
          return "50% 50%";
      }
    };

    const clipPosition = getClipPathPosition(start);

    return {
      name: `${variant}-${start}${blur ? "-blur" : ""}`,
      css: `
       ::view-transition-group(root) {
        animation-duration: 1s;
        animation-timing-function: var(--expo-out);
      }
            
      ::view-transition-new(root) {
        animation-name: reveal-light-${start}${blur ? "-blur" : ""};
        ${blur ? "filter: blur(2px);" : ""}
      }

      ::view-transition-old(root),
      .dark::view-transition-old(root) {
        animation: none;
        z-index: -1;
      }
      .dark::view-transition-new(root) {
        animation-name: reveal-dark-${start}${blur ? "-blur" : ""};
        ${blur ? "filter: blur(2px);" : ""}
      }

      @keyframes reveal-dark-${start}${blur ? "-blur" : ""} {
        from {
          clip-path: circle(0% at ${clipPosition});
          ${blur ? "filter: blur(8px);" : ""}
        }
        ${blur ? "50% { filter: blur(4px); }" : ""}
        to {
          clip-path: circle(150.0% at ${clipPosition});
          ${blur ? "filter: blur(0px);" : ""}
        }
      }

      @keyframes reveal-light-${start}${blur ? "-blur" : ""} {
        from {
           clip-path: circle(0% at ${clipPosition});
           ${blur ? "filter: blur(8px);" : ""}
        }
        ${blur ? "50% { filter: blur(4px); }" : ""}
        to {
          clip-path: circle(150.0% at ${clipPosition});
          ${blur ? "filter: blur(0px);" : ""}
        }
      }
      `,
    };
  }

  return {
    name: `${variant}-${start}${blur ? "-blur" : ""}`,
    css: `
      ::view-transition-group(root) {
        animation-timing-function: var(--expo-in);
      }
      ::view-transition-new(root) {
        mask: url('${svg}') ${start.replace("-", " ")} / 0 no-repeat;
        mask-origin: content-box;
        animation: scale-${start}${blur ? "-blur" : ""} 1s;
        transform-origin: ${transformOrigin};
        ${blur ? "filter: blur(2px);" : ""}
      }
      ::view-transition-old(root),
      .dark::view-transition-old(root) {
        animation: scale-${start}${blur ? "-blur" : ""} 1s;
        transform-origin: ${transformOrigin};
        z-index: -1;
      }
      @keyframes scale-${start}${blur ? "-blur" : ""} {
        from {
          ${blur ? "filter: blur(8px);" : ""}
        }
        ${blur ? "50% { filter: blur(4px); }" : ""}
        to {
          mask-size: 2000vmax;
          ${blur ? "filter: blur(0px);" : ""}
        }
      }
    `,
  };
};

/**
 * Skiper 26 Theme_buttons_002 — React + CSS + transition view api  https://developer.chrome.com/docs/web-platform/view-transitions/
 * Orignal concept from rudrodip
 * Inspired by from https://github.com/rudrodip/theme-toggle-effect
 * We respect the original creators. This is an inspired rebuild with our own taste and does not claim any ownership.
 * These animations aren’t associated with the rudrodip . They’re independent recreations meant to study interaction design
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### Theme toggle buttons <a name="theme-toggle-buttons"></a>

Inspired by and adapted from toggles.dev. This implementation is rebuilt in React and Framer Motion, avoiding external toggle packages for seamless theme integration. Use them for implementing dark/light mode switching in your applications with full control over animations and theme state management.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper4
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
  - `lucide-react`

#### How to Use

```tsx
import {
  ThemeToggleButton1,
  ThemeToggleButton2,
  ThemeToggleButton3,
  ThemeToggleButton4,
  ThemeToggleButton5,
} from "@/components/v1/skiper4";

const DemoSkiper4 = () => {
  return (
    <div className="flex items-center justify-center gap-4 p-8">
      <ThemeToggleButton1 className="h-12 w-12" />
      <ThemeToggleButton2 className="h-12 w-12" />
      <ThemeToggleButton3 className="h-12 w-12" />
      <ThemeToggleButton4 className="h-12 w-12" />
      <ThemeToggleButton5 className="h-12 w-12" />
    </div>
  );
};
```

#### Component Source Code

##### File: `skiper4.tsx`

```tsx
"use client";

import { motion } from "framer-motion";
import { GripHorizontal, RefreshCcw } from "lucide-react";
import React, { useState } from "react";

import { cn } from "@/lib/utils";

const Skiper4 = () => {
  const [scale, setScale] = useState(0);
  const [gap, setGap] = useState(0);
  const [flexDirection, setFlexDirection] = useState("row");

  return (
    <div className="flex h-full w-full flex-col items-center justify-center gap-5">
      <motion.div
        className="relative flex items-center justify-center gap-1"
        animate={{
          gap: gap ? `${gap}px` : "4px",
          scale: scale ? `${scale / 20}` : "1",
        }}
        style={{
          flexDirection: flexDirection === "column" ? "column" : "row",
        }}
        transition={{ duration: 0.35 }}
      >
        <motion.div layout>
          <ThemeToggleButton1 className={cn("size-12")} />
        </motion.div>
        <motion.div layout>
          <ThemeToggleButton2 className={cn("size-12 p-2")} />
        </motion.div>
        <motion.div layout>
          <ThemeToggleButton3 className={cn("size-12 p-2")} />
        </motion.div>
        <motion.div layout>
          <ThemeToggleButton4 className={cn("size-12 p-2")} />
        </motion.div>
        <motion.div layout>
          <ThemeToggleButton5 className={cn("size-12 p-3")} />
        </motion.div>
      </motion.div>

      {/* options */}
      <Options
        scale={scale}
        setScale={setScale}
        gap={gap}
        setGap={setGap}
        setFlexDirection={setFlexDirection}
      />
    </div>
  );
};

export { Skiper4 };

type OptionsProps = {
  scale: number;
  setScale: (value: number) => void;
  gap: number;
  setGap: (value: number) => void;
  setFlexDirection: (value: string) => void;
};

const Options = ({
  scale,
  setScale,
  gap,
  setGap,
  setFlexDirection,
}: OptionsProps) => {
  const [isDragging, setIsDragging] = useState(false);

  return (
    <motion.div
      className="top-30 border-foreground/10 bg-muted2 absolute right-1/2 flex w-[245px] translate-x-1/2 flex-col gap-3 rounded-3xl border p-3 backdrop-blur-sm lg:right-4 lg:translate-x-0"
      drag={isDragging}
      dragMomentum={false}
    >
      <div className="flex items-center justify-between">
        <span
          onPointerDown={() => setIsDragging(true)}
          onPointerUp={() => setIsDragging(false)}
          className="size-4 cursor-grab active:cursor-grabbing"
        >
          <GripHorizontal className="size-4 opacity-50" />
        </span>

        <p
          onClick={() => {
            setScale(0);
            setGap(0);
            setFlexDirection("row");
          }}
          className="hover:bg-foreground/10 group flex cursor-pointer items-center justify-center gap-2 rounded-lg px-2 py-1 text-sm opacity-50"
        >
          Options
          <span className="group-active:-rotate-360 rotate-0 cursor-pointer transition-all duration-300 group-hover:rotate-90">
            <RefreshCcw className="size-4 opacity-50" />
          </span>{" "}
        </p>
      </div>

      <div className="flex flex-col">
        <div className="flex items-center justify-between py-1">
          <p className="text-sm opacity-50">Scale </p>
          <input
            type="range"
            min={0}
            max={100}
            value={scale}
            onChange={(e) => setScale(Number(e.target.value))}
            className="bg-muted [&::-webkit-slider-runnable-track]:to-background [&::-webkit-slider-thumb]:bg-muted-foreground h-1.5 w-[150px] cursor-pointer appearance-none overflow-clip rounded-lg [&::-moz-range-thumb]:h-4 [&::-moz-range-thumb]:w-4 [&::-moz-range-thumb]:appearance-none [&::-moz-range-thumb]:rounded-full [&::-moz-range-thumb]:border-0 [&::-moz-range-thumb]:bg-white [&::-moz-range-track]:bg-gradient-to-r [&::-moz-range-track]:from-blue-500 [&::-moz-range-track]:to-[#4F4F4E] [&::-moz-range-track]:bg-[length:var(--range-progress)_100%] [&::-moz-range-track]:bg-no-repeat [&::-webkit-slider-runnable-track]:bg-gradient-to-r [&::-webkit-slider-runnable-track]:from-blue-500 [&::-webkit-slider-runnable-track]:bg-[length:var(--range-progress)_100%] [&::-webkit-slider-runnable-track]:bg-no-repeat [&::-webkit-slider-thumb]:h-4 [&::-webkit-slider-thumb]:w-4 [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:rounded-full"
            style={{ "--range-progress": `${scale}%` } as React.CSSProperties}
          />
        </div>
        <div className="flex items-center justify-between py-1">
          <p className="text-sm opacity-50">Gap </p>
          <input
            type="range"
            min={0}
            max={100}
            value={gap}
            onChange={(e) => setGap(Number(e.target.value))}
            className="bg-muted [&::-webkit-slider-runnable-track]:to-background [&::-webkit-slider-thumb]:bg-muted-foreground h-1.5 w-[150px] cursor-pointer appearance-none overflow-clip rounded-lg [&::-moz-range-thumb]:h-4 [&::-moz-range-thumb]:w-4 [&::-moz-range-thumb]:appearance-none [&::-moz-range-thumb]:rounded-full [&::-moz-range-thumb]:border-0 [&::-moz-range-thumb]:bg-white [&::-moz-range-track]:bg-gradient-to-r [&::-moz-range-track]:from-blue-500 [&::-moz-range-track]:to-[#4F4F4E] [&::-moz-range-track]:bg-[length:var(--range-progress)_100%] [&::-moz-range-track]:bg-no-repeat [&::-webkit-slider-runnable-track]:bg-gradient-to-r [&::-webkit-slider-runnable-track]:from-blue-500 [&::-webkit-slider-runnable-track]:bg-[length:var(--range-progress)_100%] [&::-webkit-slider-runnable-track]:bg-no-repeat [&::-webkit-slider-thumb]:h-4 [&::-webkit-slider-thumb]:w-4 [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:rounded-full"
            style={{ "--range-progress": `${gap}%` } as React.CSSProperties}
          />
        </div>

        <div className="mt-1 flex items-center justify-between py-1">
          <p className="text-sm opacity-50">Flex </p>
          <div className="flex items-center justify-end gap-2">
            <button
              className="cursor-pointer text-sm opacity-50 hover:opacity-100"
              onClick={() => setFlexDirection("column")}
            >
              coloumn
            </button>
            <button
              className="cursor-pointer text-sm opacity-50 hover:opacity-100"
              onClick={() => setFlexDirection("row")}
            >
              Row
            </button>
          </div>
        </div>
      </div>
    </motion.div>
  );
};

//..................................................... //

export const ThemeToggleButton1 = ({
  className = "",
}: {
  className?: string;
}) => {
  const [isDark, setIsDark] = useState(false);
  return (
    <button
      type="button"
      className={cn(
        "rounded-full bg-black text-white transition-all duration-300 active:scale-95",
        className,
      )}
      onClick={() => setIsDark(!isDark)}
    >
      <svg viewBox="0 0 240 240" fill="none" xmlns="http://www.w3.org/2000/svg">
        <motion.g
          animate={{ rotate: isDark ? -180 : 0 }}
          transition={{ ease: "easeInOut", duration: 0.35 }}
        >
          <path
            d="M120 67.5C149.25 67.5 172.5 90.75 172.5 120C172.5 149.25 149.25 172.5 120 172.5"
            fill="white"
          />
          <path
            d="M120 67.5C90.75 67.5 67.5 90.75 67.5 120C67.5 149.25 90.75 172.5 120 172.5"
            fill="black"
          />
        </motion.g>
        <motion.path
          animate={{ rotate: isDark ? 180 : 0 }}
          transition={{ ease: "easeInOut", duration: 0.35 }}
          d="M120 3.75C55.5 3.75 3.75 55.5 3.75 120C3.75 184.5 55.5 236.25 120 236.25C184.5 236.25 236.25 184.5 236.25 120C236.25 55.5 184.5 3.75 120 3.75ZM120 214.5V172.5C90.75 172.5 67.5 149.25 67.5 120C67.5 90.75 90.75 67.5 120 67.5V25.5C172.5 25.5 214.5 67.5 214.5 120C214.5 172.5 172.5 214.5 120 214.5Z"
          fill="white"
        />
      </svg>
    </button>
  );
};

//..................................................... //
export const ThemeToggleButton2 = ({
  className = "",
}: {
  className?: string;
}) => {
  const [isDark, setIsDark] = useState(false);
  return (
    <button
      type="button"
      className={cn(
        "rounded-full transition-all duration-300 active:scale-95",
        isDark ? "bg-black text-white" : "bg-white text-black",
        className,
      )}
      onClick={() => setIsDark(!isDark)}
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        aria-hidden="true"
        fill="currentColor"
        strokeLinecap="round"
        viewBox="0 0 32 32"
      >
        <clipPath id="skiper-btn-2">
          <motion.path
            animate={{ y: isDark ? 10 : 0, x: isDark ? -12 : 0 }}
            transition={{ ease: "easeInOut", duration: 0.35 }}
            d="M0-5h30a1 1 0 0 0 9 13v24H0Z"
          />
        </clipPath>
        <g clipPath="url(#skiper-btn-2)">
          <motion.circle
            animate={{ r: isDark ? 10 : 8 }}
            transition={{ ease: "easeInOut", duration: 0.35 }}
            cx="16"
            cy="16"
          />
          <motion.g
            animate={{
              rotate: isDark ? -100 : 0,
              scale: isDark ? 0.5 : 1,
              opacity: isDark ? 0 : 1,
            }}
            transition={{ ease: "easeInOut", duration: 0.35 }}
            stroke="currentColor"
            strokeWidth="1.5"
          >
            <path d="M16 5.5v-4" />
            <path d="M16 30.5v-4" />
            <path d="M1.5 16h4" />
            <path d="M26.5 16h4" />
            <path d="m23.4 8.6 2.8-2.8" />
            <path d="m5.7 26.3 2.9-2.9" />
            <path d="m5.8 5.8 2.8 2.8" />
            <path d="m23.4 23.4 2.9 2.9" />
          </motion.g>
        </g>
      </svg>
    </button>
  );
};

//..................................................... //
export const ThemeToggleButton3 = ({
  className = "",
}: {
  className?: string;
}) => {
  const [isDark, setIsDark] = useState(false);
  return (
    <button
      type="button"
      className={cn(
        "rounded-full transition-all duration-300 active:scale-95",
        isDark ? "bg-black text-white" : "bg-white text-black",
        className,
      )}
      onClick={() => setIsDark(!isDark)}
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        aria-hidden="true"
        fill="currentColor"
        strokeLinecap="round"
        viewBox="0 0 32 32"
      >
        <clipPath id="skiper-btn-3">
          <motion.path
            animate={{ y: isDark ? 14 : 0, x: isDark ? -11 : 0 }}
            transition={{ ease: "easeInOut", duration: 0.35 }}
            d="M0-11h25a1 1 0 0017 13v30H0Z"
          />
        </clipPath>
        <g clipPath="url(#skiper-btn-3)">
          <motion.circle
            animate={{ r: isDark ? 10 : 8 }}
            transition={{ ease: "easeInOut", duration: 0.35 }}
            cx="16"
            cy="16"
          />
          <motion.g
            animate={{
              scale: isDark ? 0.5 : 1,
              opacity: isDark ? 0 : 1,
            }}
            transition={{ ease: "easeInOut", duration: 0.35 }}
            stroke="currentColor"
            strokeWidth="1.5"
          >
            <path d="M18.3 3.2c0 1.3-1 2.3-2.3 2.3s-2.3-1-2.3-2.3S14.7.9 16 .9s2.3 1 2.3 2.3zm-4.6 25.6c0-1.3 1-2.3 2.3-2.3s2.3 1 2.3 2.3-1 2.3-2.3 2.3-2.3-1-2.3-2.3zm15.1-10.5c-1.3 0-2.3-1-2.3-2.3s1-2.3 2.3-2.3 2.3 1 2.3 2.3-1 2.3-2.3 2.3zM3.2 13.7c1.3 0 2.3 1 2.3 2.3s-1 2.3-2.3 2.3S.9 17.3.9 16s1-2.3 2.3-2.3zm5.8-7C9 7.9 7.9 9 6.7 9S4.4 8 4.4 6.7s1-2.3 2.3-2.3S9 5.4 9 6.7zm16.3 21c-1.3 0-2.3-1-2.3-2.3s1-2.3 2.3-2.3 2.3 1 2.3 2.3-1 2.3-2.3 2.3zm2.4-21c0 1.3-1 2.3-2.3 2.3S23 7.9 23 6.7s1-2.3 2.3-2.3 2.4 1 2.4 2.3zM6.7 23C8 23 9 24 9 25.3s-1 2.3-2.3 2.3-2.3-1-2.3-2.3 1-2.3 2.3-2.3z" />
          </motion.g>
        </g>
      </svg>
    </button>
  );
};

//..................................................... //
export const ThemeToggleButton4 = ({
  className = "",
}: {
  className?: string;
}) => {
  const [isDark, setIsDark] = useState(false);
  return (
    <button
      type="button"
      className={cn(
        "rounded-full transition-all duration-300 active:scale-95",
        isDark ? "bg-black text-white" : "bg-white text-black",
        className,
      )}
      onClick={() => setIsDark(!isDark)}
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        aria-hidden="true"
        strokeWidth="0.7"
        stroke="currentColor"
        fill="currentColor"
        strokeLinecap="round"
        viewBox="0 0 32 32"
      >
        <path
          strokeWidth="0"
          d="M9.4 9.9c1.8-1.8 4.1-2.7 6.6-2.7 5.1 0 9.3 4.2 9.3 9.3 0 2.3-.8 4.4-2.3 6.1-.7.8-2 2.8-2.5 4.4 0 .2-.2.4-.5.4-.2 0-.4-.2-.4-.5v-.1c.5-1.8 2-3.9 2.7-4.8 1.4-1.5 2.1-3.5 2.1-5.6 0-4.7-3.7-8.5-8.4-8.5-2.3 0-4.4.9-5.9 2.5-1.6 1.6-2.5 3.7-2.5 6 0 2.1.7 4 2.1 5.6.8.9 2.2 2.9 2.7 4.9 0 .2-.1.5-.4.5h-.1c-.2 0-.4-.1-.4-.4-.5-1.7-1.8-3.7-2.5-4.5-1.5-1.7-2.3-3.9-2.3-6.1 0-2.3 1-4.7 2.7-6.5z"
        />
        <path d="M19.8 28.3h-7.6" />
        <path d="M19.8 29.5h-7.6" />
        <path d="M19.8 30.7h-7.6" />
        <motion.path
          animate={{
            pathLength: isDark ? 0 : 1,
            opacity: isDark ? 0 : 1,
          }}
          transition={{ ease: "easeInOut", duration: 0.35 }}
          pathLength="1"
          fill="none"
          d="M14.6 27.1c0-3.4 0-6.8-.1-10.2-.2-1-1.1-1.7-2-1.7-1.2-.1-2.3 1-2.2 2.3.1 1 .9 1.9 2.1 2h7.2c1.1-.1 2-1 2.1-2 .1-1.2-1-2.3-2.2-2.3-.9 0-1.7.7-2 1.7 0 3.4 0 6.8-.1 10.2"
        />
        <motion.g
          animate={{
            scale: isDark ? 0.5 : 1,
            opacity: isDark ? 0 : 1,
          }}
          transition={{ ease: "easeInOut", duration: 0.35 }}
        >
          <path pathLength="1" d="M16 6.4V1.3" />
          <path pathLength="1" d="M26.3 15.8h5.1" />
          <path pathLength="1" d="m22.6 9 3.7-3.6" />
          <path pathLength="1" d="M9.4 9 5.7 5.4" />
          <path pathLength="1" d="M5.7 15.8H.6" />
        </motion.g>
      </svg>
    </button>
  );
};

//..................................................... //
export const ThemeToggleButton5 = ({
  className = "",
}: {
  className?: string;
}) => {
  const [isDark, setIsDark] = useState(false);
  return (
    <button
      type="button"
      className={cn(
        "rounded-full transition-all duration-300 active:scale-95",
        isDark ? "bg-black text-white" : "bg-white text-black",
        className,
      )}
      onClick={() => setIsDark(!isDark)}
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        aria-hidden="true"
        fill="currentColor"
        viewBox="0 0 32 32"
      >
        <clipPath id="skiper-btn-4">
          <motion.path
            animate={{ y: isDark ? 5 : 0, x: isDark ? -20 : 0 }}
            transition={{ ease: "easeInOut", duration: 0.35 }}
            d="M0-5h55v37h-55zm32 12a1 1 0 0025 0 1 1 0 00-25 0"
          />
        </clipPath>
        <g clipPath="url(#skiper-btn-4)">
          <circle cx="16" cy="16" r="15" />
        </g>
      </svg>
    </button>
  );
};

/**
 * Theme Toggle Animations — React + Framer Motion Recreation
 * Inspired by and adapted from https://toggles.dev/ (Open Source CSS Theme Toggles by Alfie Jones)
 * This implementation is rebuilt in React and Framer Motion, avoiding external toggle packages.
 *
 * Attribution: https://toggles.dev/
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

## Out of the box <a name="out-of-the-box"></a>

### Custom tooltip <a name="custom-tooltip"></a>

A custom tooltip component built with Radix UI primitives, featuring smooth animations, arrow indicators, and flexible positioning options

#### Installation

```bash
npx shadcn add @skiper-ui/skiper101
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-tooltip`
- **Registry Components**:
  - `tooltip`

#### How to Use

```tsx
import {
  Skiper102,
  Tooltip,
  TooltipTrigger,
  TooltipContent,
} from "@/components/v1/skiper101";

// Using the demo component
const DemoSkiper101 = () => {
  return (
    <div className="h-screen w-full">
      <Skiper102 />
    </div>
  );
};

// Using the tooltip components directly
const CustomTooltip = () => {
  return (
    <Tooltip>
      <TooltipTrigger>
        <div>
          <p>Tooltip</p>
        </div>
      </TooltipTrigger>
      <TooltipContent>
        <p>Tooltip Content</p>
      </TooltipContent>
    </Tooltip>
  );
};
```

#### Component Source Code

##### File: `skiper101.tsx`

```tsx
"use client";

import * as TooltipPrimitive from "@radix-ui/react-tooltip";
import * as React from "react";

import { cn } from "@/lib/utils";

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
  );
}

const Skiper102 = () => {
  return (
    <div className="flex flex-col">
      <div className="-mt-36 mb-36 grid content-start justify-items-center gap-6 text-center">
        <span className="after:to-foreground relative max-w-[14ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-transparent after:content-['']">
          Hover to see border arrow tooltip
        </span>
      </div>
      <Tooltip>
        <TooltipTrigger>
          <div>
            <p>Tooltip</p>
          </div>
        </TooltipTrigger>
        <TooltipContent>
          <p>Tooltip Content</p>
        </TooltipContent>
      </Tooltip>
    </div>
  );
};

function Tooltip({
  ...props
}: React.ComponentProps<typeof TooltipPrimitive.Root>) {
  return (
    <TooltipProvider>
      <TooltipPrimitive.Root data-slot="tooltip" {...props} />
    </TooltipProvider>
  );
}

function TooltipTrigger({
  ...props
}: React.ComponentProps<typeof TooltipPrimitive.Trigger>) {
  return <TooltipPrimitive.Trigger data-slot="tooltip-trigger" {...props} />;
}

function TooltipContent({
  className,
  sideOffset = 0,
  children,
  ...props
}: React.ComponentProps<typeof TooltipPrimitive.Content>) {
  return (
    <TooltipPrimitive.Portal>
      <TooltipPrimitive.Content
        data-slot="tooltip-content"
        sideOffset={sideOffset}
        className={cn(
          "origin-(--radix-tooltip-content-transform-origin) animate-in bg-background text-foreground outline-border fade-in-0 zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95 group z-50 w-fit text-balance rounded-md px-3 py-1.5 text-xs outline-1",
          className,
        )}
        {...props}
      >
        {children}
        <TooltipPrimitive.Arrow asChild>
          <span>
            <ArrowSvg />
          </span>
        </TooltipPrimitive.Arrow>
      </TooltipPrimitive.Content>
    </TooltipPrimitive.Portal>
  );
}

export { Skiper102, Tooltip, TooltipContent, TooltipProvider, TooltipTrigger };

const ArrowSvg = (props: React.ComponentProps<"svg">) => (
  <svg
    width="20"
    height="10"
    viewBox="0 0 20 10"
    fill="none"
    className="ml-[1px] mt-[-1px]"
    xmlns="http://www.w3.org/2000/svg"
    {...props}
  >
    <path
      d="M10.3356 7.39793L15.1924 3.02682C15.9269 2.36577 16.8801 2 17.8683 2H20V0H0V2H1.4651C2.4532 2 3.4064 2.36577 4.1409 3.02682L8.9977 7.39793C9.378 7.7402 9.9553 7.74021 10.3356 7.39793Z"
      fill="var(--color-background)"
    />
    <path d="M11.1363 8.14124C10.3757 8.82575 9.22111 8.82578 8.46041 8.14122L3.60361 3.77011C3.05281 3.27432 2.33791 2.99999 1.59681 2.99999L4.24171 3L9.12941 7.39793C9.50971 7.7402 10.087 7.7402 10.4674 7.39793L15.3544 3L18 2.99999C17.2589 2.99999 16.544 3.27432 15.9931 3.77011L11.1363 8.14124Z" />
    <path
      d="M9.6667 6.65461L14.5235 2.28352C15.4416 1.45721 16.6331 1 17.8683 1H20V2H17.8683C16.8801 2 15.9269 2.36577 15.1924 3.02682L10.3356 7.39793C9.9553 7.74021 9.378 7.7402 8.9977 7.39793L4.1409 3.02682C3.4064 2.36577 2.4532 2 1.4651 2H0V1H1.4651C2.7002 1 3.8917 1.45722 4.8099 2.28352L9.6667 6.65461Z"
      fill="var(--color-border)"
    />
  </svg>
);

```

---

## Rauno <a name="rauno"></a>

### Progressive Blur <a name="progressive-blur"></a>

A lightweight CSS-only utility that applies progressive blur effects on top of text content. Creates elegant visual depth with customizable gradient overlays and backdrop filters for enhanced readability and visual appeal.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper41
```

#### How to Use

```tsx
import { ProgressiveBlur } from "@/components/v1/skiper41";

const DemoSkiper41 = () => {
  return (
    <main className="relative h-full w-full bg-[#f5f4f3]">
      <ProgressiveBlur position="top" backgroundColor="#f5f4f3" />
      <ProgressiveBlur position="bottom" backgroundColor="#f5f4f3" />
      {/* content */}
    </main>
  );
};
```

#### Component Source Code

##### File: `skiper41.tsx`

```tsx
import React from "react";

type ProgressiveBlurProps = {
  className?: string;
  backgroundColor?: string;
  position?: "top" | "bottom";
  height?: string;
  blurAmount?: string;
};

const ProgressiveBlur = ({
  className = "",
  backgroundColor = "#f5f4f3",
  position = "top",
  height = "150px",
  blurAmount = "4px",
}: ProgressiveBlurProps) => {
  const isTop = position === "top";

  return (
    <div
      className={`pointer-events-none absolute left-0 w-full select-none ${className}`}
      style={{
        [isTop ? "top" : "bottom"]: 0,
        height,
        background: isTop
          ? `linear-gradient(to top, transparent, ${backgroundColor})`
          : `linear-gradient(to bottom, transparent, ${backgroundColor})`,
        maskImage: isTop
          ? `linear-gradient(to bottom, ${backgroundColor} 50%, transparent)`
          : `linear-gradient(to top, ${backgroundColor} 50%, transparent)`,
        WebkitBackdropFilter: `blur(${blurAmount})`,
        backdropFilter: `blur(${blurAmount})`,
        WebkitUserSelect: "none",
        userSelect: "none",
      }}
    />
  );
};

const Skiper41 = () => {
  return (
    <div className="relative flex h-full w-full flex-col items-center justify-center bg-[#f5f4f3] text-black/40">
      <ProgressiveBlur position="top" backgroundColor="#f5f4f3" />
      <ProgressiveBlur position="bottom" backgroundColor="#f5f4f3" />

      <div className="flex h-[calc(100vh-1rem)] w-full flex-col items-center overflow-scroll">
        <div className="mt-42 grid content-start justify-items-center gap-6 text-center text-black">
          <span className="relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-white after:to-black after:content-['']">
            Scroll down to see the effect
          </span>
        </div>

        <div className="mt-24 w-full max-w-lg space-y-20 px-5 text-justify">
          {Array.from({ length: 10 }).map((_, index) => (
            <div key={index}>
              Lorem ipsum dolor sit amet consectetur adipisicing elit.
              Obcaecati, reiciendis eum vitae nostrum, temporibus repudiandae
              voluptatibus, natus iure ipsa velit odit quibusdam illum. Quaerat
              cumque laudantium libero reprehenderit perferendis quo nulla
              voluptate? Repellat tenetur labore exercitationem dicta libero
              voluptate suscipit, iusto ea assumenda. Ipsa enim, quidem atque
              modi error eaque, debitis perferendis, hic iste libero dignissimos
              ea! Quod inventore beatae aspernatur nulla rem perferendis aperiam
              at debitis delectus odit quia animi ex mollitia vero molestias
              itaque deleniti, quos exercitationem consequatur assumenda dolor?
              Quod reiciendis in similique reprehenderit commodi quo blanditiis
              nobis hic ea optio illum placeat officia alias quasi autem earum
              quos obcaecati, voluptatum corporis quisquam. Quisquam iste, quas
              explicabo omnis harum aut quam adipisci, voluptatem saepe
              accusantium doloribus repellendus amet culpa magnam ex et dolores
              accusamus commodi facere aliquam voluptatum alias? Officia
              expedita ut vel? Beatae deserunt sequi id eos libero suscipit
              totam cum, sed architecto atque quisquam et incidunt quod fuga
              ullam repellat assumenda quos ab, voluptatum sint nesciunt? Ad
              sapiente est laborum quam sint eius sequi. Eum, veniam
              dignissimos.
            </div>
          ))}
        </div>
      </div>
    </div>
  );
};

export { ProgressiveBlur, Skiper41 };

/**
 * Skiper 41 Canvas_Landing_004 — React + framer motion
 * Inspired by and adapted from https://devouringdetails.com/
 * We respect the original creators. This is an inspired rebuild with our own taste and does not claim any ownership.
 * These animations aren’t associated with the devouringdetails.com . They’re independent recreations meant to study interaction design
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

## Scroll Effects <a name="scroll-effects"></a>

### 3D perspective text <a name="3d-perspective-text"></a>

A 3D perspective text scroll effect that creates depth and visual interest as users scroll. The text rotates and translates in 3D space, creating an immersive scrolling experience with smooth animations powered by Framer Motion and Lenis.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper28
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
  - `lenis`

#### How to Use

```tsx
import { Skiper28 } from "@/components/v1/skiper28";

// Using the 3D perspective text scroll component
const DemoSkiper28 = () => {
  return (
    <main>
      <Skiper28 />
    </main>
  );
};
```

#### Component Source Code

##### File: `skiper28.tsx`

```tsx
"use client";

import {
  motion,
  useMotionTemplate,
  useScroll,
  useTransform,
} from "framer-motion";
import ReactLenis from "lenis/react";
import React, { useRef } from "react";

const Skiper28 = () => {
  const targetRef = useRef<HTMLDivElement | null>(null);
  const { scrollYProgress } = useScroll({
    target: targetRef,
  });

  const yMotionValue = useTransform(scrollYProgress, [0, 1], [487, 0]);
  const transform = useMotionTemplate`rotateX(30deg) translateY(${yMotionValue}px) translateZ(10px)`;

  return (
    <ReactLenis root>
      <div
        ref={targetRef}
        className="relative z-0 h-[300vh] w-screen bg-[#f5f4f3] text-black"
      >
        <div className="absolute left-1/2 top-[10%] grid -translate-x-1/2 content-start justify-items-center gap-6 text-center text-black">
          <span className="relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-white after:to-black after:content-['']">
            scroll down to see
          </span>
        </div>
        <div
          className="sticky top-0 mx-auto flex items-center justify-center bg-transparent py-20"
          style={{
            transformStyle: "preserve-3d",
            perspective: "200px",
          }}
        >
          <motion.div
            style={{
              transformStyle: "preserve-3d",
              transform,
            }}
            className="font-geist w-full max-w-4xl text-center text-6xl font-bold tracking-tighter text-[#ff5800]"
          >
            Jatt seeweyan cho langheya chudail takkri jaani badi sohni bhoot
            female takkri .. kehndi jatta .. oye jatta.... kehndi jatta .. metho
            darke ho ja katha .. nai tan aah kar du ... nai tan waah kardu...
            tenu ethe khade khade nu swah kardu ... jatt kehnda hor menu ki
            chahida ... jatt kehnda hor menu ki chahida .. avein gallan-baatan
            vich bohta sama na gva aaja chimbad ja ... mein keha chimbad ja .. .
            aaja chimbad ja ... mein keha chimbad ja .. .
            <div className="absolute bottom-0 left-0 h-[60vh] w-full bg-gradient-to-b from-transparent to-white" />
          </motion.div>
        </div>
      </div>
    </ReactLenis>
  );
};

export { Skiper28 };

/**
 * Skiper 28 PerspectiveTextScroll — React + framer motion + lenis
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### Card stack scroll <a name="card-stack-scroll"></a>

Interactive sticky card stack with smooth scroll-based scaling animations. Features a collection of project cards that scale and transform as users scroll, creating an engaging parallax effect with framer-motion animations and Lenis smooth scrolling.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper16
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
  - `lenis`

#### How to Use

```tsx
import { StickyCard_001 } from "@/components/v1/skiper16";

const projects = [
  {
    title: "Your Project 1",
    src: "/path/to/your/image1.png",
  },
  {
    title: "Your Project 2",
    src: "/path/to/your/image2.png",
  },
  // Add more projects as needed
];

const DemoSkiper16 = () => {
  const container = useRef<HTMLDivElement>(null);
  const { scrollYProgress } = useScroll({
    target: container,
    offset: ["start start", "end end"],
  });

  return (
      <main
        ref={container}
        className="relative flex w-full flex-col items-center justify-center  py-[50vh]"
      >
        {projects.map((project, i) => {
          const targetScale = Math.max(
            0.5,
            1 - (projects.length - i - 1) * 0.1,
          );
          return (
            <StickyCard_001
              key={`p_${i}`}
              i={i}
              {...project}
              progress={scrollYProgress}
              range={[i * 0.25, 1]}
              targetScale={targetScale}
            />
          );
        })}
      </main>
  );
};

<PropsTable
  props={[
    {
      name: "i",
      description: "Index of the card in the stack",
      default: "0",
    },
    {
      name: "title",
      description: "Title of the project",
      default: "Project",
    },
    {
      name: "src",
      description: "Image source path for the project card",
      default: "/images/lummi/img8.png",
    },
    {
      name: "progress",
      description: "Scroll progress value from framer-motion useScroll hook",
      default: "MotionValue",
    },
    {
      name: "range",
      description: "Scroll range for animation [start, end]",
      default: "[0, 1]",
    },
    {
      name: "targetScale",
      description: "Target scale value for the card transformation",
      default: "0.5",
    },
  ]}
/>

### Attribution

illustrations are from [lummi.ai](https://www.lummi.ai/)
```

#### Component Source Code

##### File: `skiper16.tsx`

```tsx
"use client";

import { motion, useScroll, useTransform } from "framer-motion";
import ReactLenis from "lenis/react";
import React, { useRef } from "react";

const projects = [
  {
    title: "Project 1",
    src: "/images/lummi/img8.png",
  },
  {
    title: "Project 2",
    src: "/images/lummi/img14.png",
  },
  {
    title: "Project 3",
    src: "/images/lummi/img10.png",
  },
  {
    title: "Project 4",
    src: "/images/lummi/img15.png",
  },
  {
    title: "Project 5",
    src: "/images/lummi/img12.png",
  },
];

const StickyCard_001 = ({
  i,
  title,
  src,
  progress,
  range,
  targetScale,
}: {
  i: number;
  title: string;
  src: string;
  progress: any;
  range: [number, number];
  targetScale: number;
}) => {
  const container = useRef<HTMLDivElement>(null);

  const scale = useTransform(progress, range, [1, targetScale]);

  return (
    <div
      ref={container}
      className="sticky top-0 flex items-center justify-center"
    >
      <motion.div
        style={{
          scale,
          top: `calc(-5vh + ${i * 20 + 250}px)`,
        }}
        className="rounded-4xl relative -top-1/4 flex h-[300px] w-[500px] origin-top flex-col overflow-hidden"
      >
        <img src={src} alt={title} className="h-full w-full object-cover" />
      </motion.div>
    </div>
  );
};

const Skiper16 = () => {
  const container = useRef<HTMLDivElement>(null);
  const { scrollYProgress } = useScroll({
    target: container,
    offset: ["start start", "end end"],
  });

  return (
    <ReactLenis root>
      <main
        ref={container}
        className="relative flex w-full flex-col items-center justify-center pb-[100vh] pt-[50vh]"
      >
        <div className="absolute left-1/2 top-[10%] grid -translate-x-1/2 content-start justify-items-center gap-6 text-center">
          <span className="after:from-background after:to-foreground relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:content-['']">
            scroll down to see card stack
          </span>
        </div>
        {projects.map((project, i) => {
          const targetScale = Math.max(
            0.5,
            1 - (projects.length - i - 1) * 0.1,
          );
          return (
            <StickyCard_001
              key={`p_${i}`}
              i={i}
              {...project}
              progress={scrollYProgress}
              range={[i * 0.25, 1]}
              targetScale={targetScale}
            />
          );
        })}
      </main>
    </ReactLenis>
  );
};

export { Skiper16, StickyCard_001 };

/**
 * Skiper 16 StickyCard_001 — React + Framer Motion
 * We respect the original creators. This is an inspired rebuild with our own taste and does not claim any ownership.
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### Card stack with gsap and rotate <a name="card-stack-with-gsap-and-rotate"></a>

Interactive sticky card stack with GSAP-powered scroll animations. Features a collection of cards that scale, rotate, and transform as users scroll, creating an engaging parallax effect with smooth GSAP animations and Lenis smooth scrolling.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper17
```

#### Dependencies

- **NPM Packages**:
  - `gsap`
  - `@gsap/react`
  - `framer-motion`
  - `lenis`

#### How to Use

```tsx
import { StickyCard002 } from "@/components/v1/skiper17";

const cards = [
  {
    id: 1,
    image: "/path/to/your/image1.jpg",
    alt: "Description 1",
  },
  {
    id: 2,
    image: "/path/to/your/image2.jpg",
    alt: "Description 2",
  },
  // Add more cards as needed
];

const BasicDemo = () => {
  return (
    <div className="h-screen w-full">
      <StickyCard002 cards={cards} />
    </div>
  );
};
```

#### Component Source Code

##### File: `skiper17.tsx`

```tsx
"use client";

import { useGSAP } from "@gsap/react";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import ReactLenis from "lenis/react";
import { useRef } from "react";

import { cn } from "@/lib/utils";

interface CardData {
  id: number | string;
  image: string;
  alt?: string;
}

interface StickyCard002Props {
  cards: CardData[];
  className?: string;
  containerClassName?: string;
  imageClassName?: string;
}

const StickyCard002 = ({
  cards,
  className,
  containerClassName,
  imageClassName,
}: StickyCard002Props) => {
  const container = useRef(null);
  const imageRefs = useRef<(HTMLImageElement | null)[]>([]);

  useGSAP(
    () => {
      gsap.registerPlugin(ScrollTrigger);

      const imageElements = imageRefs.current;
      const totalCards = imageElements.length;

      if (!imageElements[0]) return;

      gsap.set(imageElements[0], { y: "0%", scale: 1, rotation: 0 });

      for (let i = 1; i < totalCards; i++) {
        if (!imageElements[i]) continue;
        gsap.set(imageElements[i], { y: "100%", scale: 1, rotation: 0 });
      }

      const scrollTimeline = gsap.timeline({
        scrollTrigger: {
          trigger: ".sticky-cards",
          start: "top top",
          end: `+=${window.innerHeight * (totalCards - 1)}`,
          pin: true,
          scrub: 0.5,
          pinSpacing: true,
        },
      });

      for (let i = 0; i < totalCards - 1; i++) {
        const currentImage = imageElements[i];
        const nextImage = imageElements[i + 1];
        const position = i;
        if (!currentImage || !nextImage) continue;

        scrollTimeline.to(
          currentImage,
          {
            scale: 0.7,
            rotation: 5,
            duration: 1,
            ease: "none",
          },
          position,
        );

        scrollTimeline.to(
          nextImage,
          {
            y: "0%",
            duration: 1,
            ease: "none",
          },
          position,
        );
      }

      const resizeObserver = new ResizeObserver(() => {
        ScrollTrigger.refresh();
      });

      if (container.current) {
        resizeObserver.observe(container.current);
      }

      return () => {
        resizeObserver.disconnect();
        scrollTimeline.kill();
        ScrollTrigger.getAll().forEach((trigger) => trigger.kill());
      };
    },
    { scope: container },
  );

  return (
    <div className={cn("relative h-full w-full", className)} ref={container}>
      <div className="sticky-cards relative flex h-full w-full items-center justify-center overflow-hidden p-3 lg:p-8">
        <div
          className={cn(
            "relative h-[90%] w-full max-w-sm overflow-hidden rounded-lg sm:max-w-md md:max-w-lg lg:max-w-xl xl:max-w-2xl 2xl:max-w-3xl",
            containerClassName,
          )}
        >
          {cards.map((card, i) => (
            <img
              key={card.id}
              src={card.image}
              alt={card.alt || ""}
              className={cn(
                "rounded-4xl absolute h-full w-full object-cover",
                imageClassName,
              )}
              ref={(el) => {
                imageRefs.current[i] = el;
              }}
            />
          ))}
        </div>
      </div>
    </div>
  );
};

// Example usage component with default data
const Skiper17 = () => {
  const defaultCards = [
    {
      id: 1,
      image: "/images/lummi/img14.png",
    },
    {
      id: 2,
      image: "/images/lummi/img15.png",
    },
    {
      id: 3,
      image: "/images/lummi/img29.png",
    },
    {
      id: 4,
      image: "/images/lummi/img21.png",
    },
    {
      id: 5,
      image: "/images/lummi/img27.png",
    },
  ];

  return (
    <ReactLenis root>
      <div className="h-full w-full">
        <StickyCard002 cards={defaultCards} />
      </div>
    </ReactLenis>
  );
};

export { Skiper17, StickyCard002 };

/**
 * Skiper 17 StickyCard_002 — React + Gsap + scrollTrigger
 * We respect the original creators. This is an inspired rebuild with our own taste and does not claim any ownership.
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### Oliver parallax <a name="oliver-parallax"></a>

A stunning parallax image gallery with smooth scrolling animations. Features multiple columns of images that move at different speeds creating a dynamic depth effect. Perfect for showcasing portfolios or image collections with engaging scroll interactions.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper30
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
  - `lenis`

#### How to Use

```tsx
import { Skiper30 } from "@/components/v1/skiper30";

// Using the parallax image gallery
const DemoSkiper30 = () => {
  return (
    <main>
      <Skiper30 />
    </main>
  );
};
```

#### Component Source Code

##### File: `skiper30.tsx`

```tsx
"use client";

import { motion, MotionValue, useScroll, useTransform } from "framer-motion";
import Lenis from "lenis";
import { useEffect, useRef, useState } from "react";

const images = [
  "/images/lummi/img15.png",
  "/images/lummi/img21.png",
  "/images/lummi/img3.png",
  "/images/lummi/img4.png",
  "/images/lummi/img5.png",
  "/images/lummi/img6.png",
  "/images/lummi/img7.png",
  "/images/lummi/img8.png",
  "/images/lummi/img24.png",
  "/images/lummi/img10.png",
  "/images/lummi/img11.png",
  "/images/lummi/img12.png",
  "/images/lummi/img13.png",
];

const Skiper30 = () => {
  const gallery = useRef<HTMLDivElement>(null);
  const [dimension, setDimension] = useState({ width: 0, height: 0 });

  const { scrollYProgress } = useScroll({
    target: gallery,
    offset: ["start end", "end start"],
  });

  const { height } = dimension;
  const y = useTransform(scrollYProgress, [0, 1], [0, height * 2]);
  const y2 = useTransform(scrollYProgress, [0, 1], [0, height * 3.3]);
  const y3 = useTransform(scrollYProgress, [0, 1], [0, height * 1.25]);
  const y4 = useTransform(scrollYProgress, [0, 1], [0, height * 3]);

  useEffect(() => {
    const lenis = new Lenis();

    const raf = (time: number) => {
      lenis.raf(time);
      requestAnimationFrame(raf);
    };

    const resize = () => {
      setDimension({ width: window.innerWidth, height: window.innerHeight });
    };

    window.addEventListener("resize", resize);
    requestAnimationFrame(raf);
    resize();

    return () => {
      window.removeEventListener("resize", resize);
    };
  }, []);

  return (
    <main className="w-full bg-[#eee] text-black">
      <div className="font-geist flex h-screen items-center justify-center gap-2">
        <div className="absolute left-1/2 top-[10%] grid -translate-x-1/2 content-start justify-items-center gap-6 text-center text-black">
          <span className="relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-white after:to-black after:content-['']">
            scroll down to see
          </span>
        </div>
      </div>

      <div
        ref={gallery}
        className="relative box-border flex h-[175vh] gap-[2vw] overflow-hidden bg-white p-[2vw]"
      >
        <Column images={[images[0], images[1], images[2]]} y={y} />
        <Column images={[images[3], images[4], images[5]]} y={y2} />
        <Column images={[images[6], images[7], images[8]]} y={y3} />
        <Column images={[images[6], images[7], images[8]]} y={y4} />
      </div>
      <div className="font-geist relative flex h-screen items-center justify-center gap-2">
        <div className="absolute left-1/2 top-[10%] grid -translate-x-1/2 content-start justify-items-center gap-6 text-center text-black">
          <span className="relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-white after:to-black after:content-['']">
            scroll Up to see
          </span>
        </div>
      </div>
    </main>
  );
};

type ColumnProps = {
  images: string[];
  y: MotionValue<number>;
};

const Column = ({ images, y }: ColumnProps) => {
  return (
    <motion.div
      className="relative -top-[45%] flex h-full w-1/4 min-w-[250px] flex-col gap-[2vw] first:top-[-45%] [&:nth-child(2)]:top-[-95%] [&:nth-child(3)]:top-[-45%] [&:nth-child(4)]:top-[-75%]"
      style={{ y }}
    >
      {images.map((src, i) => (
        <div key={i} className="relative h-full w-full overflow-hidden">
          <img
            src={`${src}`}
            alt="image"
            className="pointer-events-none object-cover"
          />
        </div>
      ))}
    </motion.div>
  );
};

export { Skiper30 };

/**
 * Skiper 30 Parallax_002 — React + framer motion + lenis
 * Inspired by and adapted from https://www.siena.film/films/my-project-x
 * We respect the original creators. This is an inspired rebuild with our own taste and does not claim any ownership.
 * These animations aren’t associated with the siena.film . They’re independent recreations meant to study interaction design
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### Scroll images reveal 003 <a name="scroll-images-reveal-003"></a>

Interactive sticky card stack with smooth scroll-based scaling animations. Features a collection of project cards that scale and transform as users scroll, creating an engaging parallax effect with framer-motion animations and Lenis smooth scrolling.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper34
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
  - `lenis`

#### How to Use

```tsx
import { StickyCard_003 } from "@/components/v1/skiper34";

const images = [
  "/images/lummi/img14.png",
  "/images/lummi/img30.png",
  "/images/lummi/img19.png",
  "/images/lummi/img21.png",
  "/images/lummi/img23.png",
  "/images/lummi/imgp2.png",
  "/images/lummi/img27.png",
];

const Skiper34 = () => {
  return (
    <section className="relative flex w-screen flex-col items-center gap-[10vh] py-[10vh]">
      {images.map((img, idx) => (
        <StickyCard_003 key={idx} imgUrl={img} />
      ))}
    </section>
  );
};
```

#### Component Source Code

##### File: `skiper34.tsx`

```tsx
"use client";

import {
  motion,
  useInView,
  useMotionValue,
  useScroll,
  useTransform,
} from "framer-motion";
import ReactLenis from "lenis/react";
import { useEffect, useRef, useState } from "react";

const images = [
  "/images/lummi/img14.png",
  "/images/lummi/img30.png",
  "/images/lummi/img19.png",
  "/images/lummi/img21.png",
  "/images/lummi/img23.png",
  "/images/lummi/imgp2.png",
  "/images/lummi/img27.png",
];

const Skiper34 = () => {
  return (
    <ReactLenis root>
      <section className="relative flex w-screen flex-col items-center gap-[10vh] px-4 pt-[50vh]">
        <div className="absolute left-1/2 top-24 grid -translate-x-1/2 content-start justify-items-center gap-6 text-center">
          <span className="after:from-background after:to-foreground relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:content-['']">
            scroll down to see effect
          </span>
        </div>
        {images.map((img, idx) => (
          <StickyCard_003 key={idx} imgUrl={img} />
        ))}
      </section>
    </ReactLenis>
  );
};

const StickyCard_003 = ({ imgUrl }: { imgUrl: string }) => {
  const vertMargin = 10;
  const container = useRef(null);
  const [maxScrollY, setMaxScrollY] = useState(Infinity);

  const filter = useMotionValue(0);
  // Remove filter2, add negateFilter
  const negateFilter = useTransform(filter, (value) => -value);

  const { scrollY } = useScroll({
    target: container,
  });
  const scale = useTransform(scrollY, [maxScrollY, maxScrollY + 10000], [1, 0]);
  const isInView = useInView(container, {
    margin: `0px 0px -${100 - vertMargin}% 0px`,
    once: true,
  });

  scrollY.on("change", (scrollY) => {
    let animationValue = 1;
    if (scrollY > maxScrollY) {
      animationValue = Math.max(0, 1 - (scrollY - maxScrollY) / 10000);
    }

    scale.set(animationValue);
    filter.set((1 - animationValue) * 100);
  });

  useEffect(() => {
    if (isInView) {
      setMaxScrollY(scrollY.get());
    }
  }, [isInView]);

  return (
    <motion.div
      ref={container}
      className="rounded-4xl sticky h-[200px] w-full max-w-4xl overflow-hidden bg-neutral-200"
      style={{
        scale: scale,
        rotate: filter,
        height: `${100 - 2 * vertMargin}vh`,
        top: `${vertMargin}vh`,
      }}
    >
      <motion.img
        src={imgUrl}
        alt={imgUrl}
        style={{
          rotate: negateFilter,
        }}
        className="h-full w-full scale-125 object-cover"
        sizes="90vw"
      />
    </motion.div>
  );
};

export { Skiper34, StickyCard_003 };

/**
 * Skiper 34 StickyCard_003 — React + framer motion + lenis
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### Scroll progress 001 <a name="scroll-progress-001"></a>

A draggable circular scroll progress indicator that shows scroll percentage with a circular progress bar and animated number counter, positioned at the top of the page

#### Installation

```bash
npx shadcn add @skiper-ui/skiper89
```

#### Dependencies

- **NPM Packages**:
  - `@number-flow/react`
  - `framer-motion`

#### How to Use

```tsx
import { Skiper89 } from "@/components/v1/skiper89";

const DemoSkiper89 = () => {
  return (
    <div className="h-screen w-full">
      <Skiper89 />
    </div>
  );
};
```

#### Component Source Code

##### File: `skiper89.tsx`

```tsx
"use client";

import NumberFlow from "@number-flow/react";
import {
  motion,
  useMotionValueEvent,
  useScroll,
  useTransform,
} from "motion/react";
import { useState } from "react";

import { cn } from "@/lib/utils";

const Skiper89 = () => {
  const { scrollYProgress } = useScroll();
  const [progressPercent, setProgressPercent] = useState(0);

  const clampedProgress = useTransform(scrollYProgress, (value) =>
    Math.min(Math.max(value, 0), 1),
  );
  const progressAsPercent = useTransform(clampedProgress, (value) =>
    Math.round(value * 100),
  );

  useMotionValueEvent(progressAsPercent, "change", (value) => {
    setProgressPercent(value);
  });

  const svgRadius = 18;
  const circumference = 2 * Math.PI * svgRadius;

  return (
    <div
      className={cn(
        "flex w-full max-w-3xl flex-col items-center justify-center gap-[10vh] py-[50vh]",
      )}
    >
      <motion.div
        drag
        dragMomentum={false}
        className={cn(
          "group fixed bottom-4 right-4 cursor-grab items-center gap-1 active:cursor-grabbing",
        )}
      >
        <NumberFlow
          value={progressPercent}
          className={cn(
            "text-foreground/20 absolute top-1 flex h-8 -translate-y-full items-center justify-center px-4 text-xs font-medium tabular-nums opacity-0 group-hover:opacity-100",
          )}
          suffix="%"
        />
        <div className="bg-background/30 flex size-12 items-center justify-center rounded-2xl border backdrop-blur">
          <svg
            className={cn("size-10")}
            viewBox="0 0 48 48"
            role="presentation"
          >
            <circle
              cx="24"
              cy="24"
              r={svgRadius}
              stroke="currentColor"
              strokeWidth="3"
              className={cn("opacity-30")}
              fill="none"
            />
            <motion.circle
              cx="24"
              cy="24"
              r={svgRadius}
              stroke="currentColor"
              strokeWidth="3"
              fill="none"
              strokeLinecap="round"
              strokeDasharray={`${circumference}`}
              style={{
                pathLength: clampedProgress,
                rotate: -90,
                transformOrigin: "50% 50%",
              }}
            />
          </svg>
        </div>
      </motion.div>

      <div className="-mt-10 mb-20 grid content-start justify-items-center gap-6 text-center">
        <span className="after:to-foreground relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-transparent after:content-['']">
          see the progress while scroll
        </span>
      </div>

      {Array.from({ length: 20 }).map((_, index) => (
        <div
          key={index}
          className={cn(
            "text-foreground/70 flex items-center justify-center px-4 text-justify text-base leading-relaxed",
          )}
        >
          Lorem ipsum dolor sit amet, consectetur adipisicing elit. Repellendus,
          fugiat sint eos itaque soluta provident voluptatibus mollitia? Quas
          sit excepturi minima at id nihil consectetur libero, eligendi dicta
          molestias itaque delectus ullam facilis omnis voluptatibus hic
          mollitia deleniti sed earum voluptates reprehenderit commodi porro
          assumenda eum! Doloremque est quasi temporibus!
        </div>
      ))}
    </div>
  );
};

export { Skiper89 };
```

---

### Svg follow scroll <a name="svg-follow-scroll"></a>

A scroll-driven stroke animation that follows scroll progress with a beautiful curved path. Features a dynamic SVG path that fills based on scroll position, creating an engaging visual effect that responds to user scrolling.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper19
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`

#### How to Use

```tsx
import { motion, useTransform } from "framer-motion";

const DemoSkiper19 = () => {
  const { scrollYProgress } = useScroll();

  return (
    <div className="h-[350vh] w-full bg-white">
      <LinePath
        className="absolute inset-0"
        scrollYProgress={scrollYProgress}
      />
    </div>
  );
};

const LinePath = ({
  className,
  scrollYProgress,
}: {
  className: string;
  scrollYProgress: any;
}) => {
  const pathLength = useTransform(scrollYProgress, [0, 1], [0.5, 1]);

  return (
    <svg
      width="1278"
      height="2319"
      viewBox="0 0 1278 2319"
      fill="none"
      overflow="visible"
      xmlns="http://www.w3.org/2000/svg"
      className={className}
    >
      <motion.path
        d="M876.605 394.131C788.982 335.917 696.198 358.139 691.836 416.303C685.453 501.424 853.722 498.43 941.95 409.714C1016.1 335.156 1008.64 186.907 906.167 142.846C807.014 100.212 712.699 198.494 789.049 245.127C889.053 306.207 986.062 116.979 840.548 43.3233C743.932 -5.58141 678.027 57.1682 672.279 112.188C666.53 167.208 712.538 172.943 736.353 163.088C760.167 153.234 764.14 120.924 746.651 93.3868C717.461 47.4252 638.894 77.8642 601.018 116.979C568.164 150.908 557 201.079 576.467 246.924C593.342 286.664 630.24 310.55 671.68 302.614C756.114 286.446 729.747 206.546 681.86 186.442C630.54 164.898 492 209.318 495.026 287.644C496.837 334.494 518.402 366.466 582.455 367.287C680.013 368.538 771.538 299.456 898.634 292.434C1007.02 286.446 1192.67 309.384 1242.36 382.258C1266.99 418.39 1273.65 443.108 1247.75 474.477C1217.32 511.33 1149.4 511.259 1096.84 466.093C1044.29 420.928 1029.14 380.576 1033.97 324.172C1038.31 273.428 1069.55 228.986 1117.2 216.384C1152.2 207.128 1188.29 213.629 1194.45 245.127C1201.49 281.062 1132.22 280.104 1100.44 272.673C1065.32 264.464 1044.22 234.837 1032.77 201.413C1019.29 162.061 1029.71 131.126 1056.44 100.965C1086.19 67.4032 1143.96 54.5526 1175.78 86.1513C1207.02 117.17 1186.81 143.379 1156.22 166.691C1112.57 199.959 1052.57 186.238 999.784 155.164C957.312 130.164 899.171 63.7054 931.284 26.3214C952.068 2.12513 996.288 3.87363 1007.22 43.58C1018.15 83.2749 1003.56 122.644 975.969 163.376C948.377 204.107 907.272 255.122 913.558 321.045C919.727 385.734 990.968 497.068 1063.84 503.35C1111.46 507.456 1166.79 511.984 1175.68 464.527C1191.52 379.956 1101.26 334.985 1030.29 377.017C971.109 412.064 956.297 483.647 953.797 561.655C947.587 755.413 1197.56 941.828 936.039 1140.66C745.771 1285.32 321.926 950.737 134.536 1202.19C-6.68295 1391.68 -53.4837 1655.38 131.935 1760.5C478.381 1956.91 1124.19 1515 1201.28 1997.83C1273.66 2451.23 100.805 1864.7 303.794 2668.89"
        stroke="#C2F84F"
        strokeWidth="20"
        style={{
          pathLength,
          strokeDashoffset: useTransform(pathLength, (value) => 1 - value),
        }}
      />
    </svg>
  );
};
```

#### Component Source Code

##### File: `skiper19.tsx`

```tsx
"use client";

import { motion, useScroll, useTransform } from "framer-motion";
import React, { useRef } from "react";

const Skiper19 = () => {
  const ref = useRef<HTMLDivElement>(null);
  const { scrollYProgress } = useScroll({
    target: ref,
  });

  return (
    <section
      ref={ref}
      className="mx-auto flex h-[350vh] w-screen flex-col items-center overflow-hidden bg-[#FAFDEE] px-4 text-[#1F3A4B]"
    >
      <div className="mt-42 relative flex w-fit flex-col items-center justify-center gap-5 text-center">
        <h1 className="font-jakarta-sans relative z-10 text-7xl font-medium tracking-[-0.08em] lg:text-9xl">
          The Stroke <br /> That follows the <br />
          Scroll Progress
        </h1>
        <p className="font-jakarta-sans relative z-10 max-w-2xl text-xl font-medium text-[#1F3A4B]">
          Scroll down to see the effect
        </p>

        <LinePath
          className="absolute -right-[40%] top-0 z-0"
          scrollYProgress={scrollYProgress}
        />
      </div>

      <div className="rounded-4xl font-jakarta-sans w-full translate-y-[200vh] bg-[#1F3A4B] pb-10 text-[#FAFDEE]">
        <h1 className="mt-10 text-center text-[15.5vw] font-bold leading-[0.9] tracking-tighter lg:text-[16.6vw]">
          skiperui.com
        </h1>
        <div className="mt-80 flex w-full flex-col items-start gap-5 px-4 font-medium lg:mt-0 lg:flex-row lg:justify-between">
          <div className="flex w-full items-center justify-between gap-12 uppercase lg:w-fit lg:justify-center">
            <p className="w-fit text-sm">
              punjab, india <br />
              and online
            </p>
            <p className="w-fit text-right text-sm lg:text-left">
              sep 1, 2025 <br /> the Moosa pind
            </p>
          </div>
          <div className="flex w-full flex-wrap items-center justify-between gap-12 uppercase lg:w-fit lg:justify-center">
            <p className="w-fit text-sm">
              onilne <br /> free
            </p>
            <p className="w-fit text-right text-sm lg:text-left">
              in person tickets <br /> $600
            </p>
          </div>
        </div>
      </div>
    </section>
  );
};

export { Skiper19 };

const LinePath = ({
  className,
  scrollYProgress,
}: {
  className: string;
  scrollYProgress: any;
}) => {
  const pathLength = useTransform(scrollYProgress, [0, 1], [0.5, 1]);

  return (
    <svg
      width="1278"
      height="2319"
      viewBox="0 0 1278 2319"
      fill="none"
      overflow="visible"
      xmlns="http://www.w3.org/2000/svg"
      className={className}
    >
      <motion.path
        d="M876.605 394.131C788.982 335.917 696.198 358.139 691.836 416.303C685.453 501.424 853.722 498.43 941.95 409.714C1016.1 335.156 1008.64 186.907 906.167 142.846C807.014 100.212 712.699 198.494 789.049 245.127C889.053 306.207 986.062 116.979 840.548 43.3233C743.932 -5.58141 678.027 57.1682 672.279 112.188C666.53 167.208 712.538 172.943 736.353 163.088C760.167 153.234 764.14 120.924 746.651 93.3868C717.461 47.4252 638.894 77.8642 601.018 116.979C568.164 150.908 557 201.079 576.467 246.924C593.342 286.664 630.24 310.55 671.68 302.614C756.114 286.446 729.747 206.546 681.86 186.442C630.54 164.898 492 209.318 495.026 287.644C496.837 334.494 518.402 366.466 582.455 367.287C680.013 368.538 771.538 299.456 898.634 292.434C1007.02 286.446 1192.67 309.384 1242.36 382.258C1266.99 418.39 1273.65 443.108 1247.75 474.477C1217.32 511.33 1149.4 511.259 1096.84 466.093C1044.29 420.928 1029.14 380.576 1033.97 324.172C1038.31 273.428 1069.55 228.986 1117.2 216.384C1152.2 207.128 1188.29 213.629 1194.45 245.127C1201.49 281.062 1132.22 280.104 1100.44 272.673C1065.32 264.464 1044.22 234.837 1032.77 201.413C1019.29 162.061 1029.71 131.126 1056.44 100.965C1086.19 67.4032 1143.96 54.5526 1175.78 86.1513C1207.02 117.17 1186.81 143.379 1156.22 166.691C1112.57 199.959 1052.57 186.238 999.784 155.164C957.312 130.164 899.171 63.7054 931.284 26.3214C952.068 2.12513 996.288 3.87363 1007.22 43.58C1018.15 83.2749 1003.56 122.644 975.969 163.376C948.377 204.107 907.272 255.122 913.558 321.045C919.727 385.734 990.968 497.068 1063.84 503.35C1111.46 507.456 1166.79 511.984 1175.68 464.527C1191.52 379.956 1101.26 334.985 1030.29 377.017C971.109 412.064 956.297 483.647 953.797 561.655C947.587 755.413 1197.56 941.828 936.039 1140.66C745.771 1285.32 321.926 950.737 134.536 1202.19C-6.68295 1391.68 -53.4837 1655.38 131.935 1760.5C478.381 1956.91 1124.19 1515 1201.28 1997.83C1273.66 2451.23 100.805 1864.7 303.794 2668.89"
        stroke="#C2F84F"
        strokeWidth="20"
        style={{
          pathLength,
          strokeDashoffset: useTransform(pathLength, (value) => 1 - value),
        }}
      />
    </svg>
  );
};

/**
 * Skiper 19 — React + framer motion
 * Inspired by and adapted from https://comgio.ai/
 * We respect the original creators. This is an inspired rebuild with our own taste and does not claim any ownership.
 * These animations aren’t associated with the comgio.ai . They’re independent recreations meant to study interaction design
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

### Text Scroll animation <a name="text-scroll-animation"></a>

A dynamic scroll-based text and icon animation component featuring three different animation variants. Creates stunning parallax effects with characters and icons that move, rotate, and scale based on scroll position.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper31
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
  - `lenis`

#### How to Use

```tsx
const Skiper31Demo = () => {
  const targetRef = useRef<HTMLDivElement | null>(null);

  const { scrollYProgress } = useScroll({
    target: targetRef,
  });

  return (
   <div
      ref={targetRef}
      className="relative box-border flex h-[210vh] items-center justify-center gap-[2vw] overflow-hidden bg-[#f5f4f3] p-[2vw]"
    >
      <div
        className="font-geist w-full max-w-4xl text-center text-6xl font-bold uppercase tracking-tighter text-black"
        style={{
          perspective: "500px",
        }}
      >
        {["S", "k", "I", "P", "E", "R", "U", "I"].map((char, index) => (
          <CharacterV1
            key={index}
            char={char}
            index={index}
            centerIndex={centerIndex}
            scrollYProgress={scrollYProgress}
          />
        ))}
      </div>
    </div>
  );
```

#### Component Source Code

##### File: `skiper31.tsx`

```tsx
"use client";

import { motion, useScroll, useTransform } from "framer-motion";
import ReactLenis from "lenis/react";
import React, { useRef } from "react";

import { cn } from "@/lib/utils";

type CharacterProps = {
  char: string;
  index: number;
  centerIndex: number;
  scrollYProgress: any;
};

const CharacterV1 = ({
  char,
  index,
  centerIndex,
  scrollYProgress,
}: CharacterProps) => {
  const isSpace = char === " ";

  const distanceFromCenter = index - centerIndex;

  const x = useTransform(
    scrollYProgress,
    [0, 0.5],
    [distanceFromCenter * 50, 0],
  );
  const rotateX = useTransform(
    scrollYProgress,
    [0, 0.5],
    [distanceFromCenter * 50, 0],
  );

  return (
    <motion.span
      className={cn("inline-block text-orange-500", isSpace && "w-4")}
      style={{
        x,
        rotateX,
      }}
    >
      {char}
    </motion.span>
  );
};
const CharacterV2 = ({
  char,
  index,
  centerIndex,
  scrollYProgress,
}: CharacterProps) => {
  const isSpace = char === " ";
  const distanceFromCenter = index - centerIndex;

  const x = useTransform(
    scrollYProgress,
    [0, 0.5],
    [distanceFromCenter * 50, 0],
  );
  const scale = useTransform(scrollYProgress, [0, 0.5], [0.75, 1]);

  const y = useTransform(
    scrollYProgress,
    [0, 0.5],
    [Math.abs(distanceFromCenter) * 50, 0],
  );

  return (
    <motion.img
      src={char}
      className={cn("inline-block", isSpace && "w-4")}
      style={{
        x,
        scale,
        y,
        transformOrigin: "center",
      }}
    />
  );
};
const CharacterV3 = ({
  char,
  index,
  centerIndex,
  scrollYProgress,
}: CharacterProps) => {
  const isSpace = char === " ";
  const distanceFromCenter = index - centerIndex;

  const x = useTransform(
    scrollYProgress,
    [0, 0.5],
    [distanceFromCenter * 90, 0],
  );
  const rotate = useTransform(
    scrollYProgress,
    [0, 0.5],
    [distanceFromCenter * 50, 0],
  );

  const y = useTransform(
    scrollYProgress,
    [0, 0.5],
    [-Math.abs(distanceFromCenter) * 20, 0],
  );
  const scale = useTransform(scrollYProgress, [0, 0.5], [0.75, 1]);

  return (
    <motion.img
      src={char}
      className={cn("inline-block", isSpace && "w-4")}
      style={{
        x,
        rotate,
        y,
        scale,
        transformOrigin: "center",
      }}
    />
  );
};

const Skiper31 = () => {
  const targetRef = useRef<HTMLDivElement | null>(null);
  const targetRef2 = useRef<HTMLDivElement | null>(null);
  const targetRef3 = useRef<HTMLDivElement | null>(null);

  const { scrollYProgress } = useScroll({
    target: targetRef,
  });
  const { scrollYProgress: scrollYProgress2 } = useScroll({
    target: targetRef2,
  });
  const { scrollYProgress: scrollYProgress3 } = useScroll({
    target: targetRef3,
  });

  const text = "see more from gxuri";
  const characters = text.split("");
  const centerIndex = Math.floor(characters.length / 2);

  const macIcon = [
    "/mac/Discord.png",
    "/mac/figma.png",
    "/mac/Framer.png",
    "/mac/Github.png",
    "/mac/Monog.png",
    "/mac/notion.png",
    "/mac/Pieces.png",
    "/mac/Postman.png",
    "/mac/vsCode.png",
  ];
  const iconCenterIndex = Math.floor(macIcon.length / 2);

  return (
    <ReactLenis root>
      <main className="w-full bg-white">
        <div className="top-22 absolute left-1/2 z-10 grid -translate-x-1/2 content-start justify-items-center gap-6 text-center text-black">
          <span className="relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-[#f5f4f3] after:to-black after:content-['']">
            Scroll to see more
          </span>
        </div>
        <div
          ref={targetRef}
          className="relative box-border flex h-[210vh] items-center justify-center gap-[2vw] overflow-hidden bg-[#f5f4f3] p-[2vw]"
        >
          <div
            className="font-geist w-full max-w-4xl text-center text-6xl font-bold uppercase tracking-tighter text-black"
            style={{
              perspective: "500px",
            }}
          >
            {characters.map((char, index) => (
              <CharacterV1
                key={index}
                char={char}
                index={index}
                centerIndex={centerIndex}
                scrollYProgress={scrollYProgress}
              />
            ))}
          </div>
        </div>
        <div
          ref={targetRef2}
          className="relative -mt-[100vh] box-border flex h-[210vh] flex-col items-center justify-center gap-[2vw] overflow-hidden bg-[#f5f4f3] p-[2vw]"
        >
          <p className="font-geist flex items-center justify-center gap-3 text-2xl font-medium tracking-tight text-black">
            <Bracket className="h-12 text-black" />
            <span className="font-geist font-medium">
              intergrate with your fav tech stack
            </span>
            <Bracket className="h-12 scale-x-[-1] text-black" />
          </p>
          <div className="font-geist w-full max-w-4xl text-center text-6xl font-bold uppercase tracking-tighter text-black">
            {macIcon.map((char, index) => (
              <CharacterV2
                key={index}
                char={char}
                index={index}
                centerIndex={iconCenterIndex}
                scrollYProgress={scrollYProgress2}
              />
            ))}
          </div>
        </div>
        <div
          ref={targetRef3}
          className="relative -mt-[95vh] box-border flex h-[210vh] flex-col items-center justify-center gap-[2vw] overflow-hidden bg-[#f5f4f3] p-[2vw]"
        >
          {" "}
          <p className="font-geist flex items-center justify-center gap-3 text-2xl font-medium tracking-tight text-black">
            <Bracket className="h-12 text-black" />
            <span className="font-geist font-medium">
              intergrate with your fav tech stack
            </span>
            <Bracket className="h-12 scale-x-[-1] text-black" />
          </p>
          <div
            className="font-geist w-full max-w-4xl text-center text-6xl font-bold uppercase tracking-tighter text-black"
            style={{
              perspective: "500px",
            }}
          >
            {macIcon.map((char, index) => (
              <CharacterV3
                key={index}
                char={char}
                index={index}
                centerIndex={iconCenterIndex}
                scrollYProgress={scrollYProgress3}
              />
            ))}
          </div>
        </div>
      </main>
    </ReactLenis>
  );
};

export { CharacterV1, CharacterV2, CharacterV3, Skiper31 };

const Bracket = ({ className }: { className: string }) => {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      fill="none"
      viewBox="0 0 27 78"
      className={className}
    >
      <path
        fill="#000"
        d="M26.52 77.21h-5.75c-6.83 0-12.38-5.56-12.38-12.38V48.38C8.39 43.76 4.63 40 .01 40v-4c4.62 0 8.38-3.76 8.38-8.38V12.4C8.38 5.56 13.94 0 20.77 0h5.75v4h-5.75c-4.62 0-8.38 3.76-8.38 8.38V27.6c0 4.34-2.25 8.17-5.64 10.38 3.39 2.21 5.64 6.04 5.64 10.38v16.45c0 4.62 3.76 8.38 8.38 8.38h5.75v4.02Z"
      ></path>
    </svg>
  );
};

/**
 * Skiper 31 ScrollAnimation_002 — React + framer motion + lenis
 *
 * License & Usage:
 * - Free to use and modify in both personal and commercial projects.
 * - Attribution to Skiper UI is required when using the free version.
 * - No attribution required with Skiper UI Pro.
 *
 * Feedback and contributions are welcome.
 *
 * Author: @gurvinder-singh02
 * Website: https://gxuri.in
 * Twitter: https://x.com/Gur__vi
 */

```

---

## Video Players <a name="video-players"></a>

### Video player 001 <a name="video-player-001"></a>

An interactive video player component with a hover effect and modal popup. Features a video thumbnail with a play button overlay that follows the cursor, and opens a full-screen video player modal with custom controls using media-chrome.

#### Installation

```bash
npx shadcn add @skiper-ui/skiper67
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
  - `media-chrome`

#### How to Use

```tsx
import {
  VideoPlayer,
  VideoPlayerContent,
  VideoPlayerControlBar,
  VideoPlayerPlayButton,
  VideoPlayerTimeRange,
  VideoPlayerMuteButton,
} from "@/components/v1/skiper67";

const VideoPlayerExample = () => {
  return (
    <VideoPlayer style={{ width: "100%", height: "100%" }}>
      <VideoPlayerContent
        src="/showreel/skiper-ui-showreel.mp4"
        autoPlay
        slot="media"
        className="w-full object-cover"
      />
      <VideoPlayerControlBar className="absolute bottom-0 left-1/2 flex w-full max-w-7xl -translate-x-1/2 items-center justify-center px-5">
        <VideoPlayerPlayButton className="h-4 bg-transparent" />
        <VideoPlayerTimeRange className="bg-transparent" />
        <VideoPlayerMuteButton className="size-4 bg-transparent" />
      </VideoPlayerControlBar>
    </VideoPlayer>
  );
};
```

#### Component Source Code

##### File: `skiper67.tsx`

```tsx
"use client";

import { AnimatePresence, motion, useSpring } from "framer-motion";
import { Play, Plus } from "lucide-react";
import {
  MediaControlBar,
  MediaController,
  MediaMuteButton,
  MediaPlayButton,
  MediaSeekBackwardButton,
  MediaSeekForwardButton,
  MediaTimeDisplay,
  MediaTimeRange,
  MediaVolumeRange,
} from "media-chrome/react";
import type { ComponentProps } from "react";
import React, { useState } from "react";

import { cn } from "@/lib/utils";

export type VideoPlayerProps = ComponentProps<typeof MediaController>;

export const VideoPlayer = ({ style, ...props }: VideoPlayerProps) => (
  <MediaController
    style={{
      ...style,
    }}
    {...props}
  />
);

export type VideoPlayerControlBarProps = ComponentProps<typeof MediaControlBar>;

export const VideoPlayerControlBar = (props: VideoPlayerControlBarProps) => (
  <MediaControlBar {...props} />
);

export type VideoPlayerTimeRangeProps = ComponentProps<typeof MediaTimeRange>;

export const VideoPlayerTimeRange = ({
  className,
  ...props
}: VideoPlayerTimeRangeProps) => (
  <MediaTimeRange
    className={cn(
      "[--media-range-thumb-opacity:0] [--media-range-track-height:2px]",
      className,
    )}
    {...props}
  />
);

export type VideoPlayerTimeDisplayProps = ComponentProps<
  typeof MediaTimeDisplay
>;

export const VideoPlayerTimeDisplay = ({
  className,
  ...props
}: VideoPlayerTimeDisplayProps) => (
  <MediaTimeDisplay className={cn("p-2.5", className)} {...props} />
);

export type VideoPlayerVolumeRangeProps = ComponentProps<
  typeof MediaVolumeRange
>;

export const VideoPlayerVolumeRange = ({
  className,
  ...props
}: VideoPlayerVolumeRangeProps) => (
  <MediaVolumeRange className={cn("p-2.5", className)} {...props} />
);

export type VideoPlayerPlayButtonProps = ComponentProps<typeof MediaPlayButton>;

export const VideoPlayerPlayButton = ({
  className,
  ...props
}: VideoPlayerPlayButtonProps) => (
  <MediaPlayButton className={cn("", className)} {...props} />
);

export type VideoPlayerSeekBackwardButtonProps = ComponentProps<
  typeof MediaSeekBackwardButton
>;

export const VideoPlayerSeekBackwardButton = ({
  className,
  ...props
}: VideoPlayerSeekBackwardButtonProps) => (
  <MediaSeekBackwardButton className={cn("p-2.5", className)} {...props} />
);

export type VideoPlayerSeekForwardButtonProps = ComponentProps<
  typeof MediaSeekForwardButton
>;

export const VideoPlayerSeekForwardButton = ({
  className,
  ...props
}: VideoPlayerSeekForwardButtonProps) => (
  <MediaSeekForwardButton className={cn("p-2.5", className)} {...props} />
);

export type VideoPlayerMuteButtonProps = ComponentProps<typeof MediaMuteButton>;

export const VideoPlayerMuteButton = ({
  className,
  ...props
}: VideoPlayerMuteButtonProps) => (
  <MediaMuteButton className={cn("", className)} {...props} />
);

export type VideoPlayerContentProps = ComponentProps<"video">;

export const VideoPlayerContent = ({
  className,
  ...props
}: VideoPlayerContentProps) => (
  <video className={cn("mb-0 mt-0", className)} {...props} />
);

export const Skiper67 = () => {
  const [showVideoPopOver, setShowVideoPopOver] = useState(false);

  const SPRING = {
    mass: 0.1,
  };

  const x = useSpring(0, SPRING);
  const y = useSpring(0, SPRING);
  const opacity = useSpring(0, SPRING);

  const handlePointerMove = (e: React.PointerEvent<HTMLDivElement>) => {
    opacity.set(1);
    const bounds = e.currentTarget.getBoundingClientRect();
    x.set(e.clientX - bounds.left);
    y.set(e.clientY - bounds.top);
  };

  return (
    <section className="relative flex h-full w-full items-center justify-center bg-[#f5f4f3]">
      <div className="absolute top-1/4 grid content-start justify-items-center gap-6 text-center">
        <span className="after:to-foreground relative max-w-[12ch] text-xs uppercase leading-tight opacity-40 after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-transparent after:content-['']">
          Click the video to play
        </span>
      </div>
      <AnimatePresence>
        {showVideoPopOver && (
          <VideoPopOver setShowVideoPopOver={setShowVideoPopOver} />
        )}
      </AnimatePresence>
      <div
        onMouseMove={handlePointerMove}
        onMouseLeave={() => {
          opacity.set(0);
        }}
        onClick={() => setShowVideoPopOver(true)}
        className="size-45"
      >
        <motion.div
          style={{ x, y, opacity }}
          className="relative z-20 flex w-fit select-none items-center justify-center gap-2 p-2 text-sm text-white mix-blend-exclusion"
        >
          <Play className="size-4 fill-white" /> Play
        </motion.div>
        <video
          autoPlay
          muted
          playsInline
          loop
          className="h-full w-full object-cover"
        >
          <source src="/showreel/skiper-ui-showreel.mp4" />
        </video>
      </div>
    </section>
  );
};

const VideoPopOver = ({
  setShowVideoPopOver,
}: {
  setShowVideoPopOver: (showVideoPopOver: boolean) => void;
}) => {
  return (
    <div className="fixed left-0 top-0 z-[101] flex h-screen w-screen items-center justify-center">
      <motion.div
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        exit={{ opacity: 0 }}
        transition={{ duration: 0.2 }}
        className="bg-background/90 absolute left-0 top-0 h-full w-full backdrop-blur-lg"
        onClick={() => setShowVideoPopOver(false)}
      ></motion.div>
      <motion.div
        initial={{ clipPath: "inset(43.5% 43.5% 33.5% 43.5% )", opacity: 0 }}
        animate={{ clipPath: "inset(0 0 0 0)", opacity: 1 }}
        exit={{
          clipPath: "inset(43.5% 43.5% 33.5% 43.5% )",
          opacity: 0,
          transition: {
            duration: 1,
            type: "spring",
            stiffness: 100,
            damping: 20,
            opacity: { duration: 0.2, delay: 0.8 },
          },
        }}
        transition={{
          duration: 1,
          type: "spring",
          stiffness: 100,
          damping: 20,
        }}
        className="relative aspect-video max-w-7xl"
      >
        <VideoPlayer style={{ width: "100%", height: "100%" }}>
          <VideoPlayerContent
            src="/showreel/skiper-ui-showreel.mp4"
            autoPlay
            slot="media"
            className="w-full object-cover"
            style={{ width: "100%", height: "100%" }}
          />

          <span
            onClick={() => setShowVideoPopOver(false)}
            className="absolute right-2 top-2 z-10 cursor-pointer rounded-full p-1 transition-colors"
          >
            <Plus className="size-5 rotate-45 text-white" />
          </span>
          <VideoPlayerControlBar className="absolute bottom-0 left-1/2 flex w-full max-w-7xl -translate-x-1/2 items-center justify-center px-5 mix-blend-exclusion md:px-10 md:py-5">
            <VideoPlayerPlayButton className="h-4 bg-transparent" />
            <VideoPlayerTimeRange className="bg-transparent" />
            <VideoPlayerMuteButton className="size-4 bg-transparent" />
          </VideoPlayerControlBar>
        </VideoPlayer>
      </motion.div>
    </div>
  );
};

```

---

