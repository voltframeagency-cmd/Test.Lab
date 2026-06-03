# JolyUI Creative Developer Reference Guide

A complete, structured reference guide for **JolyUI** (jolyui.dev) — a library of stunning, premium interactive React components powered by Framer Motion, Tailwind CSS, Lucide React, and WebGL custom shaders.

## 💎 Special Highlight: JolyUI Liquid Metal Button
The **Liquid Metal Button** in JolyUI is a masterclass in dynamic, responsive WebGL rendering inside a three-dimensional container shell. Here is a breakdown of its features and architectural highlights:

### Key Implementation Highlights:
1. **Interactive WebGL Shader Integration**:
   - The button imports `ShaderMount` and `liquidMetalFragmentShader` dynamically from `@paper-design/shaders`.
   - On hover and click, it directly modifies the internal speed of the shader:
     - Default idle speed: `0.6`
     - Hover speed: `1.0`
     - Click acceleration spike: `2.4` (lasts for `300ms` using a `setTimeout` feedback loop before cooling down back to current state).
2. **3D Layering Depth (`preserve-3d`)**:
   - The component creates a layered holographic look utilizing CSS perspective rendering.
   - It separates the button elements onto multiple Z-planes:
     - **Button Cover & Label**: Placed at `translateZ(20px)` and floats high above the surface.
     - **Inner Plate Shadow**: Placed at `translateZ(10px)`.
     - **Active WebGL Canvas**: Renders on a custom container at `translateZ(0px)`.
     - **Interactive Click Trigger**: Renders at `translateZ(25px)`.
   - When pressed, the container smoothly depresses along the Z-axis: `transform: translateZ(10px) translateY(1px) scale(0.98)`.
3. **Dynamic CSS Ripple Feedback**:
   - Renders a custom radial gradient ripple animation centered precisely at the click coordinates (`e.clientX - rect.left`, `e.clientY - rect.top`) that scales up and fades out dynamically.

### Liquid Metal Button Core Source Code (`docs/registry/default/ui/liquid-metal-button.tsx`)
```tsx
"use client";

import { Sparkles } from "lucide-react";
import type React from "react";
import { useEffect, useMemo, useRef, useState } from "react";

interface LiquidMetalButtonProps {
  label?: string;
  onClick?: () => void;
  viewMode?: "text" | "icon";
}

export function LiquidMetalButton({
  label = "Get Started",
  onClick,
  viewMode = "text",
}: LiquidMetalButtonProps) {
  const [isHovered, setIsHovered] = useState(false);
  const [isPressed, setIsPressed] = useState(false);
  const [ripples, setRipples] = useState<
    Array<{ x: number; y: number; id: number }>
  >([]);
  const shaderRef = useRef<HTMLDivElement>(null);
  // biome-ignore lint/suspicious/noExplicitAny: External library without types
  const shaderMount = useRef<any>(null);
  const buttonRef = useRef<HTMLButtonElement>(null);
  const rippleId = useRef(0);

  const dimensions = useMemo(() => {
    if (viewMode === "icon") {
      return {
        width: 46,
        height: 46,
        innerWidth: 42,
        innerHeight: 42,
        shaderWidth: 46,
        shaderHeight: 46,
      };
    } else {
      return {
        width: 142,
        height: 46,
        innerWidth: 138,
        innerHeight: 42,
        shaderWidth: 142,
        shaderHeight: 46,
      };
    }
  }, [viewMode]);

  useEffect(() => {
    const styleId = "shader-canvas-style-exploded";
    if (!document.getElementById(styleId)) {
      const style = document.createElement("style");
      style.id = styleId;
      style.textContent = `
        .shader-container-exploded canvas {
          width: 100% !important;
          height: 100% !important;
          display: block !important;
          position: absolute !important;
          top: 0 !important;
          left: 0 !important;
          border-radius: 100px !important;
        }
        @keyframes ripple-animation {
          0% {
            transform: translate(-50%, -50%) scale(0);
            opacity: 0.6;
          }
          100% {
            transform: translate(-50%, -50%) scale(4);
            opacity: 0;
          }
        }
      `;
      document.head.appendChild(style);
    }

    const loadShader = async () => {
      try {
        const { liquidMetalFragmentShader, ShaderMount } = await import(
          "@paper-design/shaders"
        );

        if (shaderRef.current) {
          if (shaderMount.current?.destroy) {
            shaderMount.current.destroy();
          }

          shaderMount.current = new ShaderMount(
            shaderRef.current,
            liquidMetalFragmentShader,
            {
              u_repetition: 4,
              u_softness: 0.5,
              u_shiftRed: 0.3,
              u_shiftBlue: 0.3,
              u_distortion: 0,
              u_contour: 0,
              u_angle: 45,
              u_scale: 8,
              u_shape: 1,
              u_offsetX: 0.1,
              u_offsetY: -0.1,
            },
            undefined,
            0.6,
          );
        }
      } catch (error) {
        console.error("[v0] Failed to load shader:", error);
      }
    };

    loadShader();

    return () => {
      if (shaderMount.current?.destroy) {
        shaderMount.current.destroy();
        shaderMount.current = null;
      }
    };
  }, []);

  const handleMouseEnter = () => {
    setIsHovered(true);
    shaderMount.current?.setSpeed?.(1);
  };

  const handleMouseLeave = () => {
    setIsHovered(false);
    setIsPressed(false);
    shaderMount.current?.setSpeed?.(0.6);
  };

  const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
    if (shaderMount.current?.setSpeed) {
      shaderMount.current.setSpeed(2.4);
      setTimeout(() => {
        if (isHovered) {
          shaderMount.current?.setSpeed?.(1);
        } else {
          shaderMount.current?.setSpeed?.(0.6);
        }
      }, 300);
    }

    if (buttonRef.current) {
      const rect = buttonRef.current.getBoundingClientRect();
      const x = e.clientX - rect.left;
      const y = e.clientY - rect.top;
      const ripple = { x, y, id: rippleId.current++ };

      setRipples((prev) => [...prev, ripple]);
      setTimeout(() => {
        setRipples((prev) => prev.filter((r) => r.id !== ripple.id));
      }, 600);
    }

    onClick?.();
  };

  return (
    <div className="relative inline-block">
      <div
        style={{
          perspective: "1000px",
          perspectiveOrigin: "50% 50%",
        }}
      >
        <div
          style={{
            position: "relative",
            width: `${dimensions.width}px`,
            height: `${dimensions.height}px`,
            transformStyle: "preserve-3d",
            transition:
              "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), width 0.4s ease, height 0.4s ease",
            transform: "none",
          }}
        >
          <div
            style={{
              position: "absolute",
              top: 0,
              left: 0,
              width: `${dimensions.width}px`,
              height: `${dimensions.height}px`,
              display: "flex",
              alignItems: "center",
              justifyContent: "center",
              gap: "6px",
              transformStyle: "preserve-3d",
              transition:
                "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), width 0.4s ease, height 0.4s ease, gap 0.4s ease",
              transform: "translateZ(20px)",
              zIndex: 30,
              pointerEvents: "none",
            }}
          >
            {viewMode === "icon" && (
              <Sparkles
                size={16}
                style={{
                  color: "#666666",
                  filter: "drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.5))",
                  transition: "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1)",
                  transform: "scale(1)",
                }}
              />
            )}
            {viewMode === "text" && (
              <span
                style={{
                  fontSize: "14px",
                  color: "#666666",
                  fontWeight: 400,
                  textShadow: "0px 1px 2px rgba(0, 0, 0, 0.5)",
                  transition: "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1)",
                  transform: "scale(1)",
                  whiteSpace: "nowrap",
                }}
              >
                {label}
              </span>
            )}
          </div>

          <div
            style={{
              position: "absolute",
              top: 0,
              left: 0,
              width: `${dimensions.width}px`,
              height: `${dimensions.height}px`,
              transformStyle: "preserve-3d",
              transition:
                "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), width 0.4s ease, height 0.4s ease",
              transform: `translateZ(10px) ${isPressed ? "translateY(1px) scale(0.98)" : "translateY(0) scale(1)"}`,
              zIndex: 20,
            }}
          >
            <div
              style={{
                width: `${dimensions.innerWidth}px`,
                height: `${dimensions.innerHeight}px`,
                margin: "2px",
                borderRadius: "100px",
                background: "linear-gradient(180deg, #202020 0%, #000000 100%)",
                boxShadow: isPressed
                  ? "inset 0px 2px 4px rgba(0, 0, 0, 0.4), inset 0px 1px 2px rgba(0, 0, 0, 0.3)"
                  : "none",
                transition:
                  "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), width 0.4s ease, height 0.4s ease, box-shadow 0.15s cubic-bezier(0.4, 0, 0.2, 1)",
              }}
            />
          </div>

          <div
            style={{
              position: "absolute",
              top: 0,
              left: 0,
              width: `${dimensions.width}px`,
              height: `${dimensions.height}px`,
              transformStyle: "preserve-3d",
              transition:
                "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), width 0.4s ease, height 0.4s ease",
              transform: `translateZ(0px) ${isPressed ? "translateY(1px) scale(0.98)" : "translateY(0) scale(1)"}`,
              zIndex: 10,
            }}
          >
            <div
              style={{
                height: `${dimensions.height}px`,
                width: `${dimensions.width}px`,
                borderRadius: "100px",
                boxShadow: isPressed
                  ? "0px 0px 0px 1px rgba(0, 0, 0, 0.5), 0px 1px 2px 0px rgba(0, 0, 0, 0.3)"
                  : isHovered
                    ? "0px 0px 0px 1px rgba(0, 0, 0, 0.4), 0px 12px 6px 0px rgba(0, 0, 0, 0.05), 0px 8px 5px 0px rgba(0, 0, 0, 0.1), 0px 4px 4px 0px rgba(0, 0, 0, 0.15), 0px 1px 2px 0px rgba(0, 0, 0, 0.2)"
                    : "0px 0px 0px 1px rgba(0, 0, 0, 0.3), 0px 36px 14px 0px rgba(0, 0, 0, 0.02), 0px 20px 12px 0px rgba(0, 0, 0, 0.08), 0px 9px 9px 0px rgba(0, 0, 0, 0.12), 0px 2px 5px 0px rgba(0, 0, 0, 0.15)",
                transition:
                  "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), width 0.4s ease, height 0.4s ease, box-shadow 0.15s cubic-bezier(0.4, 0, 0.2, 1)",
                background: "rgb(0 0 0 / 0)",
              }}
            >
              <div
                ref={shaderRef}
                className="shader-container-exploded"
                style={{
                  borderRadius: "100px",
                  overflow: "hidden",
                  position: "relative",
                  width: `${dimensions.shaderWidth}px`,
                  maxWidth: `${dimensions.shaderWidth}px`,
                  height: `${dimensions.shaderHeight}px`,
                  transition: "width 0.4s ease, height 0.4s ease",
                }}
              />
            </div>
          </div>

          <button
            ref={buttonRef}
            onClick={handleClick}
            onMouseEnter={handleMouseEnter}
            onMouseLeave={handleMouseLeave}
            onMouseDown={() => setIsPressed(true)}
            onMouseUp={() => setIsPressed(false)}
            style={{
              position: "absolute",
              top: 0,
              left: 0,
              width: `${dimensions.width}px`,
              height: `${dimensions.height}px`,
              background: "transparent",
              border: "none",
              cursor: "pointer",
              outline: "none",
              zIndex: 40,
              transformStyle: "preserve-3d",
              transform: "translateZ(25px)",
              transition:
                "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), width 0.4s ease, height 0.4s ease",
              overflow: "hidden",
              borderRadius: "100px",
            }}
            aria-label={label}
          >
            {ripples.map((ripple) => (
              <span
                key={ripple.id}
                style={{
                  position: "absolute",
                  left: `${ripple.x}px`,
                  top: `${ripple.y}px`,
                  width: "20px",
                  height: "20px",
                  borderRadius: "50%",
                  background:
                    "radial-gradient(circle, rgba(255, 255, 255, 0.4) 0%, rgba(255, 255, 255, 0) 70%)",
                  pointerEvents: "none",
                  animation: "ripple-animation 0.6s ease-out",
                }}
              />
            ))}
          </button>
        </div>
      </div>
    </div>
  );
}
```

## Table of Contents
- [AI Prompt Box](#ai-prompt-box)
- [Animated Beam](#animated-beam)
- [Animated Table](#animated-table)
- [Animated Theme Toggle](#animated-theme-toggle)
- [Animated Toast](#animated-toast)
- [Animated Tooltip](#animated-tooltip)
- [Bento Grid](#bento-grid)
- [Button](#button)
- [Calender](#calender)
- [Character Morph](#character-morph)
- [Code Block](#code-block)
- [Command Palette](#command-palette)
- [Date Wheel Picker](#date-wheel-picker)
- [Dock](#dock)
- [Expanded Map](#expanded-map)
- [Falling Text](#falling-text)
- [Feedback Widget](#feedback-widget)
- [File Tree](#file-tree)
- [GitHub Contributors](#github-contributors)
- [GitHub Star Button](#github-star)
- [Glitch Text](#glitch-text)
- [Glowing Button](#glowing-button)
- [Gooey Text Morphing](#gooey-text-morphing)
- [Hero Banner](#hero-banner)
- [Highlight Text](#highlight-text)
- [Hover Preview](#hover-preview)
- [Image Comparison](#image-comparison)
- [Image Sphere](#image-sphere)
- [Lazy Video](#lazy-video)
- [Liquid Metal Button](#liquid-metal-button)
- [Magnetic](#magnetic)
- [Morphing Cursor](#morphing-cursor)
- [Number Counter](#number-counter)
- [Phone Card](#phone-card)
- [Pricing 01](#pricing-01)
- [Rainbow Button](#rainbow-button)
- [Registry](#registry)
- [Rotate Text](#rotate-text)
- [Scroll Text](#scroll-text)
- [Segmented Button](#segmented-button)
- [Shutter Text](#shutter-text)
- [Spline Scene](#spline-scene)
- [Sticky Navbar](#sticky-navbar)
- [Style](#style)
- [Text Morphing](#text-morphing)
- [Theme Daylight](#theme-daylight)
- [Theme Emerald](#theme-emerald)
- [Theme Midnight](#theme-midnight)
- [Typewriter Text](#typewritter-text)
- [Use Mobile](#use-mobile)
- [Utils](#utils)
- [Velocity Morph](#velocity-morph)
- [Vercel Tabs](#vercel-tabs)
- [Video Player](#video-player)

---

## AI Prompt Box <a name="ai-prompt-box"></a>

ChatGPT-style AI prompt input for React. Supports file uploads, voice recording, @mentions, and expandable textarea. Build AI chat interfaces fast.

- **Registry Key**: `ai-prompt-box`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/ai-prompt-box.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `@radix-ui/react-dialog`
  - `@radix-ui/react-tooltip`
  - `lucide-react`

#### How to Use
```tsx
"use client";

import { PromptInputBox } from "@/registry/default/ui/ai-prompt-box";

export default function AiPromptBoxDemo() {
  return (
    <div className="relative flex h-[300px] w-full items-center justify-center">
      <PromptInputBox
        onSend={(message, files) => {
          console.log("Message:", message);
          console.log("Files:", files);
        }}
        placeholder="Ask me anything..."
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/ai-prompt-box.tsx`
```tsx
import * as DialogPrimitive from "@radix-ui/react-dialog";
import * as TooltipPrimitive from "@radix-ui/react-tooltip";
import { AnimatePresence, motion } from "framer-motion";
import {
  ArrowUp,
  BrainCog,
  FolderCode,
  Globe,
  Mic,
  Paperclip,
  Square,
  StopCircle,
  X,
} from "lucide-react";
import Image from "next/image";
import React from "react";

// Utility function for className merging
const cn = (...classes: (string | undefined | null | false)[]) =>
  classes.filter(Boolean).join(" ");

// Embedded CSS for minimal custom styles
const styles = `
  *:focus-visible {
    outline-offset: 0 !important;
    --ring-offset: 0 !important;
  }
  textarea::-webkit-scrollbar {
    width: 6px;
  }
  textarea::-webkit-scrollbar-track {
    background: transparent;
  }
  textarea::-webkit-scrollbar-thumb {
    background-color: #444444;
    border-radius: 3px;
  }
  textarea::-webkit-scrollbar-thumb:hover {
    background-color: #555555;
  }
`;

// Hook to inject styles on client only
const useStyleInjection = () => {
  React.useEffect(() => {
    const styleId = "ai-prompt-box-styles";
    if (typeof document !== "undefined" && !document.getElementById(styleId)) {
      const styleSheet = document.createElement("style");
      styleSheet.id = styleId;
      styleSheet.innerText = styles;
      document.head.appendChild(styleSheet);
    }
  }, []);
};

// Textarea Component
interface TextareaProps
  extends React.TextareaHTMLAttributes<HTMLTextAreaElement> {
  className?: string;
}
const Textarea = React.forwardRef<HTMLTextAreaElement, TextareaProps>(
  ({ className, ...props }, ref) => (
    <textarea
      className={cn(
        "scrollbar-thin scrollbar-thumb-[#444444] scrollbar-track-transparent hover:scrollbar-thumb-[#555555] flex min-h-[44px] w-full resize-none rounded-md border-none bg-transparent px-3 py-2.5 text-base text-gray-100 placeholder:text-gray-400 focus-visible:outline-none focus-visible:ring-0 disabled:cursor-not-allowed disabled:opacity-50",
        className,
      )}
      ref={ref}
      rows={1}
      {...props}
    />
  ),
);
Textarea.displayName = "Textarea";

// Tooltip Components
const TooltipProvider = TooltipPrimitive.Provider;
const Tooltip = TooltipPrimitive.Root;
const TooltipTrigger = TooltipPrimitive.Trigger;
const TooltipContent = React.forwardRef<
  React.ElementRef<typeof TooltipPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof TooltipPrimitive.Content>
>(({ className, sideOffset = 4, ...props }, ref) => (
  <TooltipPrimitive.Content
    ref={ref}
    sideOffset={sideOffset}
    className={cn(
      "fade-in-0 zoom-in-95 data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 z-50 animate-in overflow-hidden rounded-md border border-[#333333] bg-[#1F2023] px-3 py-1.5 text-sm text-white shadow-md data-[state=closed]:animate-out",
      className,
    )}
    {...props}
  />
));
TooltipContent.displayName = TooltipPrimitive.Content.displayName;

// Dialog Components
const Dialog = DialogPrimitive.Root;
const DialogPortal = DialogPrimitive.Portal;
const DialogOverlay = React.forwardRef<
  React.ElementRef<typeof DialogPrimitive.Overlay>,
  React.ComponentPropsWithoutRef<typeof DialogPrimitive.Overlay>
>(({ className, ...props }, ref) => (
  <DialogPrimitive.Overlay
    ref={ref}
    className={cn(
      "data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 fixed inset-0 z-50 bg-black/60 backdrop-blur-sm data-[state=closed]:animate-out data-[state=open]:animate-in",
      className,
    )}
    {...props}
  />
));
DialogOverlay.displayName = DialogPrimitive.Overlay.displayName;

const DialogContent = React.forwardRef<
  React.ElementRef<typeof DialogPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof DialogPrimitive.Content>
>(({ className, children, ...props }, ref) => (
  <DialogPortal>
    <DialogOverlay />
    <DialogPrimitive.Content
      ref={ref}
      className={cn(
        "data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 fixed top-[50%] left-[50%] z-50 grid w-full max-w-[90vw] translate-x-[-50%] translate-y-[-50%] gap-4 rounded-2xl border border-[#333333] bg-[#1F2023] p-0 shadow-xl duration-300 data-[state=closed]:animate-out data-[state=open]:animate-in md:max-w-[800px]",
        className,
      )}
      {...props}
    >
      {children}
      <DialogPrimitive.Close className="absolute top-4 right-4 z-10 rounded-full bg-[#2E3033]/80 p-2 transition-all hover:bg-[#2E3033]">
        <X className="h-5 w-5 text-gray-200 hover:text-white" />
        <span className="sr-only">Close</span>
      </DialogPrimitive.Close>
    </DialogPrimitive.Content>
  </DialogPortal>
));
DialogContent.displayName = DialogPrimitive.Content.displayName;

const DialogTitle = React.forwardRef<
  React.ElementRef<typeof DialogPrimitive.Title>,
  React.ComponentPropsWithoutRef<typeof DialogPrimitive.Title>
>(({ className, ...props }, ref) => (
  <DialogPrimitive.Title
    ref={ref}
    className={cn(
      "font-semibold text-gray-100 text-lg leading-none tracking-tight",
      className,
    )}
    {...props}
  />
));
DialogTitle.displayName = DialogPrimitive.Title.displayName;

// Button Component
interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: "default" | "outline" | "ghost";
  size?: "default" | "sm" | "lg" | "icon";
}
const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant = "default", size = "default", ...props }, ref) => {
    const variantClasses = {
      default: "bg-white hover:bg-white/80 text-black",
      outline: "border border-[#444444] bg-transparent hover:bg-[#3A3A40]",
      ghost: "bg-transparent hover:bg-[#3A3A40]",
    };
    const sizeClasses = {
      default: "h-10 px-4 py-2",
      sm: "h-8 px-3 text-sm",
      lg: "h-12 px-6",
      icon: "h-8 w-8 rounded-full aspect-[1/1]",
    };
    return (
      <button
        className={cn(
          "inline-flex items-center justify-center font-medium transition-colors focus-visible:outline-none disabled:pointer-events-none disabled:opacity-50",
          variantClasses[variant],
          sizeClasses[size],
          className,
        )}
        ref={ref}
        {...props}
      />
    );
  },
);
Button.displayName = "Button";

// VoiceRecorder Component
interface VoiceRecorderProps {
  isRecording: boolean;
  onStartRecording: () => void;
  onStopRecording: (duration: number) => void;
  visualizerBars?: number;
}
const VoiceRecorder: React.FC<VoiceRecorderProps> = ({
  isRecording,
  onStartRecording,
  onStopRecording,
  visualizerBars = 32,
}) => {
  const [time, setTime] = React.useState(0);
  const timerRef = React.useRef<NodeJS.Timeout | null>(null);

  React.useEffect(() => {
    if (isRecording) {
      onStartRecording();
      timerRef.current = setInterval(() => setTime((t) => t + 1), 1000);
    } else {
      if (timerRef.current) {
        clearInterval(timerRef.current);
        timerRef.current = null;
      }
      onStopRecording(time);
      setTime(0);
    }
    return () => {
      if (timerRef.current) clearInterval(timerRef.current);
    };
  }, [isRecording, time, onStartRecording, onStopRecording]);

  const formatTime = (seconds: number) => {
    const mins = Math.floor(seconds / 60);
    const secs = seconds % 60;
    return `${mins.toString().padStart(2, "0")}:${secs.toString().padStart(2, "0")}`;
  };

  return (
    <div
      className={cn(
        "flex w-full flex-col items-center justify-center py-3 transition-all duration-300",
        isRecording ? "opacity-100" : "h-0 opacity-0",
      )}
    >
      <div className="mb-3 flex items-center gap-2">
        <div className="h-2 w-2 animate-pulse rounded-full bg-red-500" />
        <span className="font-mono text-sm text-white/80">
          {formatTime(time)}
        </span>
      </div>
      <div className="flex h-10 w-full items-center justify-center gap-0.5 px-4">
        {[...Array(visualizerBars)].map((_, i) => (
          <div
            key={i}
            className="w-0.5 animate-pulse rounded-full bg-white/50"
            style={{
              height: `${Math.max(15, Math.random() * 100)}%`,
              animationDelay: `${i * 0.05}s`,
              animationDuration: `${0.5 + Math.random() * 0.5}s`,
            }}
          />
        ))}
      </div>
    </div>
  );
};

// ImageViewDialog Component
interface ImageViewDialogProps {
  imageUrl: string | null;
  onClose: () => void;
}
const ImageViewDialog: React.FC<ImageViewDialogProps> = ({
  imageUrl,
  onClose,
}) => {
  if (!imageUrl) return null;
  return (
    <Dialog open={!!imageUrl} onOpenChange={onClose}>
      <DialogContent className="max-w-[90vw] border-none bg-transparent p-0 shadow-none md:max-w-[800px]">
        <DialogTitle className="sr-only">Image Preview</DialogTitle>
        <motion.div
          initial={{ opacity: 0, scale: 0.95 }}
          animate={{ opacity: 1, scale: 1 }}
          exit={{ opacity: 0, scale: 0.95 }}
          transition={{ duration: 0.2, ease: "easeOut" }}
          className="relative overflow-hidden rounded-2xl bg-[#1F2023] shadow-2xl"
        >
          <div className="relative max-h-[80vh] w-full">
            <Image
              src={imageUrl}
              alt="Full preview"
              width={800}
              height={600}
              className="rounded-2xl object-contain"
              unoptimized
            />
          </div>
        </motion.div>
      </DialogContent>
    </Dialog>
  );
};

// PromptInput Context and Components
interface PromptInputContextType {
  isLoading: boolean;
  value: string;
  setValue: (value: string) => void;
  maxHeight: number | string;
  onSubmit?: () => void;
  disabled?: boolean;
}
const PromptInputContext = React.createContext<PromptInputContextType>({
  isLoading: false,
  value: "",
  setValue: () => {},
  maxHeight: 240,
  onSubmit: undefined,
  disabled: false,
});
function usePromptInput() {
  const context = React.useContext(PromptInputContext);
  if (!context)
    throw new Error("usePromptInput must be used within a PromptInput");
  return context;
}

interface PromptInputProps {
  isLoading?: boolean;
  value?: string;
  onValueChange?: (value: string) => void;
  maxHeight?: number | string;
  onSubmit?: () => void;
  children: React.ReactNode;
  className?: string;
  disabled?: boolean;
  onDragOver?: (e: React.DragEvent) => void;
  onDragLeave?: (e: React.DragEvent) => void;
  onDrop?: (e: React.DragEvent) => void;
}
const PromptInput = React.forwardRef<HTMLDivElement, PromptInputProps>(
  (
    {
      className,
      isLoading = false,
      maxHeight = 240,
      value,
      onValueChange,
      onSubmit,
      children,
      disabled = false,
      onDragOver,
      onDragLeave,
      onDrop,
    },
    ref,
  ) => {
    const [internalValue, setInternalValue] = React.useState(value || "");
    const handleChange = (newValue: string) => {
      setInternalValue(newValue);
      onValueChange?.(newValue);
    };
    return (
      <TooltipProvider>
        <PromptInputContext.Provider
          value={{
            isLoading,
            value: value ?? internalValue,
            setValue: onValueChange ?? handleChange,
            maxHeight,
            onSubmit,
            disabled,
          }}
        >
          <div
            ref={ref}
            className={cn(
              "rounded-3xl border border-[#444444] bg-[#1F2023] p-2 shadow-[0_8px_30px_rgba(0,0,0,0.24)] transition-all duration-300",
              isLoading && "border-red-500/70",
              className,
            )}
            onDragOver={onDragOver}
            onDragLeave={onDragLeave}
            onDrop={onDrop}
            role="form"
            aria-label="Prompt Input Area"
          >
            {children}
          </div>
        </PromptInputContext.Provider>
      </TooltipProvider>
    );
  },
);
PromptInput.displayName = "PromptInput";

interface PromptInputTextareaProps {
  disableAutosize?: boolean;
  placeholder?: string;
}
const PromptInputTextarea: React.FC<
  PromptInputTextareaProps & React.ComponentProps<typeof Textarea>
> = ({
  className,
  onKeyDown,
  disableAutosize = false,
  placeholder,
  ...props
}) => {
  const { value, setValue, maxHeight, onSubmit, disabled } = usePromptInput();
  const textareaRef = React.useRef<HTMLTextAreaElement>(null);

  React.useEffect(() => {
    if (disableAutosize || !textareaRef.current) return;
    textareaRef.current.style.height = "auto";
    textareaRef.current.style.height =
      typeof maxHeight === "number"
        ? `${Math.min(textareaRef.current.scrollHeight, maxHeight)}px`
        : `min(${textareaRef.current.scrollHeight}px, ${maxHeight})`;
  }, [maxHeight, disableAutosize]);

  const handleKeyDown = (e: React.KeyboardEvent<HTMLTextAreaElement>) => {
    if (e.key === "Enter" && !e.shiftKey) {
      e.preventDefault();
      onSubmit?.();
    }
    onKeyDown?.(e);
  };

  return (
    <Textarea
      ref={textareaRef}
      value={value}
      onChange={(e) => setValue(e.target.value)}
      onKeyDown={handleKeyDown}
      className={cn("text-base", className)}
      disabled={disabled}
      placeholder={placeholder}
      {...props}
    />
  );
};

interface PromptInputActionsProps
  extends React.HTMLAttributes<HTMLDivElement> {}
const PromptInputActions: React.FC<PromptInputActionsProps> = ({
  children,
  className,
  ...props
}) => (
  <div className={cn("flex items-center gap-2", className)} {...props}>
    {children}
  </div>
);

interface PromptInputActionProps extends React.ComponentProps<typeof Tooltip> {
  tooltip: React.ReactNode;
  children: React.ReactNode;
  side?: "top" | "bottom" | "left" | "right";
  className?: string; // Add className
}
const PromptInputAction: React.FC<PromptInputActionProps> = ({
  tooltip,
  children,
  className,
  side = "top",
  ...props
}) => {
  const { disabled } = usePromptInput();
  return (
    <Tooltip {...props}>
      <TooltipTrigger asChild disabled={disabled}>
        {children}
      </TooltipTrigger>
      <TooltipContent side={side} className={className}>
        {tooltip}
      </TooltipContent>
    </Tooltip>
  );
};

// Custom Divider Component
const CustomDivider: React.FC = () => (
  <div className="relative mx-1 h-6 w-[1.5px]">
    <div
      className="absolute inset-0 rounded-full bg-gradient-to-t from-transparent via-[#9b87f5]/70 to-transparent"
      style={{
        clipPath:
          "polygon(0% 0%, 100% 0%, 100% 40%, 140% 50%, 100% 60%, 100% 100%, 0% 100%, 0% 60%, -40% 50%, 0% 40%)",
      }}
    />
  </div>
);

// Main PromptInputBox Component
interface PromptInputBoxProps {
  onSend?: (message: string, files?: File[]) => void;
  isLoading?: boolean;
  placeholder?: string;
  className?: string;
}
export const PromptInputBox = React.forwardRef(
  (props: PromptInputBoxProps, ref: React.Ref<HTMLDivElement>) => {
    const {
      onSend = () => {},
      isLoading = false,
      placeholder = "Type your message here...",
      className,
    } = props;

    // Inject styles on client only
    useStyleInjection();

    const [input, setInput] = React.useState("");
    const [files, setFiles] = React.useState<File[]>([]);
    const [filePreviews, setFilePreviews] = React.useState<{
      [key: string]: string;
    }>({});
    const [selectedImage, setSelectedImage] = React.useState<string | null>(
      null,
    );
    const [isRecording, setIsRecording] = React.useState(false);
    const [showSearch, setShowSearch] = React.useState(false);
    const [showThink, setShowThink] = React.useState(false);
    const [showCanvas, setShowCanvas] = React.useState(false);
    const uploadInputRef = React.useRef<HTMLInputElement>(null);
    const promptBoxRef = React.useRef<HTMLDivElement>(null);

    const handleToggleChange = (value: string) => {
      if (value === "search") {
        setShowSearch((prev) => !prev);
        setShowThink(false);
      } else if (value === "think") {
        setShowThink((prev) => !prev);
        setShowSearch(false);
      }
    };

    const handleCanvasToggle = () => setShowCanvas((prev) => !prev);

    const isImageFile = (file: File) => file.type.startsWith("image/");

    const processFile = React.useCallback((file: File) => {
      if (!file.type.startsWith("image/")) {
        console.log("Only image files are allowed");
        return;
      }
      if (file.size > 10 * 1024 * 1024) {
        console.log("File too large (max 10MB)");
        return;
      }
      setFiles([file]);
      const reader = new FileReader();
      reader.onload = (e) =>
        setFilePreviews({ [file.name]: e.target?.result as string });
      reader.readAsDataURL(file);
    }, []);

    const handleDragOver = React.useCallback((e: React.DragEvent) => {
      e.preventDefault();
      e.stopPropagation();
    }, []);

    const handleDragLeave = React.useCallback((e: React.DragEvent) => {
      e.preventDefault();
      e.stopPropagation();
    }, []);

    const handleDrop = React.useCallback(
      (e: React.DragEvent) => {
        e.preventDefault();
        e.stopPropagation();
        const files = Array.from(e.dataTransfer.files);
        const imageFiles = files.filter((file) => isImageFile(file));
        if (imageFiles.length > 0 && imageFiles[0]) processFile(imageFiles[0]);
      },
      [isImageFile, processFile],
    );

    const handleRemoveFile = (index: number) => {
      const fileToRemove = files[index];
      if (fileToRemove && filePreviews[fileToRemove.name]) setFilePreviews({});
      setFiles([]);
    };

    const openImageModal = (imageUrl: string) => setSelectedImage(imageUrl);

    const handlePaste = React.useCallback(
      (e: ClipboardEvent) => {
        const items = e.clipboardData?.items;
        if (!items) return;
        for (let i = 0; i < items.length; i++) {
          const item = items[i];
          if (item && item.type.indexOf("image") !== -1) {
            const file = item.getAsFile();
            if (file) {
              e.preventDefault();
              processFile(file);
              break;
            }
          }
        }
      },
      [processFile],
    );

    React.useEffect(() => {
      document.addEventListener("paste", handlePaste);
      return () => document.removeEventListener("paste", handlePaste);
    }, [handlePaste]);

    const handleSubmit = () => {
      if (input.trim() || files.length > 0) {
        let messagePrefix = "";
        if (showSearch) messagePrefix = "[Search: ";
        else if (showThink) messagePrefix = "[Think: ";
        else if (showCanvas) messagePrefix = "[Canvas: ";
        const formattedInput = messagePrefix
          ? `${messagePrefix}${input}]`
          : input;
        onSend(formattedInput, files);
        setInput("");
        setFiles([]);
        setFilePreviews({});
      }
    };

    const handleStartRecording = () => console.log("Started recording");

    const handleStopRecording = (duration: number) => {
      console.log(`Stopped recording after ${duration} seconds`);
      setIsRecording(false);
      onSend(`[Voice message - ${duration} seconds]`, []);
    };

    const hasContent = input.trim() !== "" || files.length > 0;

    return (
      <>
        <PromptInput
          value={input}
          onValueChange={setInput}
          isLoading={isLoading}
          onSubmit={handleSubmit}
          className={cn(
            "w-full border-[#444444] bg-[#1F2023] shadow-[0_8px_30px_rgba(0,0,0,0.24)] transition-all duration-300 ease-in-out",
            isRecording && "border-red-500/70",
            className,
          )}
          disabled={isLoading || isRecording}
          ref={ref || promptBoxRef}
          onDragOver={handleDragOver}
          onDragLeave={handleDragLeave}
          onDrop={handleDrop}
        >
          {files.length > 0 && !isRecording && (
            <div className="flex flex-wrap gap-2 p-0 pb-1 transition-all duration-300">
              {files.map((file, index) => (
                <div key={index} className="group relative">
                  {file.type.startsWith("image/") &&
                    filePreviews[file.name] && (
                      <div
                        className="h-16 w-16 cursor-pointer overflow-hidden rounded-xl transition-all duration-300"
                        onClick={() => {
                          const preview = filePreviews[file.name];
                          if (preview) openImageModal(preview);
                        }}
                        role="button"
                        tabIndex={0}
                        onKeyDown={(e) => {
                          if (e.key === "Enter" || e.key === " ") {
                            const preview = filePreviews[file.name];
                            if (preview) openImageModal(preview);
                          }
                        }}
                      >
                        {filePreviews[file.name] && (
                          <Image
                            src={filePreviews[file.name] || ""}
                            alt={file.name}
                            width={64}
                            height={64}
                            className="h-full w-full object-cover"
                            unoptimized
                          />
                        )}
                        <button
                          onClick={(e) => {
                            e.stopPropagation();
                            handleRemoveFile(index);
                          }}
                          className="absolute top-1 right-1 rounded-full bg-black/70 p-0.5 opacity-100 transition-opacity"
                        >
                          <X className="h-3 w-3 text-white" />
                        </button>
                      </div>
                    )}
                </div>
              ))}
            </div>
          )}

          <div
            className={cn(
              "transition-all duration-300",
              isRecording ? "h-0 overflow-hidden opacity-0" : "opacity-100",
            )}
          >
            <PromptInputTextarea
              placeholder={
                showSearch
                  ? "Search the web..."
                  : showThink
                    ? "Think deeply..."
                    : showCanvas
                      ? "Create on canvas..."
                      : placeholder
              }
              className="text-base"
            />
          </div>

          {isRecording && (
            <VoiceRecorder
              isRecording={isRecording}
              onStartRecording={handleStartRecording}
              onStopRecording={handleStopRecording}
            />
          )}

          <PromptInputActions className="flex items-center justify-between gap-2 p-0 pt-2">
            <div
              className={cn(
                "flex items-center gap-1 transition-opacity duration-300",
                isRecording ? "invisible h-0 opacity-0" : "visible opacity-100",
              )}
            >
              <PromptInputAction tooltip="Upload image">
                <button
                  onClick={() => uploadInputRef.current?.click()}
                  className="flex h-8 w-8 cursor-pointer items-center justify-center rounded-full text-[#9CA3AF] transition-colors hover:bg-gray-600/30 hover:text-[#D1D5DB]"
                  disabled={isRecording}
                >
                  <Paperclip className="h-5 w-5 transition-colors" />
                  <input
                    ref={uploadInputRef}
                    type="file"
                    className="hidden"
                    onChange={(e) => {
                      if (e.target.files && e.target.files.length > 0) {
                        const file = e.target.files[0];
                        if (file) processFile(file);
                      }
                      if (e.target) e.target.value = "";
                    }}
                    accept="image/*"
                  />
                </button>
              </PromptInputAction>

              <div className="flex items-center">
                <button
                  type="button"
                  onClick={() => handleToggleChange("search")}
                  className={cn(
                    "flex h-8 items-center gap-1 rounded-full border px-2 py-1 transition-all",
                    showSearch
                      ? "border-[#1EAEDB] bg-[#1EAEDB]/15 text-[#1EAEDB]"
                      : "border-transparent bg-transparent text-[#9CA3AF] hover:text-[#D1D5DB]",
                  )}
                >
                  <div className="flex h-5 w-5 flex-shrink-0 items-center justify-center">
                    <motion.div
                      animate={{
                        rotate: showSearch ? 360 : 0,
                        scale: showSearch ? 1.1 : 1,
                      }}
                      whileHover={{
                        rotate: showSearch ? 360 : 15,
                        scale: 1.1,
                        transition: {
                          type: "spring",
                          stiffness: 300,
                          damping: 10,
                        },
                      }}
                      transition={{
                        type: "spring",
                        stiffness: 260,
                        damping: 25,
                      }}
                    >
                      <Globe
                        className={cn(
                          "h-4 w-4",
                          showSearch ? "text-[#1EAEDB]" : "text-inherit",
                        )}
                      />
                    </motion.div>
                  </div>
                  <AnimatePresence>
                    {showSearch && (
                      <motion.span
                        initial={{ width: 0, opacity: 0 }}
                        animate={{ width: "auto", opacity: 1 }}
                        exit={{ width: 0, opacity: 0 }}
                        transition={{ duration: 0.2 }}
                        className="flex-shrink-0 overflow-hidden whitespace-nowrap text-[#1EAEDB] text-xs"
                      >
                        Search
                      </motion.span>
                    )}
                  </AnimatePresence>
                </button>

                <CustomDivider />

                <button
                  type="button"
                  onClick={() => handleToggleChange("think")}
                  className={cn(
                    "flex h-8 items-center gap-1 rounded-full border px-2 py-1 transition-all",
                    showThink
                      ? "border-[#8B5CF6] bg-[#8B5CF6]/15 text-[#8B5CF6]"
                      : "border-transparent bg-transparent text-[#9CA3AF] hover:text-[#D1D5DB]",
                  )}
                >
                  <div className="flex h-5 w-5 flex-shrink-0 items-center justify-center">
                    <motion.div
                      animate={{
                        rotate: showThink ? 360 : 0,
                        scale: showThink ? 1.1 : 1,
                      }}
                      whileHover={{
                        rotate: showThink ? 360 : 15,
                        scale: 1.1,
                        transition: {
                          type: "spring",
                          stiffness: 300,
                          damping: 10,
                        },
                      }}
                      transition={{
                        type: "spring",
                        stiffness: 260,
                        damping: 25,
                      }}
                    >
                      <BrainCog
                        className={cn(
                          "h-4 w-4",
                          showThink ? "text-[#8B5CF6]" : "text-inherit",
                        )}
                      />
                    </motion.div>
                  </div>
                  <AnimatePresence>
                    {showThink && (
                      <motion.span
                        initial={{ width: 0, opacity: 0 }}
                        animate={{ width: "auto", opacity: 1 }}
                        exit={{ width: 0, opacity: 0 }}
                        transition={{ duration: 0.2 }}
                        className="flex-shrink-0 overflow-hidden whitespace-nowrap text-[#8B5CF6] text-xs"
                      >
                        Think
                      </motion.span>
                    )}
                  </AnimatePresence>
                </button>

                <CustomDivider />

                <button
                  type="button"
                  onClick={handleCanvasToggle}
                  className={cn(
                    "flex h-8 items-center gap-1 rounded-full border px-2 py-1 transition-all",
                    showCanvas
                      ? "border-[#F97316] bg-[#F97316]/15 text-[#F97316]"
                      : "border-transparent bg-transparent text-[#9CA3AF] hover:text-[#D1D5DB]",
                  )}
                >
                  <div className="flex h-5 w-5 flex-shrink-0 items-center justify-center">
                    <motion.div
                      animate={{
                        rotate: showCanvas ? 360 : 0,
                        scale: showCanvas ? 1.1 : 1,
                      }}
                      whileHover={{
                        rotate: showCanvas ? 360 : 15,
                        scale: 1.1,
                        transition: {
                          type: "spring",
                          stiffness: 300,
                          damping: 10,
                        },
                      }}
                      transition={{
                        type: "spring",
                        stiffness: 260,
                        damping: 25,
                      }}
                    >
                      <FolderCode
                        className={cn(
                          "h-4 w-4",
                          showCanvas ? "text-[#F97316]" : "text-inherit",
                        )}
                      />
                    </motion.div>
                  </div>
                  <AnimatePresence>
                    {showCanvas && (
                      <motion.span
                        initial={{ width: 0, opacity: 0 }}
                        animate={{ width: "auto", opacity: 1 }}
                        exit={{ width: 0, opacity: 0 }}
                        transition={{ duration: 0.2 }}
                        className="flex-shrink-0 overflow-hidden whitespace-nowrap text-[#F97316] text-xs"
                      >
                        Canvas
                      </motion.span>
                    )}
                  </AnimatePresence>
                </button>
              </div>
            </div>

            <PromptInputAction
              tooltip={
                isLoading
                  ? "Stop generation"
                  : isRecording
                    ? "Stop recording"
                    : hasContent
                      ? "Send message"
                      : "Voice message"
              }
            >
              <Button
                variant="default"
                size="icon"
                className={cn(
                  "h-8 w-8 rounded-full transition-all duration-200",
                  isRecording
                    ? "bg-transparent text-red-500 hover:bg-gray-600/30 hover:text-red-400"
                    : hasContent
                      ? "bg-white text-[#1F2023] hover:bg-white/80"
                      : "bg-transparent text-[#9CA3AF] hover:bg-gray-600/30 hover:text-[#D1D5DB]",
                )}
                onClick={() => {
                  if (isRecording) setIsRecording(false);
                  else if (hasContent) handleSubmit();
                  else setIsRecording(true);
                }}
                disabled={isLoading && !hasContent}
              >
                {isLoading ? (
                  <Square className="h-4 w-4 animate-pulse fill-[#1F2023]" />
                ) : isRecording ? (
                  <StopCircle className="h-5 w-5 text-red-500" />
                ) : hasContent ? (
                  <ArrowUp className="h-4 w-4 text-[#1F2023]" />
                ) : (
                  <Mic className="h-5 w-5 text-[#1F2023] transition-colors" />
                )}
              </Button>
            </PromptInputAction>
          </PromptInputActions>
        </PromptInput>

        <ImageViewDialog
          imageUrl={selectedImage}
          onClose={() => setSelectedImage(null)}
        />
      </>
    );
  },
);
PromptInputBox.displayName = "PromptInputBox";
```
---

## Animated Beam <a name="animated-beam"></a>

SVG-based animated beam component for React. Create stunning connection lines between elements with customizable curves, gradients, and pulse effects.

- **Registry Key**: `animated-beam`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/animated-beam.json
```

#### Dependencies
- **NPM Packages**:
  - `lucide-react`

#### How to Use
```tsx
import { Bot, Search, User, Zap } from "lucide-react";
import React from "react";
import {
  AnimatedBeam,
  BeamContainer,
  BeamNode,
} from "@/registry/default/ui/animated-beam";

export default function AnimatedBeamDemo() {
  const containerRef = React.useRef<HTMLDivElement>(null);
  const userRef = React.useRef<HTMLDivElement>(null);
  const aiRef = React.useRef<HTMLDivElement>(null);
  const searchRef = React.useRef<HTMLDivElement>(null);
  const resultRef = React.useRef<HTMLDivElement>(null);

  return (
    <BeamContainer
      ref={containerRef}
      className="mx-auto flex w-full items-center justify-between rounded-xl border bg-background p-10 shadow-sm"
    >
      <div className="flex flex-col items-center gap-2">
        <BeamNode
          ref={userRef}
          className="h-12 w-12 border-2 border-blue-500/20 bg-blue-500/10"
        >
          <User className="h-6 w-6 text-blue-600" />
        </BeamNode>
        <span className="font-medium text-[10px] text-muted-foreground uppercase tracking-wider">
          User
        </span>
      </div>

      <div className="flex flex-col items-center gap-2">
        <BeamNode
          ref={aiRef}
          className="h-16 w-16 border-2 border-purple-500/20 bg-purple-500/10 shadow-[0_0_15px_rgba(168,85,247,0.2)]"
        >
          <Bot className="h-8 w-8 text-purple-600" />
        </BeamNode>
        <span className="font-medium text-[10px] text-muted-foreground uppercase tracking-wider">
          AI Agent
        </span>
      </div>

      <div className="flex flex-col gap-8">
        <div className="flex flex-col items-center gap-2">
          <BeamNode
            ref={searchRef}
            className="h-12 w-12 border-2 border-amber-500/20 bg-amber-500/10"
          >
            <Search className="h-5 w-5 text-amber-600" />
          </BeamNode>
          <span className="font-medium text-[10px] text-muted-foreground uppercase tracking-wider">
            Search
          </span>
        </div>
        <div className="flex flex-col items-center gap-2">
          <BeamNode
            ref={resultRef}
            className="h-12 w-12 border-2 border-emerald-500/20 bg-emerald-500/10"
          >
            <Zap className="h-5 w-5 text-emerald-600" />
          </BeamNode>
          <span className="font-medium text-[10px] text-muted-foreground uppercase tracking-wider">
            Result
          </span>
        </div>
      </div>

      <AnimatedBeam
        containerRef={containerRef}
        fromRef={userRef}
        toRef={aiRef}
        duration={3}
        curvature={0.2}
        gradientStartColor="#3b82f6"
        gradientStopColor="#8b5cf6"
      />
      <AnimatedBeam
        containerRef={containerRef}
        fromRef={aiRef}
        toRef={searchRef}
        duration={3}
        delay={0.5}
        curvature={-0.3}
        gradientStartColor="#8b5cf6"
        gradientStopColor="#f59e0b"
      />
      <AnimatedBeam
        containerRef={containerRef}
        fromRef={searchRef}
        toRef={aiRef}
        duration={3}
        delay={1.5}
        curvature={-0.3}
        reverse
        gradientStartColor="#f59e0b"
        gradientStopColor="#8b5cf6"
      />
      <AnimatedBeam
        containerRef={containerRef}
        fromRef={aiRef}
        toRef={resultRef}
        duration={3}
        delay={2.5}
        curvature={0.3}
        gradientStartColor="#8b5cf6"
        gradientStopColor="#10b981"
      />
    </BeamContainer>
  );
}
```
#### Component Source Code
##### File: `ui/animated-beam.tsx`
```tsx
import * as React from "react";
import { cn } from "@/lib/utils";

interface AnimatedBeamProps {
  /** Reference to the container element */
  containerRef: React.RefObject<HTMLElement | null>;
  /** Reference to the start element */
  fromRef: React.RefObject<HTMLElement | null>;
  /** Reference to the end element */
  toRef: React.RefObject<HTMLElement | null>;
  /** Curvature of the beam (-1 to 1, 0 is straight) */
  curvature?: number;
  /** Animation duration in seconds */
  duration?: number;
  /** Delay before animation starts */
  delay?: number;
  /** Reverse the animation direction */
  reverse?: boolean;
  /** Width of the beam path */
  pathWidth?: number;
  /** Color of the beam gradient start */
  gradientStartColor?: string;
  /** Color of the beam gradient end */
  gradientStopColor?: string;
  /** Starting point offset */
  startXOffset?: number;
  startYOffset?: number;
  /** Ending point offset */
  endXOffset?: number;
  endYOffset?: number;
  className?: string;
}

const AnimatedBeam = ({
  containerRef,
  fromRef,
  toRef,
  curvature = 0,
  duration = 2,
  delay = 0,
  reverse = false,
  pathWidth = 2,
  gradientStartColor = "#18181b",
  gradientStopColor = "#18181b",
  startXOffset = 0,
  startYOffset = 0,
  endXOffset = 0,
  endYOffset = 0,
  className,
}: AnimatedBeamProps) => {
  const [pathD, setPathD] = React.useState("");
  const [svgDimensions, setSvgDimensions] = React.useState({
    width: 0,
    height: 0,
  });
  const uniqueId = React.useId();

  const updatePath = React.useCallback(() => {
    if (!containerRef.current || !fromRef.current || !toRef.current) return;

    const containerRect = containerRef.current.getBoundingClientRect();
    const fromRect = fromRef.current.getBoundingClientRect();
    const toRect = toRef.current.getBoundingClientRect();

    const startX =
      fromRect.left - containerRect.left + fromRect.width / 2 + startXOffset;
    const startY =
      fromRect.top - containerRect.top + fromRect.height / 2 + startYOffset;
    const endX =
      toRect.left - containerRect.left + toRect.width / 2 + endXOffset;
    const endY =
      toRect.top - containerRect.top + toRect.height / 2 + endYOffset;

    const midX = (startX + endX) / 2;
    const midY = (startY + endY) / 2;

    // Calculate control point for quadratic bezier curve
    const dx = endX - startX;
    const dy = endY - startY;
    const controlX = midX - dy * curvature;
    const controlY = midY + dx * curvature;

    const path = `M ${startX},${startY} Q ${controlX},${controlY} ${endX},${endY}`;
    setPathD(path);
    setSvgDimensions({
      width: containerRect.width,
      height: containerRect.height,
    });
  }, [
    containerRef,
    fromRef,
    toRef,
    curvature,
    startXOffset,
    startYOffset,
    endXOffset,
    endYOffset,
  ]);

  React.useEffect(() => {
    updatePath();

    const resizeObserver = new ResizeObserver(updatePath);
    if (containerRef.current) {
      resizeObserver.observe(containerRef.current);
    }

    window.addEventListener("resize", updatePath);

    return () => {
      resizeObserver.disconnect();
      window.removeEventListener("resize", updatePath);
    };
  }, [updatePath, containerRef]);

  return (
    <svg
      className={cn(
        "pointer-events-none absolute top-0 left-0 h-full w-full",
        className,
      )}
      width={svgDimensions.width}
      height={svgDimensions.height}
      viewBox={`0 0 ${svgDimensions.width} ${svgDimensions.height}`}
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
    >
      <defs>
        {/* Background path gradient */}
        <linearGradient
          id={`beam-gradient-bg-${uniqueId}`}
          gradientUnits="userSpaceOnUse"
          x1="0%"
          y1="0%"
          x2="100%"
          y2="0%"
        >
          <stop offset="0%" stopColor={gradientStartColor} stopOpacity="0.1" />
          <stop offset="50%" stopColor={gradientStartColor} stopOpacity="0.2" />
          <stop offset="100%" stopColor={gradientStopColor} stopOpacity="0.1" />
        </linearGradient>

        {/* Animated beam gradient */}
        <linearGradient
          id={`beam-gradient-${uniqueId}`}
          gradientUnits="userSpaceOnUse"
          x1="0%"
          y1="0%"
          x2="100%"
          y2="0%"
        >
          <stop offset="0%" stopColor={gradientStartColor} stopOpacity="0" />
          <stop offset="5%" stopColor={gradientStartColor} stopOpacity="1" />
          <stop offset="50%" stopColor={gradientStopColor} stopOpacity="1" />
          <stop offset="95%" stopColor={gradientStopColor} stopOpacity="1" />
          <stop offset="100%" stopColor={gradientStopColor} stopOpacity="0" />
        </linearGradient>

        {/* Glow filter */}
        <filter
          id={`beam-glow-${uniqueId}`}
          x="-50%"
          y="-50%"
          width="200%"
          height="200%"
        >
          <feGaussianBlur in="SourceGraphic" stdDeviation="2" result="blur" />
          <feMerge>
            <feMergeNode in="blur" />
            <feMergeNode in="SourceGraphic" />
          </feMerge>
        </filter>

        {/* Mask for animated beam */}
        <mask id={`beam-mask-${uniqueId}`}>
          <rect
            className="beam-mask-rect"
            x="-100%"
            y="0"
            width="50%"
            height="100%"
            fill="url(#beam-mask-gradient)"
            style={{
              animation: `beam-flow ${duration}s linear infinite`,
              animationDelay: `${delay}s`,
              animationDirection: reverse ? "reverse" : "normal",
            }}
          />
        </mask>

        <linearGradient
          id="beam-mask-gradient"
          x1="0%"
          y1="0%"
          x2="100%"
          y2="0%"
        >
          <stop offset="0%" stopColor="black" />
          <stop offset="25%" stopColor="white" />
          <stop offset="75%" stopColor="white" />
          <stop offset="100%" stopColor="black" />
        </linearGradient>
      </defs>

      {/* Background path */}
      <path
        d={pathD}
        stroke={`url(#beam-gradient-bg-${uniqueId})`}
        strokeWidth={pathWidth}
        strokeLinecap="round"
        fill="none"
      />

      {/* Animated glowing beam */}
      <path
        d={pathD}
        stroke={`url(#beam-gradient-${uniqueId})`}
        strokeWidth={pathWidth}
        strokeLinecap="round"
        fill="none"
        filter={`url(#beam-glow-${uniqueId})`}
        className="animated-beam-path"
        style={{
          strokeDasharray: "20 1000",
          strokeDashoffset: reverse ? "-1000" : "1000",
          animation: `beam-dash ${duration}s linear infinite`,
          animationDelay: `${delay}s`,
          animationDirection: reverse ? "reverse" : "normal",
        }}
      />
    </svg>
  );
};

interface BeamContainerProps extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode;
}

const BeamContainer = React.forwardRef<HTMLDivElement, BeamContainerProps>(
  ({ children, className, ...props }, ref) => {
    return (
      <div ref={ref} className={cn("relative", className)} {...props}>
        {children}
      </div>
    );
  },
);
BeamContainer.displayName = "BeamContainer";

interface BeamNodeProps extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode;
}

const BeamNode = React.forwardRef<HTMLDivElement, BeamNodeProps>(
  ({ children, className, ...props }, ref) => {
    return (
      <div
        ref={ref}
        className={cn(
          "relative z-10 flex items-center justify-center rounded-xl border bg-background p-3 shadow-sm",
          className,
        )}
        {...props}
      >
        {children}
      </div>
    );
  },
);
BeamNode.displayName = "BeamNode";

export {
  AnimatedBeam,
  BeamContainer,
  BeamNode,
  type AnimatedBeamProps,
  type BeamContainerProps,
  type BeamNodeProps,
};
```
---

## Animated Table <a name="animated-table"></a>

React data table with sorting, filtering, pagination, and row expansion. Smooth animations on interactions. Built on Radix primitives.

- **Registry Key**: `animated-table`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/animated-table.json
```

#### Dependencies
- **NPM Packages**:
  - `lucide-react`
  - `motion`

#### Component Source Code
##### File: `ui/animated-table.tsx`
```tsx
import {
  Check,
  ChevronDown,
  ChevronLeft,
  ChevronRight,
  ChevronsLeft,
  ChevronsRight,
  ChevronsUpDown,
  ChevronUp,
  Columns3,
  Search,
  X,
} from "lucide-react";
import { AnimatePresence, motion } from "motion/react";
import * as React from "react";
import { cn } from "@/lib/utils";

// Types
export type SortDirection = "asc" | "desc" | null;

export interface ColumnDef<T> {
  id: string;
  header: React.ReactNode;
  accessorKey?: keyof T;
  cell?: (row: T, index: number) => React.ReactNode;
  sortable?: boolean;
  filterable?: boolean;
  align?: "left" | "center" | "right";
  width?: string;
  hideable?: boolean;
}

export interface PaginationConfig {
  page: number;
  pageSize: number;
  totalItems: number;
  pageSizeOptions?: number[];
  onPageChange: (page: number) => void;
  onPageSizeChange?: (pageSize: number) => void;
}

export interface AnimatedTableProps<T extends { id: string | number }> {
  data: T[];
  columns: ColumnDef<T>[];
  selectable?: boolean;
  selectedIds?: (string | number)[];
  onSelectionChange?: (ids: (string | number)[]) => void;
  onRowClick?: (row: T) => void;
  sortColumn?: string;
  sortDirection?: SortDirection;
  onSort?: (columnId: string, direction: SortDirection) => void;
  striped?: boolean;
  stickyHeader?: boolean;
  className?: string;
  emptyMessage?: React.ReactNode;
  loading?: boolean;
  loadingRows?: number;
  // New features
  searchable?: boolean;
  searchValue?: string;
  onSearchChange?: (value: string) => void;
  searchPlaceholder?: string;
  pagination?: PaginationConfig;
  expandable?: boolean;
  renderExpandedRow?: (row: T) => React.ReactNode;
  columnVisibility?: boolean;
  visibleColumns?: string[];
  onVisibleColumnsChange?: (columns: string[]) => void;
}

// Search Input Component
const TableSearch = ({
  value,
  onChange,
  placeholder = "Search...",
}: {
  value: string;
  onChange: (value: string) => void;
  placeholder?: string;
}) => (
  <div className="relative">
    <Search className="absolute top-1/2 left-3 h-4 w-4 -translate-y-1/2 text-muted-foreground" />
    <input
      type="text"
      value={value}
      onChange={(e) => onChange(e.target.value)}
      placeholder={placeholder}
      className="h-9 w-full rounded-md border border-table-border bg-background pr-8 pl-9 text-foreground text-sm placeholder:text-muted-foreground focus:border-primary focus:outline-none focus:ring-1 focus:ring-primary"
    />
    <AnimatePresence>
      {value && (
        <motion.button
          initial={{ opacity: 0, scale: 0.8 }}
          animate={{ opacity: 1, scale: 1 }}
          exit={{ opacity: 0, scale: 0.8 }}
          onClick={() => onChange("")}
          className="absolute top-1/2 right-2 -translate-y-1/2 rounded p-0.5 text-muted-foreground hover:bg-muted hover:text-foreground"
        >
          <X className="h-3.5 w-3.5" />
        </motion.button>
      )}
    </AnimatePresence>
  </div>
);

// Column Visibility Dropdown
const ColumnVisibilityDropdown = <T,>({
  columns,
  visibleColumns,
  onChange,
}: {
  columns: ColumnDef<T>[];
  visibleColumns: string[];
  onChange: (columns: string[]) => void;
}) => {
  const [open, setOpen] = React.useState(false);
  const dropdownRef = React.useRef<HTMLDivElement>(null);

  React.useEffect(() => {
    const handleClickOutside = (e: MouseEvent) => {
      if (
        dropdownRef.current &&
        !dropdownRef.current.contains(e.target as Node)
      ) {
        setOpen(false);
      }
    };
    document.addEventListener("mousedown", handleClickOutside);
    return () => document.removeEventListener("mousedown", handleClickOutside);
  }, []);

  const hideableColumns = columns.filter((col) => col.hideable !== false);

  const toggleColumn = (columnId: string) => {
    if (visibleColumns.includes(columnId)) {
      if (visibleColumns.length > 1) {
        onChange(visibleColumns.filter((id) => id !== columnId));
      }
    } else {
      onChange([...visibleColumns, columnId]);
    }
  };

  return (
    <div className="relative" ref={dropdownRef}>
      <button
        onClick={() => setOpen(!open)}
        className={cn(
          "flex h-9 items-center gap-2 rounded-md border border-table-border bg-background px-3 text-muted-foreground text-sm transition-colors hover:bg-muted hover:text-foreground",
          open && "border-primary text-foreground",
        )}
      >
        <Columns3 className="h-4 w-4" />
        <span>Columns</span>
      </button>
      <AnimatePresence>
        {open && (
          <motion.div
            initial={{ opacity: 0, y: -8, scale: 0.96 }}
            animate={{ opacity: 1, y: 0, scale: 1 }}
            exit={{ opacity: 0, y: -8, scale: 0.96 }}
            transition={{ duration: 0.15 }}
            className="absolute top-full right-0 z-20 mt-2 min-w-[180px] rounded-lg border border-table-border bg-card p-1 shadow-lg"
          >
            {hideableColumns.map((column) => (
              <button
                key={column.id}
                onClick={() => toggleColumn(column.id)}
                className="flex w-full items-center gap-2 rounded-md px-2 py-1.5 text-left text-sm transition-colors hover:bg-muted"
              >
                <div
                  className={cn(
                    "flex h-4 w-4 items-center justify-center rounded border transition-all",
                    visibleColumns.includes(column.id)
                      ? "border-primary bg-primary text-primary-foreground"
                      : "border-muted-foreground/40",
                  )}
                >
                  {visibleColumns.includes(column.id) && (
                    <Check className="h-3 w-3" strokeWidth={3} />
                  )}
                </div>
                <span className="text-foreground">{column.header}</span>
              </button>
            ))}
          </motion.div>
        )}
      </AnimatePresence>
    </div>
  );
};

// Pagination Component
const TablePagination = ({
  page,
  pageSize,
  totalItems,
  pageSizeOptions = [5, 10, 20, 50],
  onPageChange,
  onPageSizeChange,
}: PaginationConfig) => {
  const totalPages = Math.ceil(totalItems / pageSize);
  const startItem = (page - 1) * pageSize + 1;
  const endItem = Math.min(page * pageSize, totalItems);

  const canGoPrev = page > 1;
  const canGoNext = page < totalPages;

  return (
    <div className="flex flex-wrap items-center justify-between gap-4 border-table-border border-t bg-table-header px-4 py-3">
      <div className="flex items-center gap-2 text-muted-foreground text-sm">
        <span>Rows per page:</span>
        <select
          value={pageSize}
          onChange={(e) => onPageSizeChange?.(Number(e.target.value))}
          className="h-8 rounded border border-table-border bg-background px-2 text-foreground focus:border-primary focus:outline-none"
        >
          {pageSizeOptions.map((size) => (
            <option key={size} value={size}>
              {size}
            </option>
          ))}
        </select>
      </div>

      <div className="flex items-center gap-1 text-muted-foreground text-sm">
        <span>
          {totalItems > 0
            ? `${startItem}-${endItem} of ${totalItems}`
            : "0 items"}
        </span>
      </div>

      <div className="flex items-center gap-1">
        <motion.button
          whileHover={{ scale: canGoPrev ? 1.05 : 1 }}
          whileTap={{ scale: canGoPrev ? 0.95 : 1 }}
          disabled={!canGoPrev}
          onClick={() => onPageChange(1)}
          className={cn(
            "flex h-8 w-8 items-center justify-center rounded border border-table-border transition-colors",
            canGoPrev
              ? "text-foreground hover:bg-muted"
              : "cursor-not-allowed text-muted-foreground/40",
          )}
        >
          <ChevronsLeft className="h-4 w-4" />
        </motion.button>
        <motion.button
          whileHover={{ scale: canGoPrev ? 1.05 : 1 }}
          whileTap={{ scale: canGoPrev ? 0.95 : 1 }}
          disabled={!canGoPrev}
          onClick={() => onPageChange(page - 1)}
          className={cn(
            "flex h-8 w-8 items-center justify-center rounded border border-table-border transition-colors",
            canGoPrev
              ? "text-foreground hover:bg-muted"
              : "cursor-not-allowed text-muted-foreground/40",
          )}
        >
          <ChevronLeft className="h-4 w-4" />
        </motion.button>

        <div className="flex items-center gap-1 px-2">
          {Array.from({ length: Math.min(5, totalPages) }, (_, i) => {
            let pageNum: number;
            if (totalPages <= 5) {
              pageNum = i + 1;
            } else if (page <= 3) {
              pageNum = i + 1;
            } else if (page >= totalPages - 2) {
              pageNum = totalPages - 4 + i;
            } else {
              pageNum = page - 2 + i;
            }

            return (
              <motion.button
                key={pageNum}
                whileHover={{ scale: 1.1 }}
                whileTap={{ scale: 0.9 }}
                onClick={() => onPageChange(pageNum)}
                className={cn(
                  "flex h-8 w-8 items-center justify-center rounded text-sm transition-colors",
                  page === pageNum
                    ? "bg-primary text-primary-foreground"
                    : "text-muted-foreground hover:bg-muted hover:text-foreground",
                )}
              >
                {pageNum}
              </motion.button>
            );
          })}
        </div>

        <motion.button
          whileHover={{ scale: canGoNext ? 1.05 : 1 }}
          whileTap={{ scale: canGoNext ? 0.95 : 1 }}
          disabled={!canGoNext}
          onClick={() => onPageChange(page + 1)}
          className={cn(
            "flex h-8 w-8 items-center justify-center rounded border border-table-border transition-colors",
            canGoNext
              ? "text-foreground hover:bg-muted"
              : "cursor-not-allowed text-muted-foreground/40",
          )}
        >
          <ChevronRight className="h-4 w-4" />
        </motion.button>
        <motion.button
          whileHover={{ scale: canGoNext ? 1.05 : 1 }}
          whileTap={{ scale: canGoNext ? 0.95 : 1 }}
          disabled={!canGoNext}
          onClick={() => onPageChange(totalPages)}
          className={cn(
            "flex h-8 w-8 items-center justify-center rounded border border-table-border transition-colors",
            canGoNext
              ? "text-foreground hover:bg-muted"
              : "cursor-not-allowed text-muted-foreground/40",
          )}
        >
          <ChevronsRight className="h-4 w-4" />
        </motion.button>
      </div>
    </div>
  );
};

// Animated Table Root
const AnimatedTableRoot = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement> & { stickyHeader?: boolean }
>(({ className, stickyHeader, ...props }, ref) => (
  <div
    ref={ref}
    className={cn(
      "relative w-full overflow-hidden rounded-lg border border-table-border bg-card",
      className,
    )}
    {...props}
  />
));
AnimatedTableRoot.displayName = "AnimatedTableRoot";

// Table Scroll Container
const TableScrollContainer = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement> & { stickyHeader?: boolean }
>(({ className, stickyHeader, ...props }, ref) => (
  <div
    ref={ref}
    className={cn("overflow-auto", stickyHeader && "max-h-[400px]", className)}
    {...props}
  />
));
TableScrollContainer.displayName = "TableScrollContainer";

// Table Element
const TableElement = React.forwardRef<
  HTMLTableElement,
  React.HTMLAttributes<HTMLTableElement>
>(({ className, ...props }, ref) => (
  <table
    ref={ref}
    className={cn("w-full caption-bottom text-sm", className)}
    {...props}
  />
));
TableElement.displayName = "TableElement";

// Table Header
const TableHeader = React.forwardRef<
  HTMLTableSectionElement,
  React.HTMLAttributes<HTMLTableSectionElement> & { sticky?: boolean }
>(({ className, sticky, ...props }, ref) => (
  <thead
    ref={ref}
    className={cn(
      "bg-table-header",
      sticky && "sticky top-0 z-10 shadow-sm",
      className,
    )}
    {...props}
  />
));
TableHeader.displayName = "TableHeader";

// Table Body
const TableBody = React.forwardRef<
  HTMLTableSectionElement,
  React.HTMLAttributes<HTMLTableSectionElement>
>(({ className, ...props }, ref) => (
  <tbody
    ref={ref}
    className={cn("[&_tr:last-child]:border-0", className)}
    {...props}
  />
));
TableBody.displayName = "TableBody";

// Table Row
interface TableRowProps {
  isSelected?: boolean;
  striped?: boolean;
  index?: number;
  onClick?: () => void;
  className?: string;
  children?: React.ReactNode;
}

const TableRow = React.forwardRef<HTMLTableRowElement, TableRowProps>(
  ({ className, isSelected, striped, index = 0, onClick, children }, ref) => (
    <motion.tr
      ref={ref}
      initial={{ opacity: 0, y: 8 }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, y: -8 }}
      transition={{ duration: 0.2, delay: index * 0.03 }}
      onClick={onClick}
      className={cn(
        "group border-table-border border-b transition-colors",
        "hover:bg-table-row-hover",
        isSelected && "bg-table-row-selected",
        striped && index % 2 === 1 && "bg-table-row-stripe",
        className,
      )}
    >
      {children}
    </motion.tr>
  ),
);
TableRow.displayName = "TableRow";

// Expanded Row
const ExpandedRow = ({
  children,
  colSpan,
}: {
  children: React.ReactNode;
  colSpan: number;
}) => (
  <motion.tr
    initial={{ opacity: 0, height: 0 }}
    animate={{ opacity: 1, height: "auto" }}
    exit={{ opacity: 0, height: 0 }}
    transition={{ duration: 0.2 }}
    className="border-table-border border-b bg-muted/30"
  >
    <td colSpan={colSpan} className="p-0">
      <motion.div
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        exit={{ opacity: 0 }}
        transition={{ duration: 0.15, delay: 0.05 }}
        className="px-4 py-3"
      >
        {children}
      </motion.div>
    </td>
  </motion.tr>
);

// Expand Button
const ExpandButton = ({
  isExpanded,
  onClick,
}: {
  isExpanded: boolean;
  onClick: () => void;
}) => (
  <motion.button
    onClick={(e) => {
      e.stopPropagation();
      onClick();
    }}
    className="flex h-6 w-6 items-center justify-center rounded transition-colors hover:bg-muted"
    animate={{ rotate: isExpanded ? 90 : 0 }}
    transition={{ duration: 0.2 }}
  >
    <ChevronRight className="h-4 w-4 text-muted-foreground" />
  </motion.button>
);

// Table Head Cell
interface TableHeadProps extends React.ThHTMLAttributes<HTMLTableCellElement> {
  sortable?: boolean;
  sortDirection?: SortDirection;
  onSort?: () => void;
  align?: "left" | "center" | "right";
}

const TableHead = React.forwardRef<HTMLTableCellElement, TableHeadProps>(
  (
    {
      className,
      children,
      sortable,
      sortDirection,
      onSort,
      align = "left",
      ...props
    },
    ref,
  ) => {
    const alignClass = {
      left: "text-left",
      center: "text-center",
      right: "text-right",
    }[align];

    return (
      <th
        ref={ref}
        className={cn(
          "h-12 px-4 font-medium text-muted-foreground",
          alignClass,
          sortable && "cursor-pointer select-none hover:text-foreground",
          className,
        )}
        onClick={sortable ? onSort : undefined}
        {...props}
      >
        <div
          className={cn(
            "flex items-center gap-2",
            align === "center" && "justify-center",
            align === "right" && "justify-end",
          )}
        >
          <span>{children}</span>
          {sortable && (
            <motion.span
              className="flex-shrink-0"
              animate={sortDirection ? { scale: 1 } : { scale: 0.9 }}
            >
              {sortDirection === "asc" ? (
                <ChevronUp className="h-4 w-4 animate-sort-bounce text-table-sort-active" />
              ) : sortDirection === "desc" ? (
                <ChevronDown className="h-4 w-4 animate-sort-bounce text-table-sort-active" />
              ) : (
                <ChevronsUpDown className="h-4 w-4 opacity-40 group-hover:opacity-70" />
              )}
            </motion.span>
          )}
        </div>
      </th>
    );
  },
);
TableHead.displayName = "TableHead";

// Table Cell
interface TableCellProps extends React.TdHTMLAttributes<HTMLTableCellElement> {
  align?: "left" | "center" | "right";
}

const TableCell = React.forwardRef<HTMLTableCellElement, TableCellProps>(
  ({ className, align = "left", ...props }, ref) => {
    const alignClass = {
      left: "text-left",
      center: "text-center",
      right: "text-right",
    }[align];

    return (
      <td
        ref={ref}
        className={cn("p-4 align-middle", alignClass, className)}
        {...props}
      />
    );
  },
);
TableCell.displayName = "TableCell";

// Checkbox Cell
interface CheckboxCellProps {
  checked: boolean;
  indeterminate?: boolean;
  onChange: () => void;
}

const CheckboxCell = ({
  checked,
  indeterminate,
  onChange,
}: CheckboxCellProps) => (
  <div
    role="checkbox"
    aria-checked={indeterminate ? "mixed" : checked}
    tabIndex={0}
    onClick={(e) => {
      e.stopPropagation();
      onChange();
    }}
    onKeyDown={(e) => {
      if (e.key === " " || e.key === "Enter") {
        e.preventDefault();
        onChange();
      }
    }}
    className={cn(
      "flex h-4 w-4 cursor-pointer items-center justify-center rounded border transition-all duration-150",
      checked || indeterminate
        ? "border-primary bg-primary text-primary-foreground"
        : "border-muted-foreground/40 hover:border-muted-foreground",
    )}
  >
    <AnimatePresence mode="wait">
      {(checked || indeterminate) && (
        <motion.div
          initial={{ scale: 0, opacity: 0 }}
          animate={{ scale: 1, opacity: 1 }}
          exit={{ scale: 0, opacity: 0 }}
          transition={{ duration: 0.1 }}
        >
          {indeterminate ? (
            <div className="h-0.5 w-2 rounded-full bg-current" />
          ) : (
            <Check className="h-3 w-3" strokeWidth={3} />
          )}
        </motion.div>
      )}
    </AnimatePresence>
  </div>
);

// Skeleton Row for Loading State
const SkeletonRow = ({
  columns,
  index,
}: {
  columns: number;
  index: number;
}) => (
  <motion.tr
    initial={{ opacity: 0 }}
    animate={{ opacity: 1 }}
    transition={{ delay: index * 0.05 }}
    className="border-table-border border-b"
  >
    <td colSpan={columns} className="p-4">
      <div className="flex items-center gap-4">
        <div className="h-4 w-4 animate-pulse rounded bg-muted" />
        <div className="flex-1 space-y-2">
          <div
            className="h-4 animate-pulse rounded bg-muted"
            style={{ width: `${60 + Math.random() * 30}%` }}
          />
        </div>
        {Array.from({ length: columns - 2 }).map((_, i) => (
          <div
            key={i}
            className="h-4 animate-pulse rounded bg-muted"
            style={{ width: `${40 + Math.random() * 40}px` }}
          />
        ))}
      </div>
    </td>
  </motion.tr>
);

// Empty State
const EmptyState = ({
  message,
  colSpan,
}: {
  message: React.ReactNode;
  colSpan: number;
}) => (
  <motion.tr
    initial={{ opacity: 0, y: 20 }}
    animate={{ opacity: 1, y: 0 }}
    transition={{ duration: 0.3 }}
  >
    <td colSpan={colSpan} className="h-32 text-center">
      <div className="flex flex-col items-center justify-center gap-2 text-muted-foreground">
        {message || "No data available"}
      </div>
    </td>
  </motion.tr>
);

// Main AnimatedTable Component
export function AnimatedTable<T extends { id: string | number }>({
  data,
  columns,
  selectable = false,
  selectedIds = [],
  onSelectionChange,
  onRowClick,
  sortColumn,
  sortDirection,
  onSort,
  striped = false,
  stickyHeader = false,
  className,
  emptyMessage,
  loading = false,
  loadingRows = 5,
  // New features
  searchable = false,
  searchValue = "",
  onSearchChange,
  searchPlaceholder,
  pagination,
  expandable = false,
  renderExpandedRow,
  columnVisibility = false,
  visibleColumns: controlledVisibleColumns,
  onVisibleColumnsChange,
}: AnimatedTableProps<T>) {
  const [expandedRows, setExpandedRows] = React.useState<Set<string | number>>(
    new Set(),
  );
  const [internalVisibleColumns, setInternalVisibleColumns] = React.useState<
    string[]
  >(columns.map((col) => col.id));

  const visibleColumns = controlledVisibleColumns || internalVisibleColumns;
  const setVisibleColumns = onVisibleColumnsChange || setInternalVisibleColumns;

  const displayedColumns = columns.filter((col) =>
    visibleColumns.includes(col.id),
  );

  const allSelected = data.length > 0 && selectedIds.length === data.length;
  const someSelected =
    selectedIds.length > 0 && selectedIds.length < data.length;

  const handleSelectAll = () => {
    if (onSelectionChange) {
      if (allSelected) {
        onSelectionChange([]);
      } else {
        onSelectionChange(data.map((row) => row.id));
      }
    }
  };

  const handleSelectRow = (id: string | number) => {
    if (onSelectionChange) {
      if (selectedIds.includes(id)) {
        onSelectionChange(
          selectedIds.filter((selectedId) => selectedId !== id),
        );
      } else {
        onSelectionChange([...selectedIds, id]);
      }
    }
  };

  const handleSort = (columnId: string) => {
    if (onSort) {
      let newDirection: SortDirection;
      if (sortColumn !== columnId) {
        newDirection = "asc";
      } else if (sortDirection === "asc") {
        newDirection = "desc";
      } else if (sortDirection === "desc") {
        newDirection = null;
      } else {
        newDirection = "asc";
      }
      onSort(columnId, newDirection);
    }
  };

  const toggleRowExpanded = (id: string | number) => {
    setExpandedRows((prev) => {
      const next = new Set(prev);
      if (next.has(id)) {
        next.delete(id);
      } else {
        next.add(id);
      }
      return next;
    });
  };

  const extraColumns = (selectable ? 1 : 0) + (expandable ? 1 : 0);
  const totalColumns = displayedColumns.length + extraColumns;

  const showToolbar = searchable || columnVisibility;

  return (
    <AnimatedTableRoot className={className}>
      {/* Toolbar */}
      {showToolbar && (
        <div className="flex flex-wrap items-center justify-between gap-3 border-table-border border-b bg-table-header p-3">
          {searchable && (
            <div className="w-full sm:w-64">
              <TableSearch
                value={searchValue}
                onChange={onSearchChange || (() => {})}
                placeholder={searchPlaceholder}
              />
            </div>
          )}
          <div className="flex items-center gap-2">
            {columnVisibility && (
              <ColumnVisibilityDropdown
                columns={columns}
                visibleColumns={visibleColumns}
                onChange={setVisibleColumns}
              />
            )}
          </div>
        </div>
      )}

      {/* Table */}
      <TableScrollContainer stickyHeader={stickyHeader}>
        <TableElement>
          <TableHeader sticky={stickyHeader}>
            <tr className="border-table-border border-b">
              {expandable && <TableHead className="w-10" />}
              {selectable && (
                <TableHead className="w-12">
                  <CheckboxCell
                    checked={allSelected}
                    indeterminate={someSelected}
                    onChange={handleSelectAll}
                  />
                </TableHead>
              )}
              {displayedColumns.map((column) => (
                <TableHead
                  key={column.id}
                  sortable={column.sortable && !!onSort}
                  sortDirection={
                    sortColumn === column.id ? sortDirection : null
                  }
                  onSort={() => handleSort(column.id)}
                  align={column.align}
                  style={{ width: column.width }}
                >
                  {column.header}
                </TableHead>
              ))}
            </tr>
          </TableHeader>
          <TableBody>
            <AnimatePresence mode="popLayout">
              {loading ? (
                Array.from({ length: loadingRows }).map((_, index) => (
                  <SkeletonRow
                    key={`skeleton-${index}`}
                    columns={totalColumns}
                    index={index}
                  />
                ))
              ) : data.length === 0 ? (
                <EmptyState message={emptyMessage} colSpan={totalColumns} />
              ) : (
                data.map((row, index) => (
                  <React.Fragment key={row.id}>
                    <TableRow
                      isSelected={selectedIds.includes(row.id)}
                      striped={striped}
                      index={index}
                      onClick={() => onRowClick?.(row)}
                      className={onRowClick ? "cursor-pointer" : undefined}
                    >
                      {expandable && (
                        <TableCell className="w-10">
                          <ExpandButton
                            isExpanded={expandedRows.has(row.id)}
                            onClick={() => toggleRowExpanded(row.id)}
                          />
                        </TableCell>
                      )}
                      {selectable && (
                        <TableCell className="w-12">
                          <CheckboxCell
                            checked={selectedIds.includes(row.id)}
                            onChange={() => handleSelectRow(row.id)}
                          />
                        </TableCell>
                      )}
                      {displayedColumns.map((column) => (
                        <TableCell key={column.id} align={column.align}>
                          {column.cell
                            ? column.cell(row, index)
                            : column.accessorKey
                              ? String(row[column.accessorKey] ?? "")
                              : null}
                        </TableCell>
                      ))}
                    </TableRow>
                    <AnimatePresence>
                      {expandable &&
                        expandedRows.has(row.id) &&
                        renderExpandedRow && (
                          <ExpandedRow colSpan={totalColumns}>
                            {renderExpandedRow(row)}
                          </ExpandedRow>
                        )}
                    </AnimatePresence>
                  </React.Fragment>
                ))
              )}
            </AnimatePresence>
          </TableBody>
        </TableElement>
      </TableScrollContainer>

      {/* Pagination */}
      {pagination && <TablePagination {...pagination} />}
    </AnimatedTableRoot>
  );
}

export {
  AnimatedTableRoot,
  CheckboxCell,
  ColumnVisibilityDropdown,
  EmptyState,
  ExpandButton,
  ExpandedRow,
  SkeletonRow,
  TableBody,
  TableCell,
  TableElement,
  TableHead,
  TableHeader,
  TablePagination,
  TableRow,
  TableScrollContainer,
  TableSearch,
};
```
---

## Animated Theme Toggle <a name="animated-theme-toggle"></a>

Smooth dark mode toggle for React. Sun-to-moon SVG morph animation with next-themes integration. Multiple sizes and customizable colors.

- **Registry Key**: `animated-theme-toggle`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/animated-theme-toggle.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `next-themes`

#### How to Use
```tsx
import { AnimatedThemeToggle } from "@/registry/default/ui/animated-theme-toggle";

export default function AnimatedThemeToggleDemo() {
  return (
    <div className="flex items-center justify-center p-8">
      <AnimatedThemeToggle />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/animated-theme-toggle.tsx`
```tsx
"use client";

import { motion, useMotionValue, useTransform } from "motion/react";
import { useTheme } from "next-themes";
import { useEffect, useState } from "react";
import { cn } from "@/lib/utils";
import { Button } from "./button";

export const AnimatedThemeToggle = ({ className }: { className?: string }) => {
  const { setTheme, resolvedTheme } = useTheme();
  const [mounted, setMounted] = useState(false);

  // Avoid hydration mismatch
  useEffect(() => {
    setMounted(true);
  }, []);

  const isDark = mounted ? resolvedTheme === "dark" : false;

  const toggleTheme = () => {
    setTheme(isDark ? "light" : "dark");
  };

  // Show a placeholder during SSR to avoid hydration mismatch
  if (!mounted) {
    return (
      <Button className={cn("px-2.5", className)} variant="outline" disabled>
        <div className="h-5 w-5" />
      </Button>
    );
  }

  return (
    <Button
      onClick={toggleTheme}
      className={cn("px-2.5", className)}
      variant="outline"
      aria-label={isDark ? "Switch to light mode" : "Switch to dark mode"}
    >
      <SolarSwitch isDark={isDark} />
    </Button>
  );
};

const SolarSwitch = ({ isDark }: { isDark: boolean }) => {
  const duration = 0.7;

  const moonVariants = {
    checked: {
      scale: 1,
    },
    unchecked: {
      scale: 0,
    },
  };

  const sunVariants = {
    checked: {
      scale: 0,
    },
    unchecked: {
      scale: 1,
    },
  };
  const scaleMoon = useMotionValue(isDark ? 1 : 0);
  const scaleSun = useMotionValue(isDark ? 0 : 1);
  const pathLengthMoon = useTransform(scaleMoon, [0.6, 1], [0, 1]);
  const pathLengthSun = useTransform(scaleSun, [0.6, 1], [0, 1]);

  return (
    <motion.div animate={isDark ? "checked" : "unchecked"}>
      <motion.svg
        width="20"
        height="20"
        viewBox="0 0 25 25"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <motion.path
          d="M12.4058 17.7625C15.1672 17.7625 17.4058 15.5239 17.4058 12.7625C17.4058 10.0011 15.1672 7.76251 12.4058 7.76251C9.64434 7.76251 7.40576 10.0011 7.40576 12.7625C7.40576 15.5239 9.64434 17.7625 12.4058 17.7625Z"
          stroke="currentColor"
          strokeWidth="2"
          strokeLinecap="round"
          strokeLinejoin="round"
          variants={sunVariants}
          custom={isDark}
          transition={{ duration }}
          style={{
            pathLength: pathLengthSun,
            scale: scaleSun,
          }}
        />
        <motion.path
          d="M12.4058 1.76251V3.76251"
          stroke="currentColor"
          strokeWidth="2"
          strokeLinecap="round"
          strokeLinejoin="round"
          variants={sunVariants}
          custom={isDark}
          transition={{ duration }}
          style={{
            pathLength: pathLengthSun,
            scale: scaleSun,
          }}
        />
        <motion.path
          d="M12.4058 21.7625V23.7625"
          stroke="currentColor"
          strokeWidth="2"
          strokeLinecap="round"
          strokeLinejoin="round"
          variants={sunVariants}
          custom={isDark}
          transition={{ duration }}
          style={{
            pathLength: pathLengthSun,
            scale: scaleSun,
          }}
        />
        <motion.path
          d="M4.62598 4.98248L6.04598 6.40248"
          stroke="currentColor"
          strokeWidth="2"
          strokeLinecap="round"
          strokeLinejoin="round"
          variants={sunVariants}
          custom={isDark}
          transition={{ duration }}
          style={{
            pathLength: pathLengthSun,
            scale: scaleSun,
          }}
        />
        <motion.path
          d="M18.7656 19.1225L20.1856 20.5425"
          stroke="currentColor"
          strokeWidth="2"
          strokeLinecap="round"
          strokeLinejoin="round"
          variants={sunVariants}
          custom={isDark}
          transition={{ duration }}
          style={{
            pathLength: pathLengthSun,
            scale: scaleSun,
          }}
        />
        <motion.path
          d="M1.40576 12.7625H3.40576"
          stroke="currentColor"
          strokeWidth="2"
          strokeLinecap="round"
          strokeLinejoin="round"
          variants={sunVariants}
          custom={isDark}
          transition={{ duration }}
          style={{
            pathLength: pathLengthSun,
            scale: scaleSun,
          }}
        />
        <motion.path
          d="M21.4058 12.7625H23.4058"
          stroke="currentColor"
          strokeWidth="2"
          strokeLinecap="round"
          strokeLinejoin="round"
          variants={sunVariants}
          custom={isDark}
          transition={{ duration }}
          style={{
            pathLength: pathLengthSun,
            scale: scaleSun,
          }}
        />
        <motion.path
          d="M4.62598 20.5425L6.04598 19.1225"
          stroke="currentColor"
          strokeWidth="2"
          strokeLinecap="round"
          strokeLinejoin="round"
          variants={sunVariants}
          custom={isDark}
          transition={{ duration }}
          style={{
            pathLength: pathLengthSun,
            scale: scaleSun,
          }}
        />
        <motion.path
          d="M18.7656 6.40248L20.1856 4.98248"
          stroke="currentColor"
          strokeWidth="2"
          strokeLinecap="round"
          strokeLinejoin="round"
          variants={sunVariants}
          custom={isDark}
          transition={{ duration }}
          style={{
            pathLength: pathLengthSun,
            scale: scaleSun,
          }}
        />
        <motion.path
          d="M21.1918 13.2013C21.0345 14.9035 20.3957 16.5257 19.35 17.8781C18.3044 19.2305 16.8953 20.2571 15.2875 20.8379C13.6797 21.4186 11.9398 21.5294 10.2713 21.1574C8.60281 20.7854 7.07479 19.9459 5.86602 18.7371C4.65725 17.5283 3.81774 16.0003 3.4457 14.3318C3.07367 12.6633 3.18451 10.9234 3.76526 9.31561C4.346 7.70783 5.37263 6.29868 6.72501 5.25307C8.07739 4.20746 9.69959 3.56862 11.4018 3.41132C10.4052 4.75958 9.92564 6.42077 10.0503 8.09273C10.175 9.76469 10.8957 11.3364 12.0812 12.5219C13.2667 13.7075 14.8384 14.4281 16.5104 14.5528C18.1823 14.6775 19.8435 14.1979 21.1918 13.2013Z"
          stroke="currentColor"
          strokeWidth="2"
          strokeLinecap="round"
          strokeLinejoin="round"
          transition={{ duration }}
          variants={moonVariants}
          custom={isDark}
          style={{
            pathLength: pathLengthMoon,
            scale: scaleMoon,
          }}
        />
      </motion.svg>
    </motion.div>
  );
};
```
---

## Animated Toast <a name="animated-toast"></a>

Animated toast notifications for React. Success, error, warning, and info variants with stacking, promises, and custom styling. Sonner alternative.

- **Registry Key**: `animated-toast`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/animated-toast.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### How to Use
```tsx
"use client";

import {
  AnimatedToastProvider,
  useAnimatedToast,
} from "@/registry/default/ui/animated-toast";

function ToastDemoContent() {
  const { addToast } = useAnimatedToast();

  const showToast = (
    type: "success" | "error" | "warning" | "info" | "default",
  ) => {
    addToast({
      title: `${type.charAt(0).toUpperCase() + type.slice(1)} Toast`,
      message: `This is a ${type} notification message.`,
      type,
      duration: 4000,
    });
  };

  const showToastWithAction = () => {
    addToast({
      title: "Action Required",
      message: "This toast has an action button.",
      type: "info",
      action: {
        label: "Click me",
        onClick: () => alert("Action clicked!"),
      },
    });
  };

  return (
    <div className="flex flex-wrap gap-4">
      <button
        onClick={() => showToast("success")}
        className="rounded-md bg-green-500 px-4 py-2 text-white transition-colors hover:bg-green-600"
      >
        Success Toast
      </button>
      <button
        onClick={() => showToast("error")}
        className="rounded-md bg-red-500 px-4 py-2 text-white transition-colors hover:bg-red-600"
      >
        Error Toast
      </button>
      <button
        onClick={() => showToast("warning")}
        className="rounded-md bg-yellow-500 px-4 py-2 text-white transition-colors hover:bg-yellow-600"
      >
        Warning Toast
      </button>
      <button
        onClick={() => showToast("info")}
        className="rounded-md bg-blue-500 px-4 py-2 text-white transition-colors hover:bg-blue-600"
      >
        Info Toast
      </button>
      <button
        onClick={() => showToast("default")}
        className="rounded-md bg-gray-500 px-4 py-2 text-white transition-colors hover:bg-gray-600"
      >
        Default Toast
      </button>
      <button
        onClick={showToastWithAction}
        className="rounded-md bg-purple-500 px-4 py-2 text-white transition-colors hover:bg-purple-600"
      >
        Toast with Action
      </button>
    </div>
  );
}

export default function AnimatedToastDemo() {
  return (
    <AnimatedToastProvider position="top-right" maxToasts={3}>
      <div className="p-8">
        <ToastDemoContent />
      </div>
    </AnimatedToastProvider>
  );
}
```
#### Component Source Code
##### File: `ui/animated-toast.tsx`
```tsx
import {
  AlertCircle,
  AlertTriangle,
  Bell,
  CheckCircle,
  Info,
  X,
} from "lucide-react";
import { AnimatePresence, motion } from "motion/react";
import * as React from "react";
import { cn } from "@/lib/utils";

// Toast Types
type ToastType = "success" | "error" | "warning" | "info" | "default";
type ToastPosition =
  | "top-right"
  | "top-left"
  | "top-center"
  | "bottom-right"
  | "bottom-left"
  | "bottom-center";

interface Toast {
  id: string;
  title?: string;
  message: string;
  type?: ToastType;
  duration?: number;
  action?: {
    label: string;
    onClick: () => void;
  };
}

interface ToastContextType {
  toasts: Toast[];
  addToast: (toast: Omit<Toast, "id">) => string;
  removeToast: (id: string) => void;
  clearAll: () => void;
}

const ToastContext = React.createContext<ToastContextType | null>(null);

// Toast Provider
interface AnimatedToastProviderProps {
  children: React.ReactNode;
  position?: ToastPosition;
  maxToasts?: number;
}

export function AnimatedToastProvider({
  children,
  position = "top-right",
  maxToasts = 5,
}: AnimatedToastProviderProps) {
  const [toasts, setToasts] = React.useState<Toast[]>([]);

  const addToast = React.useCallback(
    (toast: Omit<Toast, "id">) => {
      const id = Math.random().toString(36).substr(2, 9);
      setToasts((prev) => {
        const newToasts = [...prev, { ...toast, id }];
        return newToasts.slice(-maxToasts);
      });
      return id;
    },
    [maxToasts],
  );

  const removeToast = React.useCallback((id: string) => {
    setToasts((prev) => prev.filter((t) => t.id !== id));
  }, []);

  const clearAll = React.useCallback(() => {
    setToasts([]);
  }, []);

  const positionClasses: Record<ToastPosition, string> = {
    "top-right": "top-4 right-4",
    "top-left": "top-4 left-4",
    "top-center": "top-4 left-1/2 -translate-x-1/2",
    "bottom-right": "bottom-4 right-4",
    "bottom-left": "bottom-4 left-4",
    "bottom-center": "bottom-4 left-1/2 -translate-x-1/2",
  };

  const isTop = position.startsWith("top");

  return (
    <ToastContext.Provider value={{ toasts, addToast, removeToast, clearAll }}>
      {children}
      <div
        className={cn(
          "pointer-events-none fixed z-50 flex flex-col gap-2",
          positionClasses[position],
        )}
      >
        <AnimatePresence mode="popLayout">
          {(isTop ? toasts : [...toasts].reverse()).map((toast, index) => (
            <ToastItem
              key={toast.id}
              toast={toast}
              index={index}
              onRemove={() => removeToast(toast.id)}
              isTop={isTop}
            />
          ))}
        </AnimatePresence>
      </div>
    </ToastContext.Provider>
  );
}

// Toast Item
interface ToastItemProps {
  toast: Toast;
  index: number;
  onRemove: () => void;
  isTop: boolean;
}

function ToastItem({ toast, index, onRemove, isTop }: ToastItemProps) {
  const { type = "default", title, message, duration = 5000, action } = toast;

  React.useEffect(() => {
    if (duration > 0) {
      const timer = setTimeout(onRemove, duration);
      return () => clearTimeout(timer);
    }
  }, [duration, onRemove]);

  const icons: Record<ToastType, React.ReactNode> = {
    success: <CheckCircle className="h-5 w-5 text-emerald-500" />,
    error: <AlertCircle className="h-5 w-5 text-red-500" />,
    warning: <AlertTriangle className="h-5 w-5 text-amber-500" />,
    info: <Info className="h-5 w-5 text-blue-500" />,
    default: <Bell className="h-5 w-5 text-muted-foreground" />,
  };

  const borderColors: Record<ToastType, string> = {
    success: "border-l-emerald-500",
    error: "border-l-red-500",
    warning: "border-l-amber-500",
    info: "border-l-blue-500",
    default: "border-l-border",
  };

  return (
    <motion.div
      layout
      initial={{ opacity: 0, y: isTop ? -20 : 20, scale: 0.9 }}
      animate={{
        opacity: 1,
        y: 0,
        scale: 1,
        transition: {
          type: "spring",
          stiffness: 500,
          damping: 30,
          delay: index * 0.05,
        },
      }}
      exit={{
        opacity: 0,
        scale: 0.9,
        x: 100,
        transition: { duration: 0.2 },
      }}
      className={cn(
        "pointer-events-auto min-w-[320px] max-w-[420px] rounded-lg border border-l-4 bg-card p-4 shadow-lg",
        borderColors[type],
      )}
    >
      <div className="flex items-start gap-3">
        <div className="mt-0.5 flex-shrink-0">{icons[type]}</div>
        <div className="min-w-0 flex-1">
          {title && <p className="font-medium text-card-foreground">{title}</p>}
          <p className={cn("text-muted-foreground text-sm", title && "mt-1")}>
            {message}
          </p>
          {action && (
            <button
              onClick={action.onClick}
              className="mt-2 font-medium text-primary text-sm hover:underline"
            >
              {action.label}
            </button>
          )}
        </div>
        <button
          onClick={onRemove}
          className="flex-shrink-0 rounded-md p-1 text-muted-foreground transition-colors hover:bg-muted hover:text-foreground"
        >
          <X className="h-4 w-4" />
        </button>
      </div>

      {/* Progress bar */}
      {duration > 0 && (
        <motion.div
          initial={{ scaleX: 1 }}
          animate={{ scaleX: 0 }}
          transition={{ duration: duration / 1000, ease: "linear" }}
          className={cn(
            "absolute right-0 bottom-0 left-0 h-1 origin-left rounded-b-lg",
            type === "success" && "bg-emerald-500/30",
            type === "error" && "bg-red-500/30",
            type === "warning" && "bg-amber-500/30",
            type === "info" && "bg-blue-500/30",
            type === "default" && "bg-muted",
          )}
        />
      )}
    </motion.div>
  );
}

// Hook to use toast
export function useAnimatedToast() {
  const context = React.useContext(ToastContext);
  if (!context) {
    throw new Error(
      "useAnimatedToast must be used within AnimatedToastProvider",
    );
  }
  return context;
}

// Standalone Toast Components

// Minimal Toast
interface MinimalToastProps {
  open: boolean;
  onClose: () => void;
  message: string;
  type?: ToastType;
}

export function MinimalToast({
  open,
  onClose,
  message,
  type = "default",
}: MinimalToastProps) {
  React.useEffect(() => {
    if (open) {
      const timer = setTimeout(onClose, 3000);
      return () => clearTimeout(timer);
    }
  }, [open, onClose]);

  const bgColors: Record<ToastType, string> = {
    success: "bg-emerald-500",
    error: "bg-red-500",
    warning: "bg-amber-500",
    info: "bg-blue-500",
    default: "bg-foreground",
  };

  return (
    <AnimatePresence>
      {open && (
        <motion.div
          initial={{ opacity: 0, y: 50 }}
          animate={{ opacity: 1, y: 0 }}
          exit={{ opacity: 0, y: 50 }}
          className={cn(
            "fixed bottom-8 left-1/2 z-50 -translate-x-1/2 rounded-full px-6 py-3 font-medium text-background text-black text-sm shadow-lg dark:text-white",
            bgColors[type],
          )}
        >
          {message}
        </motion.div>
      )}
    </AnimatePresence>
  );
}

// Undo Toast
interface UndoToastProps {
  open: boolean;
  onClose: () => void;
  onUndo: () => void;
  message: string;
  duration?: number;
}

export function UndoToast({
  open,
  onClose,
  onUndo,
  message,
  duration = 5000,
}: UndoToastProps) {
  const [progress, setProgress] = React.useState(100);

  React.useEffect(() => {
    if (open) {
      const interval = setInterval(() => {
        setProgress((prev) => {
          if (prev <= 0) {
            onClose();
            return 0;
          }
          return prev - 100 / (duration / 100);
        });
      }, 100);
      return () => clearInterval(interval);
    } else {
      setProgress(100);
    }
  }, [open, duration, onClose]);

  return (
    <AnimatePresence>
      {open && (
        <motion.div
          initial={{ opacity: 0, y: 50, scale: 0.9 }}
          animate={{ opacity: 1, y: 0, scale: 1 }}
          exit={{ opacity: 0, y: 50, scale: 0.9 }}
          className="fixed bottom-8 left-1/2 z-50 -translate-x-1/2 overflow-hidden rounded-lg bg-foreground text-background shadow-xl"
        >
          <div className="flex items-center gap-4 px-4 py-3">
            <span className="text-sm">{message}</span>
            <button
              onClick={() => {
                onUndo();
                onClose();
              }}
              className="rounded-md bg-primary px-3 py-1 font-semibold text-primary-foreground text-sm transition-opacity hover:opacity-90"
            >
              Undo
            </button>
          </div>
          <div
            className="h-1 bg-primary transition-all duration-100"
            style={{ width: `${progress}%` }}
          />
        </motion.div>
      )}
    </AnimatePresence>
  );
}

// Notification Toast (with avatar/image)
interface NotificationToastProps {
  open: boolean;
  onClose: () => void;
  title: string;
  message: string;
  avatar?: string;
  time?: string;
}

export function NotificationToast({
  open,
  onClose,
  title,
  message,
  avatar,
  time,
}: NotificationToastProps) {
  React.useEffect(() => {
    if (open) {
      const timer = setTimeout(onClose, 5000);
      return () => clearTimeout(timer);
    }
  }, [open, onClose]);

  return (
    <AnimatePresence>
      {open && (
        <motion.div
          initial={{ opacity: 0, x: 100, scale: 0.9 }}
          animate={{ opacity: 1, x: 0, scale: 1 }}
          exit={{ opacity: 0, x: 100, scale: 0.9 }}
          transition={{ type: "spring", stiffness: 400, damping: 25 }}
          className="fixed top-4 right-4 z-50 w-80 overflow-hidden rounded-xl border border-border bg-card shadow-2xl"
        >
          <div className="p-4">
            <div className="flex items-start gap-3">
              {avatar ? (
                // biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx
                <img
                  src={avatar}
                  alt=""
                  width={40}
                  height={40}
                  className="rounded-full object-cover"
                />
              ) : (
                <div className="flex h-10 w-10 items-center justify-center rounded-full bg-primary/10">
                  <Bell className="h-5 w-5 text-primary" />
                </div>
              )}
              <div className="min-w-0 flex-1">
                <div className="flex items-center justify-between">
                  <p className="truncate font-semibold text-card-foreground">
                    {title}
                  </p>
                  {time && (
                    <span className="text-muted-foreground text-xs">
                      {time}
                    </span>
                  )}
                </div>
                <p className="mt-0.5 line-clamp-2 text-muted-foreground text-sm">
                  {message}
                </p>
              </div>
            </div>
          </div>
          <button
            onClick={onClose}
            className="absolute top-2 right-2 rounded-full p-1 transition-colors hover:bg-muted"
          >
            <X className="h-4 w-4 text-muted-foreground" />
          </button>
        </motion.div>
      )}
    </AnimatePresence>
  );
}

// Stacked Notifications
export interface StackedToast {
  id: string;
  title: string;
  message: string;
  type?: ToastType;
}

interface StackedNotificationsProps {
  toasts: StackedToast[];
  onRemove: (id: string) => void;
  maxVisible?: number;
}

export function StackedNotifications({
  toasts,
  onRemove,
  maxVisible = 3,
}: StackedNotificationsProps) {
  const visibleToasts = toasts.slice(0, maxVisible);
  const hiddenCount = Math.max(0, toasts.length - maxVisible);

  const icons: Record<ToastType, React.ReactNode> = {
    success: <CheckCircle className="h-5 w-5 text-emerald-500" />,
    error: <AlertCircle className="h-5 w-5 text-red-500" />,
    warning: <AlertTriangle className="h-5 w-5 text-amber-500" />,
    info: <Info className="h-5 w-5 text-blue-500" />,
    default: <Bell className="h-5 w-5 text-muted-foreground" />,
  };

  return (
    <div className="fixed top-4 right-4 z-50 w-80">
      <AnimatePresence mode="popLayout">
        {visibleToasts.map((toast, index) => (
          <motion.div
            key={toast.id}
            layout
            initial={{ opacity: 0, y: -20, scale: 0.9 }}
            animate={{
              opacity: 1 - index * 0.15,
              y: index * 8,
              scale: 1 - index * 0.05,
              zIndex: maxVisible - index,
            }}
            exit={{ opacity: 0, x: 100 }}
            transition={{ type: "spring", stiffness: 400, damping: 25 }}
            style={{
              position: index === 0 ? "relative" : "absolute",
              top: 0,
              left: 0,
              right: 0,
            }}
            className="rounded-lg border border-border bg-card p-4 shadow-lg"
          >
            <div className="flex items-start gap-3">
              {icons[toast.type || "default"]}
              <div className="min-w-0 flex-1">
                <p className="font-medium text-card-foreground">
                  {toast.title}
                </p>
                <p className="mt-0.5 text-muted-foreground text-sm">
                  {toast.message}
                </p>
              </div>
              <button
                onClick={() => onRemove(toast.id)}
                className="rounded-md p-1 transition-colors hover:bg-muted"
              >
                <X className="h-4 w-4 text-muted-foreground" />
              </button>
            </div>
          </motion.div>
        ))}
      </AnimatePresence>

      {hiddenCount > 0 && (
        <motion.div
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          className="mt-2 text-center text-muted-foreground text-sm"
        >
          +{hiddenCount} more notifications
        </motion.div>
      )}
    </div>
  );
}

// Promise Toast (for async operations)
interface PromiseToastProps<T> {
  promise: Promise<T>;
  loading: string;
  success: string | ((data: T) => string);
  error: string | ((err: Error) => string);
}

export function usePromiseToast() {
  const { addToast, removeToast } = useAnimatedToast();

  return async function promiseToast<T>({
    promise,
    loading,
    success,
    error,
  }: PromiseToastProps<T>) {
    const id = addToast({ message: loading, type: "info", duration: 0 });

    try {
      const data = await promise;
      removeToast(id);
      addToast({
        message: typeof success === "function" ? success(data) : success,
        type: "success",
      });
      return data;
    } catch (err) {
      removeToast(id);
      addToast({
        message: typeof error === "function" ? error(err as Error) : error,
        type: "error",
      });
      throw err;
    }
  };
}
```
---

## Animated Tooltip <a name="animated-tooltip"></a>

Animated tooltip component for React. Spring animations, 12 placement options, and custom content support. Built on Radix Tooltip primitive.

- **Registry Key**: `animated-tooltip`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/animated-tooltip.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
"use client";

import { AnimatedTooltip } from "@/components/ui/animated-tooltip";

export default function AnimatedTooltipDemo() {
  return (
    <div className="flex flex-wrap items-center justify-center gap-4">
      {(["top", "bottom", "left", "right"] as const).map((placement) => (
        <AnimatedTooltip
          key={placement}
          content={`Tooltip on ${placement}`}
          placement={placement}
          animation="slide"
        >
          <button className="rounded-lg border bg-card px-4 py-2 font-medium text-sm capitalize transition-colors hover:bg-accent">
            {placement}
          </button>
        </AnimatedTooltip>
      ))}
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/animated-tooltip.tsx`
```tsx
"use client";
import { AnimatePresence, motion } from "motion/react";
import type React from "react";
import { useCallback, useEffect, useRef, useState } from "react";
import { cn } from "@/lib/utils";

type Placement =
  | "top"
  | "bottom"
  | "left"
  | "right"
  | "top-start"
  | "top-end"
  | "bottom-start"
  | "bottom-end";
type Animation = "fade" | "scale" | "slide" | "spring";

// Animated Tooltip
interface AnimatedTooltipProps {
  children: React.ReactNode;
  content: React.ReactNode;
  placement?: Placement;
  animation?: Animation;
  delay?: number;
  duration?: number;
  className?: string;
  contentClassName?: string;
  arrow?: boolean;
  offset?: number;
  disabled?: boolean;
}

export function AnimatedTooltip({
  children,
  content,
  placement = "top",
  animation = "fade",
  delay = 0,
  duration = 0.15,
  className,
  contentClassName,
  arrow = true,
  offset = 8,
  disabled = false,
}: AnimatedTooltipProps) {
  const [isVisible, setIsVisible] = useState(false);
  const [position, setPosition] = useState({ x: 0, y: 0 });
  const triggerRef = useRef<HTMLButtonElement>(null);
  const tooltipRef = useRef<HTMLDivElement>(null);
  const timeoutRef = useRef<NodeJS.Timeout | null>(null);

  const calculatePosition = useCallback(() => {
    if (!triggerRef.current || !tooltipRef.current) return;

    const triggerRect = triggerRef.current.getBoundingClientRect();
    const tooltipRect = tooltipRef.current.getBoundingClientRect();

    let x = 0;
    let y = 0;

    switch (placement) {
      case "top":
        x = triggerRect.left + triggerRect.width / 2 - tooltipRect.width / 2;
        y = triggerRect.top - tooltipRect.height - offset;
        break;
      case "top-start":
        x = triggerRect.left;
        y = triggerRect.top - tooltipRect.height - offset;
        break;
      case "top-end":
        x = triggerRect.right - tooltipRect.width;
        y = triggerRect.top - tooltipRect.height - offset;
        break;
      case "bottom":
        x = triggerRect.left + triggerRect.width / 2 - tooltipRect.width / 2;
        y = triggerRect.bottom + offset;
        break;
      case "bottom-start":
        x = triggerRect.left;
        y = triggerRect.bottom + offset;
        break;
      case "bottom-end":
        x = triggerRect.right - tooltipRect.width;
        y = triggerRect.bottom + offset;
        break;
      case "left":
        x = triggerRect.left - tooltipRect.width - offset;
        y = triggerRect.top + triggerRect.height / 2 - tooltipRect.height / 2;
        break;
      case "right":
        x = triggerRect.right + offset;
        y = triggerRect.top + triggerRect.height / 2 - tooltipRect.height / 2;
        break;
    }

    // Keep tooltip within viewport
    x = Math.max(8, Math.min(x, window.innerWidth - tooltipRect.width - 8));
    y = Math.max(8, Math.min(y, window.innerHeight - tooltipRect.height - 8));

    setPosition({ x, y });
  }, [placement, offset]);

  useEffect(() => {
    if (isVisible) {
      calculatePosition();
      window.addEventListener("scroll", calculatePosition);
      window.addEventListener("resize", calculatePosition);
    }
    return () => {
      window.removeEventListener("scroll", calculatePosition);
      window.removeEventListener("resize", calculatePosition);
    };
  }, [isVisible, calculatePosition]);

  const handleMouseEnter = () => {
    if (disabled) return;
    timeoutRef.current = setTimeout(() => setIsVisible(true), delay);
  };

  const handleMouseLeave = () => {
    if (timeoutRef.current) clearTimeout(timeoutRef.current);
    setIsVisible(false);
  };

  const getAnimationVariants = () => {
    const baseDirection = placement.split("-")[0];

    switch (animation) {
      case "scale":
        return {
          hidden: { opacity: 0, scale: 0.8 },
          visible: { opacity: 1, scale: 1 },
        };
      case "slide": {
        const slideOffset = 10;
        const slideVariants = {
          top: {
            hidden: { opacity: 0, y: slideOffset },
            visible: { opacity: 1, y: 0 },
          },
          bottom: {
            hidden: { opacity: 0, y: -slideOffset },
            visible: { opacity: 1, y: 0 },
          },
          left: {
            hidden: { opacity: 0, x: slideOffset },
            visible: { opacity: 1, x: 0 },
          },
          right: {
            hidden: { opacity: 0, x: -slideOffset },
            visible: { opacity: 1, x: 0 },
          },
        };
        return (
          slideVariants[baseDirection as keyof typeof slideVariants] ||
          slideVariants.top
        );
      }
      case "spring":
        return {
          hidden: { opacity: 0, scale: 0.5 },
          visible: { opacity: 1, scale: 1 },
        };
      default:
        return {
          hidden: { opacity: 0 },
          visible: { opacity: 1 },
        };
    }
  };

  const getArrowPosition = () => {
    const baseDirection = placement.split("-")[0];
    const alignment = placement.split("-")[1];

    const arrowClasses = {
      top: "bottom-0 left-1/2 -translate-x-1/2 translate-y-full border-t-foreground border-x-transparent border-b-transparent",
      bottom:
        "top-0 left-1/2 -translate-x-1/2 -translate-y-full border-b-foreground border-x-transparent border-t-transparent",
      left: "right-0 top-1/2 -translate-y-1/2 translate-x-full border-l-foreground border-y-transparent border-r-transparent",
      right:
        "left-0 top-1/2 -translate-y-1/2 -translate-x-full border-r-foreground border-y-transparent border-l-transparent",
    };

    let alignmentClass = "";
    if (alignment === "start") {
      alignmentClass =
        baseDirection === "top" || baseDirection === "bottom"
          ? "left-4 -translate-x-0"
          : "";
    } else if (alignment === "end") {
      alignmentClass =
        baseDirection === "top" || baseDirection === "bottom"
          ? "left-auto right-4 translate-x-0"
          : "";
    }

    return cn(
      arrowClasses[baseDirection as keyof typeof arrowClasses],
      alignmentClass,
    );
  };

  return (
    <>
      <button
        ref={triggerRef}
        className={cn("inline-block", className)}
        onMouseEnter={handleMouseEnter}
        onMouseLeave={handleMouseLeave}
        onFocus={handleMouseEnter}
        onBlur={handleMouseLeave}
        type="button"
      >
        {children}
      </button>

      <AnimatePresence>
        {isVisible && (
          <motion.div
            ref={tooltipRef}
            className={cn(
              "fixed z-50 max-w-xs rounded-md bg-foreground px-3 py-1.5 text-background text-sm shadow-lg",
              contentClassName,
            )}
            style={{ left: position.x, top: position.y }}
            variants={getAnimationVariants()}
            initial="hidden"
            animate="visible"
            exit="hidden"
            transition={
              animation === "spring"
                ? { type: "spring", stiffness: 500, damping: 25 }
                : { duration }
            }
          >
            {content}
            {arrow && (
              <div
                className={cn("absolute h-0 w-0 border-4", getArrowPosition())}
              />
            )}
          </motion.div>
        )}
      </AnimatePresence>
    </>
  );
}

// Tooltip with Rich Content
interface RichTooltipProps {
  children: React.ReactNode;
  title: string;
  description?: string;
  image?: string;
  placement?: Placement;
  className?: string;
}

export function RichTooltip({
  children,
  title,
  description,
  image,
  placement = "top",
  className,
}: RichTooltipProps) {
  return (
    <AnimatedTooltip
      placement={placement}
      animation="scale"
      arrow={false}
      className={className}
      contentClassName="p-0 overflow-hidden max-w-[280px]"
      content={
        <div className="rounded-lg border bg-card text-card-foreground shadow-xl">
          {image && (
            <div className="h-32 w-full overflow-hidden">
              {/* biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx */}
              <img
                src={image}
                alt={title}
                className="h-full w-full object-cover"
              />
            </div>
          )}
          <div className="p-3">
            <p className="font-semibold text-foreground">{title}</p>
            {description && (
              <p className="mt-1 text-muted-foreground text-sm">
                {description}
              </p>
            )}
          </div>
        </div>
      }
    >
      {children}
    </AnimatedTooltip>
  );
}

// Icon Tooltip (compact)
interface IconTooltipProps {
  children: React.ReactNode;
  label: string;
  placement?: Placement;
  shortcut?: string;
}

export function IconTooltip({
  children,
  label,
  placement = "top",
  shortcut,
}: IconTooltipProps) {
  return (
    <AnimatedTooltip
      placement={placement}
      animation="fade"
      delay={200}
      content={
        <div className="flex items-center gap-2">
          <span>{label}</span>
          {shortcut && (
            <kbd className="rounded bg-background/20 px-1.5 py-0.5 font-mono text-xs">
              {shortcut}
            </kbd>
          )}
        </div>
      }
    >
      {children}
    </AnimatedTooltip>
  );
}

// Hover Card (larger tooltip with delay)
interface HoverCardTooltipProps {
  children: React.ReactNode;
  content: React.ReactNode;
  placement?: Placement;
  className?: string;
}

export function HoverCardTooltip({
  children,
  content,
  placement = "bottom",
  className,
}: HoverCardTooltipProps) {
  return (
    <AnimatedTooltip
      placement={placement}
      animation="spring"
      delay={300}
      arrow={false}
      contentClassName={cn(
        "p-0 bg-card text-card-foreground border rounded-xl shadow-2xl max-w-sm",
        className,
      )}
      content={content}
    >
      {children}
    </AnimatedTooltip>
  );
}

// Confirmation Tooltip
interface ConfirmTooltipProps {
  children: React.ReactNode;
  message: string;
  onConfirm: () => void;
  onCancel?: () => void;
  placement?: Placement;
  confirmText?: string;
  cancelText?: string;
}

export function ConfirmTooltip({
  children,
  message,
  onConfirm,
  onCancel,
  placement: _placement = "top",
  confirmText = "Confirm",
  cancelText = "Cancel",
}: ConfirmTooltipProps) {
  const [isOpen, setIsOpen] = useState(false);

  const handleConfirm = () => {
    onConfirm();
    setIsOpen(false);
  };

  const handleCancel = () => {
    onCancel?.();
    setIsOpen(false);
  };

  return (
    <>
      <button
        type="button"
        onClick={() => setIsOpen(!isOpen)}
        className="inline-block cursor-pointer"
      >
        {children}
      </button>

      <AnimatePresence>
        {isOpen && (
          <>
            {/* Backdrop */}
            <motion.div
              className="fixed inset-0 z-40"
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              exit={{ opacity: 0 }}
              onClick={handleCancel}
            />

            {/* Tooltip */}
            <motion.div
              className="fixed z-50 w-64 rounded-lg border bg-card p-4 shadow-xl"
              initial={{ opacity: 0, scale: 0.9 }}
              animate={{ opacity: 1, scale: 1 }}
              exit={{ opacity: 0, scale: 0.9 }}
              transition={{ type: "spring", stiffness: 400, damping: 25 }}
            >
              <p className="mb-3 text-sm">{message}</p>
              <div className="flex justify-end gap-2">
                <button
                  onClick={handleCancel}
                  className="rounded-md px-3 py-1.5 text-muted-foreground text-sm hover:bg-accent"
                >
                  {cancelText}
                </button>
                <button
                  onClick={handleConfirm}
                  className="rounded-md bg-primary px-3 py-1.5 text-primary-foreground text-sm hover:bg-primary/90"
                >
                  {confirmText}
                </button>
              </div>
            </motion.div>
          </>
        )}
      </AnimatePresence>
    </>
  );
}

// Tooltip Group (for toolbar-style tooltips)
interface TooltipItem {
  icon: React.ReactNode;
  label: string;
  shortcut?: string;
  onClick?: () => void;
}

interface TooltipGroupProps {
  items: TooltipItem[];
  className?: string;
}

export function TooltipGroup({ items, className }: TooltipGroupProps) {
  return (
    <div
      className={cn(
        "inline-flex items-center gap-1 rounded-lg border bg-card p-1",
        className,
      )}
    >
      {items.map((item, index) => (
        <IconTooltip key={index} label={item.label} shortcut={item.shortcut}>
          <button
            onClick={item.onClick}
            className="flex h-8 w-8 items-center justify-center rounded-md text-muted-foreground transition-colors hover:bg-accent hover:text-foreground"
          >
            {item.icon}
          </button>
        </IconTooltip>
      ))}
    </div>
  );
}

// Floating Label (tooltip that stays visible on focus)
interface FloatingLabelProps {
  children: React.ReactNode;
  label: string;
  className?: string;
}

export function FloatingLabel({
  children,
  label,
  className,
}: FloatingLabelProps) {
  const [isFocused, setIsFocused] = useState(false);

  return (
    <div className={cn("relative", className)}>
      <AnimatePresence>
        {isFocused && (
          <motion.div
            className="absolute -top-6 left-0 font-medium text-primary text-xs"
            initial={{ opacity: 0, y: 5 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: 5 }}
            transition={{ duration: 0.15 }}
          >
            {label}
          </motion.div>
        )}
      </AnimatePresence>
      <div
        onFocus={() => setIsFocused(true)}
        onBlur={() => setIsFocused(false)}
        role="button"
        tabIndex={0}
      >
        {children}
      </div>
    </div>
  );
}

// Status Tooltip (with colored indicator)
interface StatusTooltipProps {
  children: React.ReactNode;
  status: "online" | "offline" | "away" | "busy";
  label?: string;
  placement?: Placement;
}

export function StatusTooltip({
  children,
  status,
  label,
  placement = "top",
}: StatusTooltipProps) {
  const statusConfig = {
    online: { color: "bg-green-500", text: "Online" },
    offline: { color: "bg-gray-400", text: "Offline" },
    away: { color: "bg-yellow-500", text: "Away" },
    busy: { color: "bg-red-500", text: "Busy" },
  };

  const config = statusConfig[status];

  return (
    <AnimatedTooltip
      placement={placement}
      animation="fade"
      content={
        <div className="flex items-center gap-2">
          <div className={cn("h-2 w-2 rounded-full", config.color)} />
          <span>{label || config.text}</span>
        </div>
      }
    >
      {children}
    </AnimatedTooltip>
  );
}

export type {
  AnimatedTooltipProps,
  Animation,
  ConfirmTooltipProps,
  FloatingLabelProps,
  HoverCardTooltipProps,
  IconTooltipProps,
  Placement,
  RichTooltipProps,
  StatusTooltipProps,
  TooltipGroupProps,
};
```
---

## Bento Grid <a name="bento-grid"></a>

Responsive bento box grid layout for React. Create Apple-style dashboard layouts with flexible grid areas. Perfect for landing pages and feature showcases.

- **Registry Key**: `bento-grid`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/bento-grid.json
```

#### Component Source Code
##### File: `ui/bento-grid.tsx`
```tsx
import * as React from "react";
import { cn } from "@/lib/utils";

interface BentoGridProps extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode;
}

const BentoGrid = React.forwardRef<HTMLDivElement, BentoGridProps>(
  ({ className, children, ...props }, ref) => {
    return (
      <div
        ref={ref}
        className={cn(
          "grid auto-rows-[minmax(180px,1fr)] grid-cols-1 gap-4 md:grid-cols-2 lg:grid-cols-3",
          className,
        )}
        {...props}
      >
        {children}
      </div>
    );
  },
);
BentoGrid.displayName = "BentoGrid";

export { BentoGrid };
```
---

## Button <a name="button"></a>

A fully accessible React button component with 10+ variants, loading states, and icon support. Copy-paste ready for shadcn/ui projects.

- **Registry Key**: `button`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/button.json
```

#### Dependencies
- **NPM Packages**:
  - `@radix-ui/react-slot`
  - `class-variance-authority`
  - `motion`

#### How to Use
```tsx
import { Button } from "@/registry/default/ui/button";

export default function ButtonDemo() {
  return <Button>Button</Button>;
}
```
#### Component Source Code
##### File: `ui/button.tsx`
```tsx
import { Slot } from "@radix-ui/react-slot";
import { cva, type VariantProps } from "class-variance-authority";
import { motion, useMotionTemplate, useMotionValue } from "motion/react";
import * as React from "react";

const buttonVariants = cva(
  "group relative inline-flex items-center justify-center gap-2 overflow-hidden whitespace-nowrap rounded-xl font-semibold text-sm transition-all duration-300 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0",
  {
    variants: {
      variant: {
        default:
          "bg-gradient-to-br from-primary via-primary to-primary/80 text-primary-foreground shadow-lg shadow-primary/25 before:absolute before:inset-0 before:bg-gradient-to-br before:from-white/20 before:to-transparent before:opacity-0 before:transition-opacity hover:scale-[1.02] hover:shadow-primary/30 hover:shadow-xl hover:before:opacity-100 active:scale-[0.98]",
        destructive:
          "bg-gradient-to-br from-destructive via-destructive to-destructive/80 text-destructive-foreground shadow-destructive/25 shadow-lg before:absolute before:inset-0 before:bg-gradient-to-br before:from-white/20 before:to-transparent before:opacity-0 before:transition-opacity hover:scale-[1.02] hover:shadow-destructive/30 hover:shadow-xl hover:before:opacity-100 active:scale-[0.98]",
        outline:
          "border-2 border-input bg-background/50 shadow-sm backdrop-blur-sm hover:scale-[1.02] hover:border-accent hover:bg-accent hover:text-accent-foreground hover:shadow-md active:scale-[0.98]",
        secondary:
          "bg-gradient-to-br from-secondary via-secondary to-secondary/80 text-secondary-foreground shadow-lg shadow-secondary/25 before:absolute before:inset-0 before:bg-gradient-to-br before:from-white/20 before:to-transparent before:opacity-0 before:transition-opacity hover:scale-[1.02] hover:shadow-secondary/30 hover:shadow-xl hover:before:opacity-100 active:scale-[0.98]",
        ghost:
          "backdrop-blur-sm hover:scale-[1.02] hover:bg-accent/80 hover:text-accent-foreground active:scale-[0.98]",
        link: "text-primary underline-offset-4 transition-colors hover:text-primary/80 hover:underline",
        shimmer:
          "relative animate-shimmer border border-slate-800/50 bg-[length:200%_100%] bg-[linear-gradient(110deg,#000103,45%,#1e2631,55%,#000103)] text-white shadow-2xl shadow-primary/30 before:absolute before:inset-0 before:translate-x-[-200%] before:animate-shimmer-slide before:bg-gradient-to-r before:from-transparent before:via-white/10 before:to-transparent hover:scale-[1.02] hover:shadow-primary/50 active:scale-[0.98]",
        glow: "relative animate-gradient-x bg-gradient-to-r from-violet-600 via-purple-600 to-fuchsia-600 text-white shadow-lg shadow-purple-500/50 before:absolute before:inset-[-2px] before:-z-10 before:rounded-xl before:bg-gradient-to-r before:from-violet-600 before:via-purple-600 before:to-fuchsia-600 before:opacity-75 before:blur-md hover:scale-[1.02] hover:shadow-2xl hover:shadow-purple-500/60 active:scale-[0.98]",
      },
      size: {
        default: "h-10 px-6 py-2",
        sm: "h-8 rounded-lg px-4 text-xs",
        lg: "h-12 rounded-xl px-10 text-base",
        icon: "h-10 w-10",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  },
);

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean;
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, asChild = false, children, ...props }, ref) => {
    const Comp = asChild ? Slot : "button";
    const mouseX = useMotionValue(0);
    const mouseY = useMotionValue(0);

    const handleMouseMove = (e: React.MouseEvent<HTMLButtonElement>) => {
      const { left, top } = e.currentTarget.getBoundingClientRect();
      mouseX.set(e.clientX - left);
      mouseY.set(e.clientY - top);
    };

    const background = useMotionTemplate`radial-gradient(circle at ${mouseX}px ${mouseY}px, rgba(255,255,255,0.15), transparent 80%)`;

    if (asChild) {
      return (
        <Comp
          className={`${buttonVariants({ variant, size })} ${className || ""}`}
          ref={ref}
          {...props}
        >
          {children}
        </Comp>
      );
    }

    const MotionButton = motion.button;

    return (
      <MotionButton
        className={`${buttonVariants({ variant, size })} ${className || ""}`}
        ref={ref}
        onMouseMove={handleMouseMove}
        whileHover={{ scale: variant === "link" ? 1 : 1.02 }}
        whileTap={{ scale: variant === "link" ? 1 : 0.98 }}
        transition={{ type: "spring", stiffness: 400, damping: 17 }}
        // biome-ignore lint/suspicious/noExplicitAny: Motion button requires any for props compatibility
        {...(props as any)}
      >
        {variant !== "link" && variant !== "ghost" && (
          <motion.div
            className="pointer-events-none absolute inset-0 opacity-0 transition-opacity duration-300 group-hover:opacity-100"
            style={{ background }}
          />
        )}
        <span className="relative z-10 flex items-center justify-center gap-2">
          {children}
        </span>
      </MotionButton>
    );
  },
);
Button.displayName = "Button";

export { Button, buttonVariants };
```
---

## Calender <a name="calender"></a>

- **Registry Key**: `calender`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/calender.json
```

#### Dependencies
- **NPM Packages**:
  - `lucide-react`
  - `motion`
  - `date-fns`

#### Component Source Code
##### File: `ui/calender.tsx`
```tsx
import {
  addDays,
  addMonths,
  addYears,
  eachDayOfInterval,
  endOfMonth,
  endOfWeek,
  endOfYear,
  format,
  getDay,
  getHours,
  getMinutes,
  getMonth,
  getYear,
  isAfter,
  isBefore,
  isSameDay,
  isSameMonth,
  isToday,
  isWeekend,
  isWithinInterval,
  type Locale,
  setHours,
  setMinutes,
  setMonth,
  setYear,
  startOfDay,
  startOfMonth,
  startOfWeek,
  startOfYear,
  subDays,
  subMonths,
  subYears,
} from "date-fns";
import { enUS } from "date-fns/locale";
import {
  AlertCircle,
  Calendar as CalendarIcon,
  Check,
  ChevronLeft,
  ChevronRight,
  ChevronsLeft,
  ChevronsRight,
  Clock,
  X,
} from "lucide-react";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import * as React from "react";
import {
  useCallback,
  useEffect,
  useId,
  useMemo,
  useRef,
  useState,
} from "react";
import { Button } from "@/components/ui/button";
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from "@/components/ui/popover";
import { cn } from "@/lib/utils";

// ============================================================================
// TYPES
// ============================================================================

export type CalendarMode = "single" | "range" | "multiple";
export type CalendarView = "days" | "months" | "years" | "time";
export type CalendarSize = "sm" | "md" | "lg";

export interface DateRange {
  from: Date | undefined;
  to: Date | undefined;
}

export interface PresetRange {
  label: string;
  getValue: () => DateRange;
}

export interface CalendarLocale {
  weekdays: string[];
  weekdaysShort: string[];
  months: string[];
  monthsShort: string[];
  today: string;
  clear: string;
  close: string;
  selectTime: string;
  backToCalendar: string;
  selected: string;
  weekNumber: string;
}

// Base props shared by all modes
interface BaseCalendarProps {
  // Display
  placeholder?: string;
  disabled?: boolean;
  readOnly?: boolean;
  required?: boolean;
  className?: string;
  size?: CalendarSize;

  // Constraints
  minDate?: Date;
  maxDate?: Date;
  disabledDates?: Date[];
  disabledDaysOfWeek?: number[];
  disableWeekends?: boolean;
  disablePastDates?: boolean;
  disableFutureDates?: boolean;

  // Validation
  error?: boolean;
  errorMessage?: string;

  // Features
  showTime?: boolean;
  use24Hour?: boolean;
  minuteStep?: number;
  showWeekNumbers?: boolean;
  showTodayButton?: boolean;
  showClearButton?: boolean;
  weekStartsOn?: 0 | 1 | 2 | 3 | 4 | 5 | 6;
  monthsToShow?: 1 | 2 | 3;

  // Range presets
  showPresets?: boolean;
  presets?: PresetRange[];

  // Events
  highlightedDates?: { date: Date; color?: string; label?: string }[];

  // Customization
  formatStr?: string;
  closeOnSelect?: boolean;
  locale?: Locale;
  localeStrings?: Partial<CalendarLocale>;

  // Callbacks
  onMonthChange?: (date: Date) => void;
  onYearChange?: (date: Date) => void;
  onViewChange?: (view: CalendarView) => void;
  onOpen?: () => void;
  onClose?: () => void;

  // Accessibility
  id?: string;
  name?: string;
  "aria-label"?: string;
  "aria-describedby"?: string;

  // Custom renderers
  renderDay?: (date: Date, defaultRender: React.ReactNode) => React.ReactNode;
  renderHeader?: (
    date: Date,
    defaultRender: React.ReactNode,
  ) => React.ReactNode;

  // Form integration
  onBlur?: () => void;
  onFocus?: () => void;
}

// Single date selection mode
interface SingleModeProps extends BaseCalendarProps {
  mode?: "single";
  value?: Date;
  defaultValue?: Date;
  onChange?: (value: Date | undefined) => void;
}

// Range selection mode
interface RangeModeProps extends BaseCalendarProps {
  mode: "range";
  value?: DateRange;
  defaultValue?: DateRange;
  onChange?: (value: DateRange | undefined) => void;
}

// Multiple dates selection mode
interface MultipleModeProps extends BaseCalendarProps {
  mode: "multiple";
  value?: Date[];
  defaultValue?: Date[];
  onChange?: (value: Date[]) => void;
}

// Union type for all calendar props (external API)
export type AnimatedCalendarProps =
  | SingleModeProps
  | RangeModeProps
  | MultipleModeProps;

// Internal unified type for component implementation
interface InternalCalendarProps extends BaseCalendarProps {
  mode?: CalendarMode;
  value?: Date | DateRange | Date[];
  defaultValue?: Date | DateRange | Date[];
  onChange?: (value: Date | DateRange | Date[] | undefined) => void;
}

// ============================================================================
// CONSTANTS & DEFAULTS
// ============================================================================

const defaultLocaleStrings: CalendarLocale = {
  weekdays: [
    "Sunday",
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday",
    "Saturday",
  ],
  weekdaysShort: ["Su", "Mo", "Tu", "We", "Th", "Fr", "Sa"],
  months: [
    "January",
    "February",
    "March",
    "April",
    "May",
    "June",
    "July",
    "August",
    "September",
    "October",
    "November",
    "December",
  ],
  monthsShort: [
    "Jan",
    "Feb",
    "Mar",
    "Apr",
    "May",
    "Jun",
    "Jul",
    "Aug",
    "Sep",
    "Oct",
    "Nov",
    "Dec",
  ],
  today: "Today",
  clear: "Clear",
  close: "Close",
  selectTime: "Select time",
  backToCalendar: "Back to calendar",
  selected: "selected",
  weekNumber: "Week",
};

const defaultPresets: PresetRange[] = [
  {
    label: "Today",
    getValue: () => ({
      from: startOfDay(new Date()),
      to: startOfDay(new Date()),
    }),
  },
  {
    label: "Yesterday",
    getValue: () => ({
      from: startOfDay(subDays(new Date(), 1)),
      to: startOfDay(subDays(new Date(), 1)),
    }),
  },
  {
    label: "Last 7 days",
    getValue: () => ({
      from: startOfDay(subDays(new Date(), 6)),
      to: startOfDay(new Date()),
    }),
  },
  {
    label: "Last 30 days",
    getValue: () => ({
      from: startOfDay(subDays(new Date(), 29)),
      to: startOfDay(new Date()),
    }),
  },
  {
    label: "This month",
    getValue: () => ({
      from: startOfMonth(new Date()),
      to: endOfMonth(new Date()),
    }),
  },
  {
    label: "Last month",
    getValue: () => ({
      from: startOfMonth(subMonths(new Date(), 1)),
      to: endOfMonth(subMonths(new Date(), 1)),
    }),
  },
  {
    label: "This year",
    getValue: () => ({
      from: startOfYear(new Date()),
      to: endOfYear(new Date()),
    }),
  },
];

const sizeClasses = {
  sm: { cell: "h-7 w-7 text-xs", header: "text-sm", container: "p-2" },
  md: { cell: "h-9 w-9 text-sm", header: "text-base", container: "p-4" },
  lg: { cell: "h-11 w-11 text-base", header: "text-lg", container: "p-5" },
};

// ============================================================================
// ANIMATION VARIANTS
// ============================================================================

const slideVariants = {
  enter: (direction: number) => ({ x: direction > 0 ? 280 : -280, opacity: 0 }),
  center: { x: 0, opacity: 1 },
  exit: (direction: number) => ({ x: direction < 0 ? 280 : -280, opacity: 0 }),
};

const fadeScale = {
  initial: { opacity: 0, scale: 0.95 },
  animate: { opacity: 1, scale: 1 },
  exit: { opacity: 0, scale: 0.95 },
};

// ============================================================================
// UTILITY HOOKS
// ============================================================================

function useControllableState<T>(
  controlledValue: T | undefined,
  defaultValue: T,
  onChange?: (value: T) => void,
): [T, (value: T) => void] {
  const [uncontrolledValue, setUncontrolledValue] = useState(defaultValue);
  const isControlled = controlledValue !== undefined;
  const value = isControlled ? controlledValue : uncontrolledValue;

  const setValue = useCallback(
    (newValue: T) => {
      if (!isControlled) {
        setUncontrolledValue(newValue);
      }
      onChange?.(newValue);
    },
    [isControlled, onChange],
  );

  return [value, setValue];
}

// ============================================================================
// SUB-COMPONENTS
// ============================================================================

// Time Picker
const TimePicker = React.memo(
  ({
    value,
    onChange,
    use24Hour = true,
    minuteStep = 5,
    size = "md",
    localeStrings,
    disabled,
  }: {
    value: Date;
    onChange: (date: Date) => void;
    use24Hour?: boolean;
    minuteStep?: number;
    size?: CalendarSize;
    localeStrings: CalendarLocale;
    disabled?: boolean;
  }) => {
    const hours = getHours(value);
    const minutes = getMinutes(value);
    const isPM = hours >= 12;
    const displayHours = use24Hour ? hours : hours % 12 || 12;
    const sizes = sizeClasses[size];

    const updateTime = useCallback(
      (newHours: number, newMinutes: number) => {
        let updated = setHours(value, Math.max(0, Math.min(23, newHours)));
        updated = setMinutes(updated, Math.max(0, Math.min(59, newMinutes)));
        onChange(updated);
      },
      [value, onChange],
    );

    const incrementHour = () => updateTime((hours + 1) % 24, minutes);
    const decrementHour = () => updateTime((hours - 1 + 24) % 24, minutes);
    const incrementMinute = () =>
      updateTime(hours, (minutes + minuteStep) % 60);
    const decrementMinute = () =>
      updateTime(hours, (minutes - minuteStep + 60) % 60);
    const toggleAMPM = () =>
      updateTime(isPM ? hours - 12 : hours + 12, minutes);

    return (
      <div
        className="pointer-events-auto flex items-center justify-center gap-3 px-2 py-4"
        role="group"
        aria-label={localeStrings.selectTime}
        onClick={(e) => e.stopPropagation()}
      >
        {/* Hours */}
        <div className="flex flex-col items-center gap-1">
          <button
            type="button"
            onClick={(e) => {
              e.preventDefault();
              e.stopPropagation();
              incrementHour();
            }}
            disabled={disabled}
            className="pointer-events-auto rounded-lg p-1.5 transition-colors hover:bg-accent disabled:opacity-50"
            aria-label="Increase hours"
          >
            <ChevronLeft className="h-4 w-4 rotate-90" />
          </button>
          <input
            type="text"
            inputMode="numeric"
            value={displayHours.toString().padStart(2, "0")}
            onClick={(e) => e.stopPropagation()}
            onChange={(e) => {
              const val = parseInt(e.target.value, 10) || 0;
              if (use24Hour) {
                updateTime(Math.min(23, Math.max(0, val)), minutes);
              } else {
                const newHours = Math.min(12, Math.max(1, val));
                updateTime(
                  isPM
                    ? newHours === 12
                      ? 12
                      : newHours + 12
                    : newHours === 12
                      ? 0
                      : newHours,
                  minutes,
                );
              }
            }}
            disabled={disabled}
            className={cn(
              "pointer-events-auto w-12 rounded border-none bg-transparent text-center font-bold font-mono focus:outline-none focus:ring-2 focus:ring-primary",
              sizes.header,
            )}
            aria-label="Hours"
            maxLength={2}
          />
          <button
            type="button"
            onClick={(e) => {
              e.preventDefault();
              e.stopPropagation();
              decrementHour();
            }}
            disabled={disabled}
            className="pointer-events-auto rounded-lg p-1.5 transition-colors hover:bg-accent disabled:opacity-50"
            aria-label="Decrease hours"
          >
            <ChevronRight className="h-4 w-4 rotate-90" />
          </button>
        </div>

        <span className={cn("font-bold text-muted-foreground", sizes.header)}>
          :
        </span>

        {/* Minutes */}
        <div className="flex flex-col items-center gap-1">
          <button
            type="button"
            onClick={(e) => {
              e.preventDefault();
              e.stopPropagation();
              incrementMinute();
            }}
            disabled={disabled}
            className="pointer-events-auto rounded-lg p-1.5 transition-colors hover:bg-accent disabled:opacity-50"
            aria-label="Increase minutes"
          >
            <ChevronLeft className="h-4 w-4 rotate-90" />
          </button>
          <input
            type="text"
            inputMode="numeric"
            value={minutes.toString().padStart(2, "0")}
            onClick={(e) => e.stopPropagation()}
            onChange={(e) => {
              const val = parseInt(e.target.value, 10) || 0;
              updateTime(hours, Math.min(59, Math.max(0, val)));
            }}
            disabled={disabled}
            className={cn(
              "pointer-events-auto w-12 rounded border-none bg-transparent text-center font-bold font-mono focus:outline-none focus:ring-2 focus:ring-primary",
              sizes.header,
            )}
            aria-label="Minutes"
            maxLength={2}
          />
          <button
            type="button"
            onClick={(e) => {
              e.preventDefault();
              e.stopPropagation();
              decrementMinute();
            }}
            disabled={disabled}
            className="pointer-events-auto rounded-lg p-1.5 transition-colors hover:bg-accent disabled:opacity-50"
            aria-label="Decrease minutes"
          >
            <ChevronRight className="h-4 w-4 rotate-90" />
          </button>
        </div>

        {/* AM/PM */}
        {!use24Hour && (
          <button
            type="button"
            onClick={(e) => {
              e.preventDefault();
              e.stopPropagation();
              toggleAMPM();
            }}
            disabled={disabled}
            className="pointer-events-auto ml-2 rounded-lg bg-accent px-3 py-2 font-semibold text-sm transition-colors hover:bg-accent/80 disabled:opacity-50"
            aria-label={`Switch to ${isPM ? "AM" : "PM"}`}
          >
            {isPM ? "PM" : "AM"}
          </button>
        )}
      </div>
    );
  },
);
TimePicker.displayName = "TimePicker";

// Month Picker
const MonthPicker = React.memo(
  ({
    currentMonth,
    onSelect,
    minDate,
    maxDate,
    size = "md",
    localeStrings,
    disabled,
    prefersReducedMotion,
  }: {
    currentMonth: Date;
    onSelect: (month: number) => void;
    minDate?: Date;
    maxDate?: Date;
    size?: CalendarSize;
    localeStrings: CalendarLocale;
    disabled?: boolean;
    prefersReducedMotion: boolean;
  }) => {
    const currentYear = getYear(currentMonth);
    const currentMonthIndex = getMonth(currentMonth);
    const _sizes = sizeClasses[size];

    const isMonthDisabled = useCallback(
      (month: number) => {
        if (disabled) return true;
        const date = new Date(currentYear, month, 1);
        if (minDate && isBefore(endOfMonth(date), startOfDay(minDate)))
          return true;
        if (maxDate && isAfter(startOfMonth(date), startOfDay(maxDate)))
          return true;
        return false;
      },
      [currentYear, minDate, maxDate, disabled],
    );

    return (
      <div
        className="grid grid-cols-3 gap-2 p-2"
        role="listbox"
        aria-label="Select month"
      >
        {localeStrings.monthsShort.map((month, index) => {
          const isDisabled = isMonthDisabled(index);
          const isSelected = index === currentMonthIndex;
          return (
            <motion.button
              key={month}
              type="button"
              role="option"
              aria-selected={isSelected}
              aria-disabled={isDisabled}
              initial={
                prefersReducedMotion ? false : { opacity: 0, scale: 0.8 }
              }
              animate={{ opacity: 1, scale: 1 }}
              transition={{ delay: prefersReducedMotion ? 0 : index * 0.02 }}
              whileHover={
                !isDisabled && !prefersReducedMotion
                  ? { scale: 1.05 }
                  : undefined
              }
              whileTap={
                !isDisabled && !prefersReducedMotion
                  ? { scale: 0.95 }
                  : undefined
              }
              onClick={() => !isDisabled && onSelect(index)}
              disabled={isDisabled}
              className={cn(
                "rounded-lg px-2 py-3 font-medium text-sm transition-all focus:outline-none focus:ring-2 focus:ring-primary",
                isSelected
                  ? "bg-primary text-primary-foreground shadow-md"
                  : "text-foreground hover:bg-accent",
                isDisabled && "cursor-not-allowed opacity-30",
              )}
            >
              {month}
            </motion.button>
          );
        })}
      </div>
    );
  },
);
MonthPicker.displayName = "MonthPicker";

// Year Picker
const YearPicker = React.memo(
  ({
    currentYear,
    onSelect,
    minDate,
    maxDate,
    size: _size = "md",
    disabled,
    prefersReducedMotion,
  }: {
    currentYear: number;
    onSelect: (year: number) => void;
    minDate?: Date;
    maxDate?: Date;
    size?: CalendarSize;
    disabled?: boolean;
    prefersReducedMotion: boolean;
  }) => {
    const [startYear, setStartYear] = useState(currentYear - 6);
    const years = useMemo(
      () => Array.from({ length: 12 }, (_, i) => startYear + i),
      [startYear],
    );

    const isYearDisabled = useCallback(
      (year: number) => {
        if (disabled) return true;
        if (minDate && year < getYear(minDate)) return true;
        if (maxDate && year > getYear(maxDate)) return true;
        return false;
      },
      [minDate, maxDate, disabled],
    );

    return (
      <div className="space-y-2 p-2" role="listbox" aria-label="Select year">
        <div className="mb-2 flex items-center justify-between">
          <button
            type="button"
            onClick={() => setStartYear((s) => s - 12)}
            className="rounded-lg p-1.5 transition-colors hover:bg-accent"
            aria-label="Previous 12 years"
          >
            <ChevronsLeft className="h-4 w-4" />
          </button>
          <span className="font-medium text-muted-foreground text-sm">
            {years[0]} – {years[years.length - 1]}
          </span>
          <button
            type="button"
            onClick={() => setStartYear((s) => s + 12)}
            className="rounded-lg p-1.5 transition-colors hover:bg-accent"
            aria-label="Next 12 years"
          >
            <ChevronsRight className="h-4 w-4" />
          </button>
        </div>
        <div className="grid grid-cols-3 gap-2">
          {years.map((year, index) => {
            const isDisabled = isYearDisabled(year);
            const isSelected = year === currentYear;
            return (
              <motion.button
                key={year}
                type="button"
                role="option"
                aria-selected={isSelected}
                aria-disabled={isDisabled}
                initial={
                  prefersReducedMotion ? false : { opacity: 0, scale: 0.8 }
                }
                animate={{ opacity: 1, scale: 1 }}
                transition={{ delay: prefersReducedMotion ? 0 : index * 0.02 }}
                whileHover={
                  !isDisabled && !prefersReducedMotion
                    ? { scale: 1.05 }
                    : undefined
                }
                whileTap={
                  !isDisabled && !prefersReducedMotion
                    ? { scale: 0.95 }
                    : undefined
                }
                onClick={() => !isDisabled && onSelect(year)}
                disabled={isDisabled}
                className={cn(
                  "rounded-lg px-2 py-3 font-medium text-sm transition-all focus:outline-none focus:ring-2 focus:ring-primary",
                  isSelected
                    ? "bg-primary text-primary-foreground shadow-md"
                    : "text-foreground hover:bg-accent",
                  isDisabled && "cursor-not-allowed opacity-30",
                )}
              >
                {year}
              </motion.button>
            );
          })}
        </div>
      </div>
    );
  },
);
YearPicker.displayName = "YearPicker";

// Presets Panel
const PresetsPanel = React.memo(
  ({
    presets,
    onSelect,
    disabled,
  }: {
    presets: PresetRange[];
    onSelect: (range: DateRange) => void;
    disabled?: boolean;
  }) => (
    <div
      className="mr-3 min-w-[140px] space-y-1 border-border border-r pr-3"
      role="group"
      aria-label="Quick date presets"
    >
      <span className="mb-2 block font-semibold text-muted-foreground text-xs uppercase tracking-wider">
        Quick Select
      </span>
      {presets.map((preset, index) => (
        <motion.button
          key={preset.label}
          type="button"
          initial={{ opacity: 0, x: -10 }}
          animate={{ opacity: 1, x: 0 }}
          transition={{ delay: index * 0.03 }}
          whileHover={{ x: 4 }}
          onClick={() => !disabled && onSelect(preset.getValue())}
          disabled={disabled}
          className="w-full rounded-lg px-3 py-2 text-left text-sm transition-colors hover:bg-accent focus:outline-none focus:ring-2 focus:ring-primary disabled:opacity-50"
        >
          {preset.label}
        </motion.button>
      ))}
    </div>
  ),
);
PresetsPanel.displayName = "PresetsPanel";

// ============================================================================
// MAIN CALENDAR CONTENT
// ============================================================================

const CalendarContent = React.memo(
  ({
    mode = "single",
    value,
    onChange,
    minDate,
    maxDate,
    disabledDates = [],
    disabledDaysOfWeek = [],
    disableWeekends = false,
    disablePastDates = false,
    disableFutureDates = false,
    showTime = false,
    use24Hour = true,
    minuteStep = 5,
    showWeekNumbers = false,
    showTodayButton = true,
    showClearButton = true,
    weekStartsOn = 0,
    monthsToShow = 1,
    showPresets = false,
    presets = defaultPresets,
    highlightedDates = [],
    closeOnSelect = true,
    size = "md",
    disabled = false,
    readOnly = false,
    localeStrings = defaultLocaleStrings,
    locale = enUS,
    onMonthChange,
    onYearChange,
    onViewChange,
    renderDay,
    onClose,
    id,
  }: InternalCalendarProps & {
    onClose?: () => void;
    localeStrings: CalendarLocale;
  }) => {
    const prefersReducedMotion = useReducedMotion() ?? false;
    const calendarRef = useRef<HTMLDivElement>(null);
    const announcerRef = useRef<HTMLDivElement>(null);
    const sizes = sizeClasses[size];

    // Get initial date from value
    const getInitialDate = useCallback(() => {
      if (!value) return new Date();
      if (mode === "single" && value instanceof Date) return value;
      if (mode === "range") return (value as DateRange).from || new Date();
      if (mode === "multiple" && Array.isArray(value))
        return value[0] || new Date();
      return new Date();
    }, [value, mode]);

    const [currentMonth, setCurrentMonth] = useState(getInitialDate);
    const [direction, setDirection] = useState(0);
    const [view, setView] = useState<CalendarView>("days");
    const [focusedDate, setFocusedDate] = useState<Date | null>(null);
    const [rangeHover, setRangeHover] = useState<Date | null>(null);
    const [rangeStart, setRangeStart] = useState<Date | undefined>(
      mode === "range" ? (value as DateRange)?.from : undefined,
    );

    // Announce changes for screen readers
    const announce = useCallback((message: string) => {
      if (announcerRef.current) {
        announcerRef.current.textContent = message;
      }
    }, []);

    // View change handler
    const handleViewChange = useCallback(
      (newView: CalendarView) => {
        setView(newView);
        onViewChange?.(newView);
        announce(`Switched to ${newView} view`);
      },
      [onViewChange, announce],
    );

    // Generate calendar days
    const generateDays = useCallback(
      (month: Date) => {
        const monthStart = startOfMonth(month);
        const monthEnd = endOfMonth(month);
        const calendarStart = startOfWeek(monthStart, { weekStartsOn });
        const calendarEnd = endOfWeek(monthEnd, { weekStartsOn });
        return eachDayOfInterval({ start: calendarStart, end: calendarEnd });
      },
      [weekStartsOn],
    );

    const getWeekDays = useCallback(() => {
      const days = [...localeStrings.weekdaysShort];
      return [...days.slice(weekStartsOn), ...days.slice(0, weekStartsOn)];
    }, [weekStartsOn, localeStrings.weekdaysShort]);

    // Navigation handlers
    const navigate = useCallback(
      (delta: number, type: "month" | "year") => {
        setDirection(delta);
        setCurrentMonth((prev) => {
          const newDate =
            type === "month"
              ? delta > 0
                ? addMonths(prev, 1)
                : subMonths(prev, 1)
              : delta > 0
                ? addYears(prev, 1)
                : subYears(prev, 1);

          if (type === "month") onMonthChange?.(newDate);
          else onYearChange?.(newDate);

          announce(format(newDate, "MMMM yyyy", { locale }));
          return newDate;
        });
      },
      [onMonthChange, onYearChange, announce, locale],
    );

    // Check if day is disabled
    const isDayDisabled = useCallback(
      (day: Date) => {
        if (disabled || readOnly) return true;
        const dayStart = startOfDay(day);
        const today = startOfDay(new Date());

        if (minDate && isBefore(dayStart, startOfDay(minDate))) return true;
        if (maxDate && isAfter(dayStart, startOfDay(maxDate))) return true;
        if (disabledDates.some((d) => isSameDay(d, day))) return true;
        if (disableWeekends && isWeekend(day)) return true;
        if (disabledDaysOfWeek.includes(getDay(day))) return true;
        if (disablePastDates && isBefore(dayStart, today)) return true;
        if (disableFutureDates && isAfter(dayStart, today)) return true;

        return false;
      },
      [
        disabled,
        readOnly,
        minDate,
        maxDate,
        disabledDates,
        disableWeekends,
        disabledDaysOfWeek,
        disablePastDates,
        disableFutureDates,
      ],
    );

    // Date selection handler
    const handleSelectDate = useCallback(
      (day: Date) => {
        if (isDayDisabled(day)) return;

        if (mode === "single") {
          const dateToSet =
            showTime && value instanceof Date
              ? setMinutes(setHours(day, getHours(value)), getMinutes(value))
              : day;
          onChange?.(dateToSet);
          announce(`Selected ${format(dateToSet, "PPPP", { locale })}`);
          if (closeOnSelect && !showTime) onClose?.();
        } else if (mode === "range") {
          if (!rangeStart) {
            setRangeStart(day);
            onChange?.({ from: day, to: undefined });
            announce(`Range start: ${format(day, "PP", { locale })}`);
          } else {
            const range = isBefore(day, rangeStart)
              ? { from: day, to: rangeStart }
              : { from: rangeStart, to: day };
            onChange?.(range);
            setRangeStart(undefined);
            announce(
              `Range: ${format(range.from, "PP", { locale })} to ${format(range.to, "PP", { locale })}`,
            );
            if (closeOnSelect) onClose?.();
          }
        } else if (mode === "multiple") {
          const currentDates = (value as Date[]) || [];
          const exists = currentDates.some((d) => isSameDay(d, day));
          const newDates = exists
            ? currentDates.filter((d) => !isSameDay(d, day))
            : [...currentDates, day];
          onChange?.(newDates);
          announce(
            `${exists ? "Deselected" : "Selected"} ${format(day, "PP", { locale })}. ${newDates.length} dates selected.`,
          );
        }
      },
      [
        mode,
        onChange,
        onClose,
        rangeStart,
        showTime,
        value,
        closeOnSelect,
        isDayDisabled,
        announce,
        locale,
      ],
    );

    // Selection state checks
    const isDaySelected = useCallback(
      (day: Date) => {
        if (mode === "single" && value instanceof Date)
          return isSameDay(day, value);
        if (mode === "range" && value) {
          const range = value as DateRange;
          return (
            (range.from && isSameDay(day, range.from)) ||
            (range.to && isSameDay(day, range.to))
          );
        }
        if (mode === "multiple" && Array.isArray(value)) {
          return value.some((d) => isSameDay(d, day));
        }
        return false;
      },
      [mode, value],
    );

    const isDayInRange = useCallback(
      (day: Date) => {
        if (mode !== "range") return false;
        const range = value as DateRange | undefined;

        // Completed range
        if (range?.from && range?.to) {
          return (
            isWithinInterval(day, { start: range.from, end: range.to }) &&
            !isSameDay(day, range.from) &&
            !isSameDay(day, range.to)
          );
        }

        // Hover preview
        if (rangeStart && rangeHover) {
          const start = isBefore(rangeHover, rangeStart)
            ? rangeHover
            : rangeStart;
          const end = isBefore(rangeHover, rangeStart)
            ? rangeStart
            : rangeHover;
          return (
            isWithinInterval(day, { start, end }) &&
            !isSameDay(day, start) &&
            !isSameDay(day, end)
          );
        }

        return false;
      },
      [mode, value, rangeStart, rangeHover],
    );

    const getHighlight = useCallback(
      (day: Date) => {
        return highlightedDates.find((h) => isSameDay(h.date, day));
      },
      [highlightedDates],
    );

    // Keyboard navigation
    useEffect(() => {
      const handleKeyDown = (e: KeyboardEvent) => {
        if (view !== "days" || disabled || readOnly) return;

        const baseDate =
          focusedDate || (value instanceof Date ? value : new Date());
        let newDate = baseDate;
        let handled = true;

        switch (e.key) {
          case "ArrowLeft":
            newDate = addDays(baseDate, -1);
            break;
          case "ArrowRight":
            newDate = addDays(baseDate, 1);
            break;
          case "ArrowUp":
            newDate = addDays(baseDate, -7);
            break;
          case "ArrowDown":
            newDate = addDays(baseDate, 7);
            break;
          case "Home":
            newDate = startOfMonth(baseDate);
            break;
          case "End":
            newDate = endOfMonth(baseDate);
            break;
          case "PageUp":
            newDate = e.shiftKey
              ? subYears(baseDate, 1)
              : subMonths(baseDate, 1);
            break;
          case "PageDown":
            newDate = e.shiftKey
              ? addYears(baseDate, 1)
              : addMonths(baseDate, 1);
            break;
          case "Enter":
          case " ":
            if (focusedDate && !isDayDisabled(focusedDate)) {
              handleSelectDate(focusedDate);
            }
            e.preventDefault();
            return;
          case "Escape":
            onClose?.();
            e.preventDefault();
            return;
          default:
            handled = false;
        }

        if (handled) {
          e.preventDefault();
          setFocusedDate(newDate);
          if (!isSameMonth(newDate, currentMonth)) {
            setDirection(isAfter(newDate, currentMonth) ? 1 : -1);
            setCurrentMonth(startOfMonth(newDate));
          }
          announce(format(newDate, "EEEE, MMMM d, yyyy", { locale }));
        }
      };

      const el = calendarRef.current;
      if (el) {
        el.addEventListener("keydown", handleKeyDown);
        return () => el.removeEventListener("keydown", handleKeyDown);
      }
    }, [
      focusedDate,
      view,
      currentMonth,
      value,
      disabled,
      readOnly,
      isDayDisabled,
      handleSelectDate,
      onClose,
      announce,
      locale,
    ]);

    // Action handlers
    const handleClear = useCallback(() => {
      onChange?.(undefined);
      setRangeStart(undefined);
      announce("Selection cleared");
    }, [onChange, announce]);

    const handlePresetSelect = useCallback(
      (range: DateRange) => {
        onChange?.(range);
        if (range.from) setCurrentMonth(range.from);
        announce(
          `Selected: ${range.from && range.to ? `${format(range.from, "PP")} to ${format(range.to, "PP")}` : "preset"}`,
        );
        if (closeOnSelect) onClose?.();
      },
      [onChange, closeOnSelect, onClose, announce],
    );

    const handleMonthSelect = useCallback(
      (month: number) => {
        const newDate = setMonth(currentMonth, month);
        setCurrentMonth(newDate);
        handleViewChange("days");
        onMonthChange?.(newDate);
      },
      [currentMonth, handleViewChange, onMonthChange],
    );

    const handleYearSelect = useCallback(
      (year: number) => {
        const newDate = setYear(currentMonth, year);
        setCurrentMonth(newDate);
        handleViewChange("months");
        onYearChange?.(newDate);
      },
      [currentMonth, handleViewChange, onYearChange],
    );

    const handleTimeChange = useCallback(
      (newDate: Date) => {
        if (mode === "single") {
          // Ensure we have a valid date, use today if needed
          const baseDate =
            value instanceof Date ? value : startOfDay(new Date());
          const updatedDate = setMinutes(
            setHours(baseDate, getHours(newDate)),
            getMinutes(newDate),
          );
          onChange?.(updatedDate);
          announce(
            `Time set to ${format(updatedDate, use24Hour ? "HH:mm" : "hh:mm a")}`,
          );
        }
      },
      [mode, onChange, use24Hour, announce, value],
    );

    const goToToday = useCallback(() => {
      const today = new Date();
      setDirection(isAfter(today, currentMonth) ? 1 : -1);
      setCurrentMonth(today);
      if (mode === "single" && !isDayDisabled(today)) {
        handleSelectDate(today);
      }
    }, [currentMonth, mode, isDayDisabled, handleSelectDate]);

    // Render single day cell
    const renderDayCell = useCallback(
      (day: Date, monthDate: Date, index: number) => {
        const isCurrentMonth = isSameMonth(day, monthDate);
        const isSelected = isDaySelected(day);
        const isTodayDate = isToday(day);
        const isDisabled = isDayDisabled(day);
        const inRange = isDayInRange(day);
        const highlight = getHighlight(day);
        const isFocused = focusedDate && isSameDay(day, focusedDate);

        const dayContent = (
          <motion.button
            key={day.toISOString()}
            type="button"
            role="gridcell"
            aria-selected={isSelected}
            aria-disabled={isDisabled}
            aria-current={isTodayDate ? "date" : undefined}
            aria-label={`${format(day, "EEEE, MMMM d, yyyy", { locale })}${isSelected ? ", selected" : ""}${isTodayDate ? ", today" : ""}${highlight ? `, ${highlight.label}` : ""}`}
            tabIndex={isFocused ? 0 : -1}
            initial={prefersReducedMotion ? false : { opacity: 0, scale: 0.8 }}
            animate={{ opacity: 1, scale: 1 }}
            transition={{
              duration: 0.1,
              delay: prefersReducedMotion ? 0 : index * 0.003,
            }}
            whileHover={
              !isDisabled && !prefersReducedMotion ? { scale: 1.1 } : undefined
            }
            whileTap={
              !isDisabled && !prefersReducedMotion ? { scale: 0.95 } : undefined
            }
            onClick={() => handleSelectDate(day)}
            onMouseEnter={() => {
              if (mode === "range" && rangeStart && !isDisabled)
                setRangeHover(day);
            }}
            onMouseLeave={() => setRangeHover(null)}
            onFocus={() => setFocusedDate(day)}
            disabled={isDisabled}
            className={cn(
              sizes.cell,
              "relative flex items-center justify-center rounded-lg font-medium outline-none transition-all",
              !isCurrentMonth && "text-muted-foreground/40",
              isDisabled && "cursor-not-allowed opacity-25",
              !isSelected &&
                isCurrentMonth &&
                !inRange &&
                "text-foreground hover:bg-accent",
              isSelected && "bg-primary text-primary-foreground shadow-sm",
              isTodayDate &&
                !isSelected &&
                "ring-2 ring-primary ring-offset-2 ring-offset-background",
              inRange && "rounded-none bg-primary/15",
              isFocused && "ring-2 ring-ring ring-offset-1",
            )}
          >
            <span className="relative z-10">{format(day, "d")}</span>

            {/* Today indicator */}
            {isTodayDate && !isSelected && (
              <span className="absolute bottom-0.5 left-1/2 h-1 w-1 -translate-x-1/2 rounded-full bg-primary" />
            )}

            {/* Event highlight */}
            {highlight && (
              <span
                className="absolute top-0.5 right-0.5 h-1.5 w-1.5 rounded-full"
                style={{
                  backgroundColor: highlight.color || "hsl(var(--primary))",
                }}
                title={highlight.label}
              />
            )}
          </motion.button>
        );

        return renderDay ? renderDay(day, dayContent) : dayContent;
      },
      [
        sizes.cell,
        isDaySelected,
        isDayDisabled,
        isDayInRange,
        getHighlight,
        focusedDate,
        handleSelectDate,
        mode,
        rangeStart,
        prefersReducedMotion,
        locale,
        renderDay,
      ],
    );

    // Render month grid
    const renderMonthGrid = useCallback(
      (monthDate: Date, isSecondary = false) => {
        const days = generateDays(monthDate);

        return (
          <div
            className="space-y-1"
            role="grid"
            aria-label={format(monthDate, "MMMM yyyy", { locale })}
          >
            {isSecondary && (
              <div className="mb-2 flex h-8 items-center justify-center">
                <span
                  className={cn("font-semibold text-foreground", sizes.header)}
                >
                  {format(monthDate, "MMMM yyyy", { locale })}
                </span>
              </div>
            )}

            {/* Week days header */}
            <div
              className={cn(
                "grid gap-0.5",
                showWeekNumbers ? "grid-cols-8" : "grid-cols-7",
              )}
              role="row"
              tabIndex={-1}
            >
              {showWeekNumbers && (
                <div
                  className={cn(
                    sizes.cell,
                    "flex items-center justify-center font-medium text-muted-foreground text-xs",
                  )}
                  role="columnheader"
                  tabIndex={-1}
                >
                  #
                </div>
              )}
              {getWeekDays().map((day, i) => (
                <div
                  key={day}
                  role="columnheader"
                  aria-label={localeStrings.weekdays[(weekStartsOn + i) % 7]}
                  className={cn(
                    sizes.cell,
                    "flex items-center justify-center font-semibold text-muted-foreground text-xs",
                  )}
                  tabIndex={-1}
                >
                  {day}
                </div>
              ))}
            </div>

            {/* Days grid */}
            <AnimatePresence mode="wait" custom={direction}>
              <motion.div
                key={format(monthDate, "yyyy-MM")}
                custom={direction}
                variants={prefersReducedMotion ? undefined : slideVariants}
                initial={isSecondary || prefersReducedMotion ? false : "enter"}
                animate="center"
                exit={isSecondary || prefersReducedMotion ? undefined : "exit"}
                transition={{ duration: prefersReducedMotion ? 0 : 0.2 }}
                className={cn(
                  "grid gap-0.5",
                  showWeekNumbers ? "grid-cols-8" : "grid-cols-7",
                )}
                role="rowgroup"
              >
                {days.map((day, index) => {
                  const showWeekNumber = showWeekNumbers && index % 7 === 0;
                  return (
                    <React.Fragment key={day.toISOString()}>
                      {showWeekNumber && (
                        <div
                          className={cn(
                            sizes.cell,
                            "flex items-center justify-center text-muted-foreground text-xs",
                          )}
                          role="rowheader"
                          tabIndex={-1}
                        >
                          {format(day, "w")}
                        </div>
                      )}
                      {renderDayCell(day, monthDate, index)}
                    </React.Fragment>
                  );
                })}
              </motion.div>
            </AnimatePresence>
          </div>
        );
      },
      [
        generateDays,
        getWeekDays,
        showWeekNumbers,
        sizes,
        direction,
        prefersReducedMotion,
        renderDayCell,
        locale,
        localeStrings.weekdays,
        weekStartsOn,
      ],
    );

    const calendarWidth =
      monthsToShow === 1
        ? "w-auto"
        : monthsToShow === 2
          ? "min-w-[580px]"
          : "min-w-[860px]";

    return (
      <motion.div
        ref={calendarRef}
        id={id}
        tabIndex={0}
        role="application"
        aria-label="Calendar"
        initial={
          prefersReducedMotion ? false : { opacity: 0, scale: 0.95, y: -10 }
        }
        animate={{ opacity: 1, scale: 1, y: 0 }}
        transition={{ duration: prefersReducedMotion ? 0 : 0.2 }}
        className={cn(
          "pointer-events-auto overflow-hidden rounded-xl border bg-card shadow-black/10 shadow-xl focus:outline-none focus:ring-2 focus:ring-primary",
          sizes.container,
          calendarWidth,
          showPresets && mode === "range" && "flex",
          disabled && "pointer-events-none opacity-50",
        )}
      >
        {/* Screen reader announcer */}
        <div
          ref={announcerRef}
          className="sr-only"
          role="status"
          aria-live="polite"
          aria-atomic="true"
        />

        {/* Presets panel */}
        {showPresets && mode === "range" && (
          <PresetsPanel
            presets={presets}
            onSelect={handlePresetSelect}
            disabled={disabled}
          />
        )}

        <div className="flex-1">
          {/* Header */}
          <div className="mb-3 flex items-center justify-between">
            <div className="flex items-center gap-0.5">
              <button
                type="button"
                onClick={() => navigate(-1, "year")}
                disabled={disabled}
                className="rounded-lg p-1.5 transition-colors hover:bg-accent disabled:opacity-50"
                aria-label="Previous year"
              >
                <ChevronsLeft className="h-4 w-4" />
              </button>
              <button
                type="button"
                onClick={() => navigate(-1, "month")}
                disabled={disabled}
                className="rounded-lg p-1.5 transition-colors hover:bg-accent disabled:opacity-50"
                aria-label="Previous month"
              >
                <ChevronLeft className="h-4 w-4" />
              </button>
            </div>

            <div className="flex items-center gap-1">
              <button
                type="button"
                onClick={() =>
                  handleViewChange(view === "months" ? "days" : "months")
                }
                disabled={disabled}
                className={cn(
                  "rounded-lg px-2 py-1 font-bold transition-colors hover:bg-accent",
                  sizes.header,
                )}
                aria-label={`Select month, currently ${format(currentMonth, "MMMM", { locale })}`}
              >
                {format(currentMonth, "MMMM", { locale })}
              </button>
              <button
                type="button"
                onClick={() =>
                  handleViewChange(view === "years" ? "days" : "years")
                }
                disabled={disabled}
                className={cn(
                  "rounded-lg px-2 py-1 font-bold transition-colors hover:bg-accent",
                  sizes.header,
                )}
                aria-label={`Select year, currently ${format(currentMonth, "yyyy")}`}
              >
                {format(currentMonth, "yyyy")}
              </button>
            </div>

            <div className="flex items-center gap-0.5">
              <button
                type="button"
                onClick={() => navigate(1, "month")}
                disabled={disabled}
                className="rounded-lg p-1.5 transition-colors hover:bg-accent disabled:opacity-50"
                aria-label="Next month"
              >
                <ChevronRight className="h-4 w-4" />
              </button>
              <button
                type="button"
                onClick={() => navigate(1, "year")}
                disabled={disabled}
                className="rounded-lg p-1.5 transition-colors hover:bg-accent disabled:opacity-50"
                aria-label="Next year"
              >
                <ChevronsRight className="h-4 w-4" />
              </button>
            </div>
          </div>

          {/* View content */}
          <AnimatePresence mode="wait">
            {view === "days" && (
              <motion.div
                key="days"
                {...(prefersReducedMotion ? {} : fadeScale)}
                className="flex gap-4"
              >
                {renderMonthGrid(currentMonth)}
                {monthsToShow >= 2 && (
                  <>
                    <div className="w-px bg-border" />
                    {renderMonthGrid(addMonths(currentMonth, 1), true)}
                  </>
                )}
                {monthsToShow === 3 && (
                  <>
                    <div className="w-px bg-border" />
                    {renderMonthGrid(addMonths(currentMonth, 2), true)}
                  </>
                )}
              </motion.div>
            )}
            {view === "months" && (
              <MonthPicker
                key="months"
                currentMonth={currentMonth}
                onSelect={handleMonthSelect}
                minDate={minDate}
                maxDate={maxDate}
                size={size}
                localeStrings={localeStrings}
                disabled={disabled}
                prefersReducedMotion={prefersReducedMotion}
              />
            )}
            {view === "years" && (
              <YearPicker
                key="years"
                currentYear={getYear(currentMonth)}
                onSelect={handleYearSelect}
                minDate={minDate}
                maxDate={maxDate}
                size={size}
                disabled={disabled}
                prefersReducedMotion={prefersReducedMotion}
              />
            )}
            {view === "time" && mode === "single" && (
              <TimePicker
                key="time"
                value={value instanceof Date ? value : new Date()}
                onChange={handleTimeChange}
                use24Hour={use24Hour}
                minuteStep={minuteStep}
                size={size}
                localeStrings={localeStrings}
                disabled={disabled}
              />
            )}
          </AnimatePresence>

          {/* Time toggle */}
          {showTime && mode === "single" && view === "days" && (
            <button
              type="button"
              onClick={(e) => {
                e.preventDefault();
                e.stopPropagation();
                // If no date selected, select today first
                if (!(value instanceof Date)) {
                  const today = new Date();
                  onChange?.(today);
                }
                handleViewChange("time");
              }}
              disabled={disabled}
              className="mt-3 flex w-full items-center justify-center gap-2 rounded-lg bg-accent/50 py-2 font-medium text-sm transition-colors hover:bg-accent disabled:opacity-50"
            >
              <Clock className="h-4 w-4" />
              {value instanceof Date
                ? format(value, use24Hour ? "HH:mm" : "hh:mm a")
                : localeStrings.selectTime}
            </button>
          )}

          {view === "time" && (
            <button
              type="button"
              onClick={(e) => {
                e.preventDefault();
                e.stopPropagation();
                handleViewChange("days");
              }}
              disabled={disabled}
              className="mt-3 flex w-full items-center justify-center gap-2 rounded-lg bg-accent/50 py-2 font-medium text-sm transition-colors hover:bg-accent disabled:opacity-50"
            >
              <CalendarIcon className="h-4 w-4" />
              {localeStrings.backToCalendar}
            </button>
          )}

          {/* Footer */}
          {(showTodayButton ||
            showClearButton ||
            (mode === "multiple" && value)) &&
            view === "days" && (
              <div className="mt-4 flex items-center justify-between border-border/50 border-t pt-3">
                <div className="flex items-center gap-2">
                  {showTodayButton && (
                    <button
                      type="button"
                      onClick={goToToday}
                      disabled={disabled}
                      className="flex items-center gap-1 rounded-md px-3 py-1.5 font-semibold text-xs transition-colors hover:bg-accent disabled:opacity-50"
                    >
                      <Check className="h-3 w-3" />
                      {localeStrings.today}
                    </button>
                  )}
                  {mode === "multiple" &&
                    Array.isArray(value) &&
                    value.length > 0 && (
                      <span className="text-muted-foreground text-xs">
                        {value.length} {localeStrings.selected}
                      </span>
                    )}
                </div>
                {showClearButton && value && (
                  <button
                    type="button"
                    onClick={handleClear}
                    disabled={disabled}
                    className="flex items-center gap-1 rounded-md px-3 py-1.5 font-medium text-muted-foreground text-xs transition-colors hover:bg-accent disabled:opacity-50"
                  >
                    <X className="h-3 w-3" />
                    {localeStrings.clear}
                  </button>
                )}
              </div>
            )}
        </div>
      </motion.div>
    );
  },
);
CalendarContent.displayName = "CalendarContent";

// ============================================================================
// POPOVER CALENDAR
// ============================================================================

export function AnimatedCalendar({
  mode = "single",
  value: controlledValue,
  defaultValue,
  onChange,
  placeholder = "Pick a date",
  disabled = false,
  readOnly = false,
  required = false,
  error = false,
  errorMessage,
  className,
  size = "md",
  formatStr,
  showTime,
  use24Hour = true,
  locale = enUS,
  localeStrings: customLocaleStrings,
  onOpen,
  onClose,
  onBlur,
  onFocus,
  id,
  name,
  "aria-label": ariaLabel,
  "aria-describedby": ariaDescribedBy,
  ...props
}: AnimatedCalendarProps) {
  const [isOpen, setIsOpen] = useState(false);
  const generatedId = useId();
  const triggerId = id || generatedId;
  const errorId = `${triggerId}-error`;

  const localeStrings = useMemo(
    () => ({
      ...defaultLocaleStrings,
      ...customLocaleStrings,
    }),
    [customLocaleStrings],
  );

  // Internal state uses unified type for implementation
  type InternalValue = Date | DateRange | Date[] | undefined;
  const [value, setValue] = useControllableState<InternalValue>(
    controlledValue as InternalValue,
    (defaultValue ?? (mode === "multiple" ? [] : undefined)) as InternalValue,
    onChange as ((value: InternalValue) => void) | undefined,
  );

  const handleOpenChange = useCallback(
    (open: boolean) => {
      setIsOpen(open);
      if (open) {
        onOpen?.();
        onFocus?.();
      } else {
        onClose?.();
        onBlur?.();
      }
    },
    [onOpen, onClose, onFocus, onBlur],
  );

  const getDisplayValue = useMemo(() => {
    if (!value) return placeholder;

    if (mode === "single" && value instanceof Date) {
      const fmt =
        formatStr ||
        (showTime ? (use24Hour ? "PPP HH:mm" : "PPP hh:mm a") : "PPP");
      return format(value, fmt, { locale });
    }
    if (mode === "range") {
      const range = value as DateRange;
      if (range.from && range.to) {
        return `${format(range.from, "MMM d", { locale })} – ${format(range.to, "MMM d, yyyy", { locale })}`;
      }
      if (range.from)
        return `${format(range.from, "MMM d, yyyy", { locale })} – ...`;
      return placeholder;
    }
    if (mode === "multiple" && Array.isArray(value)) {
      if (value.length === 0) return placeholder;
      const firstDate = value[0];
      if (value.length === 1 && firstDate)
        return format(firstDate, "PPP", { locale });
      return `${value.length} dates selected`;
    }
    return placeholder;
  }, [value, mode, placeholder, formatStr, showTime, use24Hour, locale]);

  const sizeClasses = {
    sm: "w-[240px] h-8 text-xs",
    md: "w-[280px] h-10 text-sm",
    lg: "w-[320px] h-12 text-base",
  };

  return (
    <div className="relative">
      <Popover open={isOpen} onOpenChange={handleOpenChange}>
        <PopoverTrigger asChild>
          <Button
            id={triggerId}
            type="button"
            variant="outline"
            disabled={disabled}
            aria-label={ariaLabel || placeholder}
            aria-describedby={cn(
              ariaDescribedBy,
              error && errorMessage && errorId,
            )}
            aria-invalid={error}
            aria-required={required}
            aria-expanded={isOpen}
            aria-haspopup="dialog"
            className={cn(
              sizeClasses[size],
              "justify-start text-left font-normal",
              !value && "text-muted-foreground",
              error && "border-destructive focus:ring-destructive",
              className,
            )}
          >
            <CalendarIcon className="mr-2 h-4 w-4 shrink-0" />
            <span className="flex-1 truncate">{getDisplayValue}</span>
            {required && <span className="ml-1 text-destructive">*</span>}
          </Button>
        </PopoverTrigger>
        <PopoverContent
          className="w-auto border-0 bg-transparent p-0 shadow-none"
          align="start"
        >
          <CalendarContent
            {...(props as InternalCalendarProps)}
            mode={mode}
            value={value as Date | DateRange | Date[] | undefined}
            onChange={
              setValue as (value: Date | DateRange | Date[] | undefined) => void
            }
            disabled={disabled}
            readOnly={readOnly}
            showTime={showTime}
            use24Hour={use24Hour}
            size={size}
            locale={locale}
            localeStrings={localeStrings as CalendarLocale}
            onClose={() => handleOpenChange(false)}
          />
        </PopoverContent>
      </Popover>

      {/* Hidden input for form integration */}
      {name && (
        <input
          type="hidden"
          name={name}
          value={
            value instanceof Date ? value.toISOString() : JSON.stringify(value)
          }
        />
      )}

      {/* Error message */}
      {error && errorMessage && (
        <p
          id={errorId}
          className="mt-1.5 flex items-center gap-1 text-destructive text-xs"
        >
          <AlertCircle className="h-3 w-3" />
          {errorMessage}
        </p>
      )}
    </div>
  );
}

// ============================================================================
// STANDALONE CALENDAR
// ============================================================================

export function AnimatedCalendarStandalone({
  localeStrings: customLocaleStrings,
  ...props
}: Omit<
  AnimatedCalendarProps,
  "placeholder" | "onOpen" | "onClose" | "onBlur" | "onFocus"
>) {
  const localeStrings = useMemo(
    () => ({
      ...defaultLocaleStrings,
      ...customLocaleStrings,
    }),
    [customLocaleStrings],
  );

  return (
    <CalendarContent
      {...(props as InternalCalendarProps)}
      localeStrings={localeStrings as CalendarLocale}
    />
  );
}
```
---

## Character Morph <a name="character-morph"></a>

Character-by-character text morph for React. Staggered letter animations with customizable timing. Create stunning word transition effects.

- **Registry Key**: `character-morph`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/character-morph.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
"use client";

import { CharacterMorph } from "@/registry/default/ui/character-morph";

export default function CharacterMorphDemo() {
  return (
    <div className="flex items-center justify-center">
      <CharacterMorph
        texts={["Hello", "World", "Morph", "Effect"]}
        className="font-bold text-4xl text-foreground"
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/character-morph.tsx`
```tsx
import { AnimatePresence, motion } from "motion/react";
import * as React from "react";
import { cn } from "@/lib/utils";

interface CharacterMorphProps {
  texts: string[];
  className?: string;
  interval?: number;
  staggerDelay?: number;
  charDuration?: number;
}

const CharacterMorph = React.forwardRef<HTMLDivElement, CharacterMorphProps>(
  (
    {
      texts,
      className,
      interval = 3000,
      staggerDelay = 0.03,
      charDuration = 0.5,
    },
    ref,
  ) => {
    const [currentIndex, setCurrentIndex] = React.useState(0);
    const currentText = texts[currentIndex] || "";

    React.useEffect(() => {
      const timer = setInterval(() => {
        setCurrentIndex((prev) => (prev + 1) % texts.length);
      }, interval);

      return () => clearInterval(timer);
    }, [interval, texts.length]);

    const maxLength = Math.max(...texts.map((t) => t.length));

    return (
      <div
        ref={ref}
        className={cn("relative inline-flex whitespace-nowrap", className)}
      >
        <AnimatePresence mode="popLayout">
          {currentText.split("").map((char, i) => (
            <motion.span
              key={`${currentIndex}-${i}-${char}`}
              initial={{ opacity: 0, y: 20, filter: "blur(8px)", rotateX: -90 }}
              animate={{ opacity: 1, y: 0, filter: "blur(0px)", rotateX: 0 }}
              exit={{ opacity: 0, y: -20, filter: "blur(8px)", rotateX: 90 }}
              transition={{
                duration: charDuration,
                delay: i * staggerDelay,
                ease: [0.215, 0.61, 0.355, 1],
              }}
              className="inline-block"
              style={{ transformStyle: "preserve-3d" }}
            >
              {char === " " ? "\u00A0" : char}
            </motion.span>
          ))}
        </AnimatePresence>
        {/* Maintain minimum width */}
        <span className="invisible absolute">{"M".repeat(maxLength)}</span>
      </div>
    );
  },
);

CharacterMorph.displayName = "CharacterMorph";
export { CharacterMorph };
```
---

## Code Block <a name="code-block"></a>

Syntax-highlighted code block for React. Copy button, line numbers, file tabs, and terminal styling. Built on Shiki for accurate highlighting.

- **Registry Key**: `code-block`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/code-block.json
```

#### Dependencies
- **NPM Packages**:
  - `lucide-react`
  - `motion`
  - `prism-react-renderer`

#### How to Use
```tsx
import { CodeBlock } from "@/registry/default/ui/code-block";

const code = `import React from "react";

function HelloWorld() {
  return (
    <div className="p-4 rounded-lg bg-primary text-primary-foreground">
      <h1>Hello, World!</h1>
      <p>This is a simple React component.</p>
    </div>
  );
}

export default HelloWorld;`;

export default function CodeBlockDemo() {
  return (
    <div className="w-full max-w-3xl">
      <CodeBlock
        code={code}
        language="tsx"
        title="HelloWorld.tsx"
        highlightLines={[6]}
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/code-block.tsx`
```tsx
import {
  Check,
  Copy,
  Download,
  FileCode,
  Maximize2,
  Minimize2,
  Terminal,
  WrapText,
} from "lucide-react";
import { AnimatePresence, motion } from "motion/react";
import { Highlight, type Language, themes } from "prism-react-renderer";
import * as React from "react";
import { cn } from "@/lib/utils";

type CodeBlockVariant =
  | "default"
  | "terminal"
  | "minimal"
  | "gradient"
  | "glass";
type AnimationType = "none" | "fadeIn" | "slideIn" | "typewriter" | "highlight";
type ThemeType =
  | "oneDark"
  | "dracula"
  | "github"
  | "nightOwl"
  | "oceanicNext"
  | "palenight"
  | "shadesOfPurple"
  | "synthwave84"
  | "vsDark"
  | "vsLight";

// Theme mapping
const themeMap: Record<ThemeType, typeof themes.oneDark> = {
  oneDark: themes.oneDark,
  dracula: themes.dracula,
  github: themes.github,
  nightOwl: themes.nightOwl,
  oceanicNext: themes.oceanicNext,
  palenight: themes.palenight,
  shadesOfPurple: themes.shadesOfPurple,
  synthwave84: themes.synthwave84,
  vsDark: themes.vsDark,
  vsLight: themes.vsLight,
};

// Supported languages list
const supportedLanguages = [
  "javascript",
  "typescript",
  "jsx",
  "tsx",
  "python",
  "bash",
  "shell",
  "css",
  "scss",
  "html",
  "json",
  "yaml",
  "markdown",
  "sql",
  "graphql",
  "rust",
  "go",
  "java",
  "c",
  "cpp",
  "csharp",
  "php",
  "ruby",
  "swift",
  "kotlin",
  "scala",
  "r",
  "lua",
  "perl",
  "haskell",
  "elixir",
  "clojure",
  "dockerfile",
  "toml",
  "ini",
  "xml",
  "diff",
  "makefile",
  "regex",
] as const;

interface CodeBlockProps {
  code: string;
  language?: Language | string;
  title?: string;
  showLineNumbers?: boolean;
  highlightLines?: number[];
  addedLines?: number[];
  removedLines?: number[];
  variant?: CodeBlockVariant;
  animation?: AnimationType;
  animationDelay?: number;
  className?: string;
  copyable?: boolean;
  downloadable?: boolean;
  downloadFileName?: string;
  maxHeight?: string;
  theme?: ThemeType;
  wrapLongLines?: boolean;
  showLanguage?: boolean;
  collapsible?: boolean;
  defaultCollapsed?: boolean;
  startingLineNumber?: number;
  caption?: string;
}

// Copy button component
const CopyButton = ({
  code,
  className,
}: {
  code: string;
  className?: string;
}) => {
  const [copied, setCopied] = React.useState(false);

  const handleCopy = async () => {
    try {
      await navigator.clipboard.writeText(code);
      setCopied(true);
      setTimeout(() => setCopied(false), 2000);
    } catch (err) {
      console.error("Failed to copy:", err);
    }
  };

  return (
    <motion.button
      onClick={handleCopy}
      className={cn(
        "rounded-md p-2 text-muted-foreground transition-colors hover:bg-muted hover:text-foreground",
        className,
      )}
      whileHover={{ scale: 1.05 }}
      whileTap={{ scale: 0.95 }}
      aria-label={copied ? "Copied!" : "Copy code"}
      title={copied ? "Copied!" : "Copy code"}
    >
      <AnimatePresence mode="wait">
        {copied ? (
          <motion.div
            key="check"
            initial={{ scale: 0, rotate: -180 }}
            animate={{ scale: 1, rotate: 0 }}
            exit={{ scale: 0, rotate: 180 }}
            transition={{ duration: 0.2 }}
          >
            <Check className="h-4 w-4 text-green-500" />
          </motion.div>
        ) : (
          <motion.div
            key="copy"
            initial={{ scale: 0 }}
            animate={{ scale: 1 }}
            exit={{ scale: 0 }}
            transition={{ duration: 0.2 }}
          >
            <Copy className="h-4 w-4" />
          </motion.div>
        )}
      </AnimatePresence>
    </motion.button>
  );
};

// Download button component
const DownloadButton = ({
  code,
  fileName,
  language,
}: {
  code: string;
  fileName?: string;
  language: string;
}) => {
  const handleDownload = () => {
    const extension = getFileExtension(language);
    const name = fileName || `code.${extension}`;
    const blob = new Blob([code], { type: "text/plain" });
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url;
    a.download = name;
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    URL.revokeObjectURL(url);
  };

  return (
    <motion.button
      onClick={handleDownload}
      className="rounded-md p-2 text-muted-foreground transition-colors hover:bg-muted hover:text-foreground"
      whileHover={{ scale: 1.05 }}
      whileTap={{ scale: 0.95 }}
      aria-label="Download code"
      title="Download code"
    >
      <Download className="h-4 w-4" />
    </motion.button>
  );
};

// Get file extension from language
const getFileExtension = (language: string): string => {
  const extensions: Record<string, string> = {
    javascript: "js",
    typescript: "ts",
    jsx: "jsx",
    tsx: "tsx",
    python: "py",
    bash: "sh",
    shell: "sh",
    css: "css",
    scss: "scss",
    html: "html",
    json: "json",
    yaml: "yml",
    markdown: "md",
    sql: "sql",
    graphql: "graphql",
    rust: "rs",
    go: "go",
    java: "java",
    c: "c",
    cpp: "cpp",
    csharp: "cs",
    php: "php",
    ruby: "rb",
    swift: "swift",
    kotlin: "kt",
    dockerfile: "dockerfile",
    toml: "toml",
    xml: "xml",
  };
  return extensions[language] || "txt";
};

// Language display names
const getLanguageDisplayName = (language: string): string => {
  const names: Record<string, string> = {
    javascript: "JavaScript",
    typescript: "TypeScript",
    jsx: "JSX",
    tsx: "TSX",
    python: "Python",
    bash: "Bash",
    shell: "Shell",
    css: "CSS",
    scss: "SCSS",
    html: "HTML",
    json: "JSON",
    yaml: "YAML",
    markdown: "Markdown",
    sql: "SQL",
    graphql: "GraphQL",
    rust: "Rust",
    go: "Go",
    java: "Java",
    c: "C",
    cpp: "C++",
    csharp: "C#",
    php: "PHP",
    ruby: "Ruby",
    swift: "Swift",
    kotlin: "Kotlin",
    dockerfile: "Dockerfile",
    toml: "TOML",
    xml: "XML",
    diff: "Diff",
  };
  return (
    names[language] || language.charAt(0).toUpperCase() + language.slice(1)
  );
};

// Typewriter code animation component
const TypewriterCode = ({
  code,
  language,
  speed = 20,
  showLineNumbers,
  highlightLines = [],
  startingLineNumber = 1,
  theme,
}: {
  code: string;
  language: Language;
  speed?: number;
  showLineNumbers: boolean;
  highlightLines: number[];
  startingLineNumber: number;
  theme: typeof themes.oneDark;
}) => {
  const [displayedCode, setDisplayedCode] = React.useState("");
  const [currentIndex, setCurrentIndex] = React.useState(0);

  React.useEffect(() => {
    if (currentIndex >= code.length) return;

    const timeout = setTimeout(() => {
      setDisplayedCode(code.slice(0, currentIndex + 1));
      setCurrentIndex((prev) => prev + 1);
    }, speed);

    return () => clearTimeout(timeout);
  }, [currentIndex, code, speed]);

  return (
    <div className="relative">
      <Highlight theme={theme} code={displayedCode || " "} language={language}>
        {({ className, style, tokens, getLineProps, getTokenProps }) => (
          <pre
            className={cn(
              className,
              "!bg-transparent font-mono text-sm leading-relaxed",
            )}
            style={{ ...style, background: "transparent" }}
          >
            {tokens.map((line, i) => {
              const lineNumber = i + startingLineNumber;
              const isHighlighted = highlightLines.includes(lineNumber);
              return (
                <div
                  key={i}
                  {...getLineProps({ line })}
                  className={cn(
                    "flex",
                    isHighlighted &&
                      "-mx-4 border-primary border-l-2 bg-primary/10 px-4",
                  )}
                >
                  {showLineNumbers && (
                    <span className="mr-4 inline-block w-8 shrink-0 select-none text-right text-muted-foreground/50">
                      {lineNumber}
                    </span>
                  )}
                  <span className="flex-1">
                    {line.map((token, key) => (
                      <span key={key} {...getTokenProps({ token })} />
                    ))}
                  </span>
                </div>
              );
            })}
          </pre>
        )}
      </Highlight>
      {currentIndex < code.length && (
        <motion.span
          className="absolute inline-block h-4 w-2 bg-primary"
          animate={{ opacity: [1, 0] }}
          transition={{ duration: 0.5, repeat: Infinity }}
        />
      )}
    </div>
  );
};

// Main CodeBlock component
const CodeBlock = React.forwardRef<HTMLDivElement, CodeBlockProps>(
  (
    {
      code,
      language = "typescript",
      title,
      showLineNumbers = true,
      highlightLines = [],
      addedLines = [],
      removedLines = [],
      variant = "default",
      animation = "fadeIn",
      animationDelay = 0,
      className,
      copyable = true,
      downloadable = false,
      downloadFileName,
      maxHeight,
      theme = "oneDark",
      wrapLongLines = false,
      showLanguage = true,
      collapsible = false,
      defaultCollapsed = false,
      startingLineNumber = 1,
      caption,
    },
    ref,
  ) => {
    const [isCollapsed, setIsCollapsed] = React.useState(defaultCollapsed);
    const [isExpanded, setIsExpanded] = React.useState(false);
    const [wordWrap, setWordWrap] = React.useState(wrapLongLines);
    const trimmedCode = code.trim();
    const selectedTheme = themeMap[theme] || themes.oneDark;

    const variantStyles: Record<CodeBlockVariant, string> = {
      default: "bg-card border border-border shadow-sm",
      terminal: "bg-[#1a1b26] border border-border shadow-lg",
      minimal: "bg-muted/50",
      gradient:
        "bg-gradient-to-br from-card via-card to-primary/5 border border-border shadow-md",
      glass: "bg-card/80 backdrop-blur-xl border border-border/50 shadow-xl",
    };

    const headerStyles: Record<CodeBlockVariant, string> = {
      default: "border-b border-border bg-muted/50",
      terminal: "border-b border-border bg-[#16161e]",
      minimal: "border-b border-border/50",
      gradient: "border-b border-border bg-muted/30",
      glass: "border-b border-border/50 bg-muted/30 backdrop-blur-sm",
    };

    const containerAnimation = {
      fadeIn: {
        initial: { opacity: 0, y: 20 },
        animate: { opacity: 1, y: 0 },
        transition: { duration: 0.4, delay: animationDelay },
      },
      slideIn: {
        initial: { opacity: 0, x: -20 },
        animate: { opacity: 1, x: 0 },
        transition: { duration: 0.4, delay: animationDelay },
      },
      highlight: {
        initial: { opacity: 0, scale: 0.98 },
        animate: { opacity: 1, scale: 1 },
        transition: { duration: 0.3, delay: animationDelay },
      },
      none: {
        initial: {},
        animate: {},
        transition: {},
      },
      typewriter: {
        initial: { opacity: 0 },
        animate: { opacity: 1 },
        transition: { duration: 0.2 },
      },
    };

    const currentAnimation =
      containerAnimation[animation] || containerAnimation.fadeIn;

    return (
      <motion.div
        ref={ref}
        className={cn(
          "overflow-hidden rounded-lg",
          variantStyles[variant],
          isExpanded && "fixed inset-4 z-50",
          className,
        )}
        initial={currentAnimation.initial}
        animate={currentAnimation.animate}
        transition={currentAnimation.transition}
      >
        {/* Header */}
        <div
          className={cn(
            "flex items-center justify-between px-4 py-2",
            headerStyles[variant],
          )}
        >
          <div className="flex items-center gap-3">
            {/* Window controls */}
            <div className="flex gap-1.5">
              <div className="h-3 w-3 rounded-full bg-red-500/80 transition-colors hover:bg-red-500" />
              <div className="h-3 w-3 rounded-full bg-yellow-500/80 transition-colors hover:bg-yellow-500" />
              <div className="h-3 w-3 rounded-full bg-green-500/80 transition-colors hover:bg-green-500" />
            </div>

            {/* Title or language */}
            <div className="flex items-center gap-2 text-muted-foreground text-sm">
              {variant === "terminal" ? (
                <Terminal className="h-4 w-4" />
              ) : (
                <FileCode className="h-4 w-4" />
              )}
              <span className="font-medium">
                {title || (showLanguage && getLanguageDisplayName(language))}
              </span>
            </div>
          </div>

          {/* Action buttons */}
          <div className="flex items-center gap-1">
            {/* Word wrap toggle */}
            <motion.button
              onClick={() => setWordWrap(!wordWrap)}
              className={cn(
                "rounded-md p-2 transition-colors hover:bg-muted",
                wordWrap
                  ? "text-primary"
                  : "text-muted-foreground hover:text-foreground",
              )}
              whileHover={{ scale: 1.05 }}
              whileTap={{ scale: 0.95 }}
              aria-label="Toggle word wrap"
              title="Toggle word wrap"
            >
              <WrapText className="h-4 w-4" />
            </motion.button>

            {/* Expand toggle */}
            <motion.button
              onClick={() => setIsExpanded(!isExpanded)}
              className="rounded-md p-2 text-muted-foreground transition-colors hover:bg-muted hover:text-foreground"
              whileHover={{ scale: 1.05 }}
              whileTap={{ scale: 0.95 }}
              aria-label={isExpanded ? "Minimize" : "Maximize"}
              title={isExpanded ? "Minimize" : "Maximize"}
            >
              {isExpanded ? (
                <Minimize2 className="h-4 w-4" />
              ) : (
                <Maximize2 className="h-4 w-4" />
              )}
            </motion.button>

            {/* Download button */}
            {downloadable && (
              <DownloadButton
                code={trimmedCode}
                fileName={downloadFileName}
                language={language}
              />
            )}

            {/* Copy button */}
            {copyable && <CopyButton code={trimmedCode} />}

            {/* Collapse toggle */}
            {collapsible && (
              <motion.button
                onClick={() => setIsCollapsed(!isCollapsed)}
                className="rounded-md px-2 py-1 text-muted-foreground text-xs transition-colors hover:bg-muted hover:text-foreground"
                whileHover={{ scale: 1.02 }}
                whileTap={{ scale: 0.98 }}
              >
                {isCollapsed ? "Expand" : "Collapse"}
              </motion.button>
            )}
          </div>
        </div>

        {/* Code content */}
        <AnimatePresence>
          {!isCollapsed && (
            <motion.div
              initial={{ height: 0, opacity: 0 }}
              animate={{ height: "auto", opacity: 1 }}
              exit={{ height: 0, opacity: 0 }}
              transition={{ duration: 0.2 }}
              className={cn(
                "overflow-auto p-4",
                wordWrap && "whitespace-pre-wrap break-words",
              )}
              style={maxHeight && !isExpanded ? { maxHeight } : undefined}
            >
              {animation === "typewriter" ? (
                <TypewriterCode
                  code={trimmedCode}
                  language={language as Language}
                  showLineNumbers={showLineNumbers}
                  highlightLines={highlightLines}
                  startingLineNumber={startingLineNumber}
                  theme={selectedTheme}
                />
              ) : (
                <Highlight
                  theme={selectedTheme}
                  code={trimmedCode}
                  language={language as Language}
                >
                  {({
                    className: preClassName,
                    style,
                    tokens,
                    getLineProps,
                    getTokenProps,
                  }) => (
                    <pre
                      className={cn(
                        preClassName,
                        "!bg-transparent font-mono text-sm leading-relaxed",
                      )}
                      style={{ ...style, background: "transparent" }}
                    >
                      {tokens.map((line, i) => {
                        const lineNumber = i + startingLineNumber;
                        const isHighlighted =
                          highlightLines.includes(lineNumber);
                        const isAdded = addedLines.includes(lineNumber);
                        const isRemoved = removedLines.includes(lineNumber);

                        return (
                          <motion.div
                            key={i}
                            {...getLineProps({ line })}
                            className={cn(
                              "flex",
                              isHighlighted &&
                                "-mx-4 border-primary border-l-2 bg-primary/10 px-4",
                              isAdded &&
                                "-mx-4 border-green-500 border-l-2 bg-green-500/10 px-4",
                              isRemoved &&
                                "-mx-4 border-red-500 border-l-2 bg-red-500/10 px-4 line-through opacity-60",
                            )}
                            initial={
                              animation === "slideIn"
                                ? { opacity: 0, x: -10 }
                                : animation === "highlight"
                                  ? {
                                      backgroundColor:
                                        "hsl(var(--primary) / 0.2)",
                                    }
                                  : {}
                            }
                            animate={
                              animation === "slideIn"
                                ? { opacity: 1, x: 0 }
                                : animation === "highlight"
                                  ? { backgroundColor: "transparent" }
                                  : {}
                            }
                            transition={{
                              duration: 0.3,
                              delay: animationDelay + i * 0.03,
                            }}
                          >
                            {showLineNumbers && (
                              <span className="mr-4 inline-block w-8 shrink-0 select-none text-right text-muted-foreground/50">
                                {isAdded && (
                                  <span className="mr-1 text-green-500">+</span>
                                )}
                                {isRemoved && (
                                  <span className="mr-1 text-red-500">-</span>
                                )}
                                {lineNumber}
                              </span>
                            )}
                            <span
                              className={cn("flex-1", wordWrap && "break-all")}
                            >
                              {line.map((token, key) => (
                                <span key={key} {...getTokenProps({ token })} />
                              ))}
                            </span>
                          </motion.div>
                        );
                      })}
                    </pre>
                  )}
                </Highlight>
              )}
            </motion.div>
          )}
        </AnimatePresence>

        {/* Caption */}
        {caption && (
          <div className="border-border border-t px-4 py-2 text-muted-foreground text-xs">
            {caption}
          </div>
        )}
      </motion.div>
    );
  },
);

CodeBlock.displayName = "CodeBlock";

// Inline code component
interface InlineCodeProps {
  children: string;
  className?: string;
  variant?: "default" | "primary" | "success" | "warning" | "error";
}

const InlineCode = React.forwardRef<HTMLSpanElement, InlineCodeProps>(
  ({ children, className, variant = "default" }, ref) => {
    const variantStyles: Record<string, string> = {
      default: "bg-muted text-foreground",
      primary: "bg-primary/10 text-primary",
      success: "bg-green-500/10 text-green-600 dark:text-green-400",
      warning: "bg-yellow-500/10 text-yellow-600 dark:text-yellow-400",
      error: "bg-red-500/10 text-red-600 dark:text-red-400",
    };

    return (
      <code
        ref={ref}
        className={cn(
          "rounded-md px-1.5 py-0.5 font-mono text-sm",
          variantStyles[variant],
          className,
        )}
      >
        {children}
      </code>
    );
  },
);

InlineCode.displayName = "InlineCode";

// Code comparison component
interface CodeCompareProps {
  before: string;
  after: string;
  language?: string;
  beforeTitle?: string;
  afterTitle?: string;
  className?: string;
  theme?: ThemeType;
  showDiff?: boolean;
}

const CodeCompare = React.forwardRef<HTMLDivElement, CodeCompareProps>(
  (
    {
      before,
      after,
      language = "typescript",
      beforeTitle = "Before",
      afterTitle = "After",
      className,
      theme = "oneDark",
      showDiff = false,
    },
    ref,
  ) => {
    // Simple diff calculation for line additions/removals
    const beforeLines = before.trim().split("\n");
    const afterLines = after.trim().split("\n");

    const removedLines = showDiff
      ? beforeLines
          .map((_, i) => i + 1)
          .filter(
            (_, i) => beforeLines[i] && !afterLines.includes(beforeLines[i]),
          )
      : [];
    const addedLines = showDiff
      ? afterLines
          .map((_, i) => i + 1)
          .filter(
            (_, i) => afterLines[i] && !beforeLines.includes(afterLines[i]),
          )
      : [];

    return (
      <div ref={ref} className={cn("grid gap-4 md:grid-cols-2", className)}>
        <CodeBlock
          code={before}
          language={language}
          title={beforeTitle}
          variant="default"
          animation="slideIn"
          theme={theme}
          removedLines={removedLines}
        />
        <CodeBlock
          code={after}
          language={language}
          title={afterTitle}
          variant="gradient"
          animation="slideIn"
          animationDelay={0.2}
          theme={theme}
          addedLines={addedLines}
        />
      </div>
    );
  },
);

CodeCompare.displayName = "CodeCompare";

// Animated code tabs
interface CodeTabsProps {
  tabs: Array<{
    label: string;
    code: string;
    language?: string;
    icon?: React.ReactNode;
  }>;
  className?: string;
  theme?: ThemeType;
  defaultTab?: number;
}

const CodeTabs = React.forwardRef<HTMLDivElement, CodeTabsProps>(
  ({ tabs, className, theme = "oneDark", defaultTab = 0 }, ref) => {
    const [activeTab, setActiveTab] = React.useState(defaultTab);

    return (
      <div
        ref={ref}
        className={cn(
          "overflow-hidden rounded-lg border border-border bg-card shadow-sm",
          className,
        )}
      >
        {/* Tab headers */}
        <div className="flex overflow-x-auto border-border border-b bg-muted/50">
          {tabs.map((tab, index) => (
            <motion.button
              key={index}
              onClick={() => setActiveTab(index)}
              className={cn(
                "flex items-center gap-2 whitespace-nowrap px-4 py-2.5 font-medium text-sm transition-colors",
                activeTab === index
                  ? "border-primary border-b-2 bg-background/50 text-primary"
                  : "text-muted-foreground hover:bg-muted/50 hover:text-foreground",
              )}
              whileHover={{
                backgroundColor:
                  activeTab === index ? undefined : "hsl(var(--muted) / 0.5)",
              }}
              whileTap={{ scale: 0.98 }}
            >
              {tab.icon}
              {tab.label}
            </motion.button>
          ))}
        </div>

        {/* Tab content */}
        <AnimatePresence mode="wait">
          <motion.div
            key={activeTab}
            initial={{ opacity: 0, y: 10 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -10 }}
            transition={{ duration: 0.2 }}
          >
            <CodeBlock
              code={tabs[activeTab]?.code || ""}
              language={tabs[activeTab]?.language || "typescript"}
              showLineNumbers={true}
              variant="minimal"
              animation="none"
              copyable={true}
              className="rounded-none border-0"
              theme={theme}
            />
          </motion.div>
        </AnimatePresence>
      </div>
    );
  },
);

CodeTabs.displayName = "CodeTabs";

// Terminal/Command component
interface TerminalBlockProps {
  commands: Array<{
    command: string;
    output?: string;
  }>;
  title?: string;
  className?: string;
  animated?: boolean;
}

const TerminalBlock = React.forwardRef<HTMLDivElement, TerminalBlockProps>(
  ({ commands, title = "Terminal", className, animated = true }, ref) => {
    return (
      <motion.div
        ref={ref}
        className={cn(
          "overflow-hidden rounded-lg border border-border bg-[#1a1b26] shadow-lg",
          className,
        )}
        initial={animated ? { opacity: 0, y: 20 } : {}}
        animate={animated ? { opacity: 1, y: 0 } : {}}
        transition={{ duration: 0.4 }}
      >
        {/* Header */}
        <div className="flex items-center gap-3 border-border border-b bg-[#16161e] px-4 py-2">
          <div className="flex gap-1.5">
            <div className="h-3 w-3 rounded-full bg-red-500/80" />
            <div className="h-3 w-3 rounded-full bg-yellow-500/80" />
            <div className="h-3 w-3 rounded-full bg-green-500/80" />
          </div>
          <div className="flex items-center gap-2 text-muted-foreground text-sm">
            <Terminal className="h-4 w-4" />
            <span>{title}</span>
          </div>
        </div>

        {/* Commands */}
        <div className="p-4 font-mono text-sm">
          {commands.map((item, index) => (
            <motion.div
              key={index}
              initial={animated ? { opacity: 0, x: -10 } : {}}
              animate={animated ? { opacity: 1, x: 0 } : {}}
              transition={{ delay: index * 0.1 }}
              className="mb-2 last:mb-0"
            >
              <div className="flex items-center gap-2">
                <span className="text-green-400">$</span>
                <span className="text-foreground">{item.command}</span>
              </div>
              {item.output && (
                <div className="mt-1 whitespace-pre-wrap pl-4 text-muted-foreground">
                  {item.output}
                </div>
              )}
            </motion.div>
          ))}
        </div>
      </motion.div>
    );
  },
);

TerminalBlock.displayName = "TerminalBlock";

export {
  CodeBlock,
  CodeCompare,
  CodeTabs,
  InlineCode,
  supportedLanguages,
  TerminalBlock,
  themeMap,
};
export type {
  AnimationType,
  CodeBlockProps,
  CodeBlockVariant,
  CodeCompareProps,
  CodeTabsProps,
  InlineCodeProps,
  TerminalBlockProps,
  ThemeType,
};
```
---

## Command Palette <a name="command-palette"></a>

VS Code-style command palette for React. Fuzzy search, keyboard navigation, custom shortcuts, and nested commands. ⌘K ready component.

- **Registry Key**: `command-palette`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/command-palette.json
```

#### Dependencies
- **NPM Packages**:
  - `lucide-react`
  - `motion`

#### How to Use
```tsx
"use client";

import {
  Calculator,
  Calendar,
  CreditCard,
  Settings,
  Smile,
  User,
} from "lucide-react";
import { useState } from "react";
import { Button } from "@/components/ui/button";
import { CommandPalette } from "@/registry/default/ui/command-palette";

export default function CommandPaletteDemo() {
  const [open, setOpen] = useState(false);

  const groups = [
    {
      id: "suggestions",
      heading: "Suggestions",
      items: [
        {
          id: "calendar",
          label: "Calendar",
          icon: Calendar,
          onSelect: () => console.log("Calendar selected"),
        },
        {
          id: "search-emoji",
          label: "Search Emoji",
          icon: Smile,
          onSelect: () => console.log("Search Emoji selected"),
        },
        {
          id: "calculator",
          label: "Calculator",
          icon: Calculator,
          onSelect: () => console.log("Calculator selected"),
        },
      ],
    },
    {
      id: "settings",
      heading: "Settings",
      items: [
        {
          id: "profile",
          label: "Profile",
          icon: User,
          shortcut: ["⌘", "P"],
          onSelect: () => console.log("Profile selected"),
        },
        {
          id: "billing",
          label: "Billing",
          icon: CreditCard,
          shortcut: ["⌘", "B"],
          onSelect: () => console.log("Billing selected"),
        },
        {
          id: "settings",
          label: "Settings",
          icon: Settings,
          shortcut: ["⌘", "S"],
          onSelect: () => console.log("Settings selected"),
        },
      ],
    },
  ];

  return (
    <div className="flex min-h-[400px] flex-col items-center justify-center gap-4">
      <p className="text-muted-foreground text-sm">
        Press{" "}
        <kbd className="rounded bg-muted px-1.5 py-0.5 font-mono text-xs">
          ⌘K
        </kbd>{" "}
        to open the command palette
      </p>
      <Button onClick={() => setOpen(true)}>Open Command Palette</Button>

      <CommandPalette open={open} onOpenChange={setOpen} groups={groups} />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/command-palette.tsx`
```tsx
"use client";

import {
  ChevronRight,
  Clock,
  CornerDownLeft,
  Loader2,
  type LucideIcon,
  Search,
  X,
} from "lucide-react";
import { AnimatePresence, LayoutGroup, motion } from "motion/react";
import type React from "react";
import { useCallback, useEffect, useMemo, useRef, useState } from "react";
import { cn } from "@/lib/utils";

export interface CommandItem {
  id: string;
  label: string;
  description?: string;
  icon?: LucideIcon;
  shortcut?: string[];
  keywords?: string[];
  onSelect?: () => void;
  children?: CommandGroup[]; // Support for sub-menus
  disabled?: boolean;
}

export interface CommandGroup {
  id: string;
  heading: string;
  items: CommandItem[];
}

export interface CommandPaletteProps {
  open?: boolean;
  onOpenChange?: (open: boolean) => void;
  groups: CommandGroup[];
  placeholder?: string;
  emptyMessage?: string;
  shortcut?: string[];
  loading?: boolean;
  showRecent?: boolean;
  maxRecent?: number;
}

interface FlattenedItem {
  type: "group" | "item";
  groupId: string;
  groupHeading?: string;
  item?: CommandItem;
  score?: number;
  matches?: [number, number][];
}

interface PageState {
  id: string;
  title: string;
  groups: CommandGroup[];
}

export function CommandPalette({
  open: controlledOpen,
  onOpenChange,
  groups: initialGroups,
  placeholder = "Type a command or search...",
  emptyMessage = "No results found.",
  shortcut = ["⌘", "K"],
  loading = false,
  showRecent = true,
  maxRecent = 5,
}: CommandPaletteProps) {
  const [internalOpen, setInternalOpen] = useState(false);
  const [query, setQuery] = useState("");
  const [selectedIndex, setSelectedIndex] = useState(0);
  const [pages, setPages] = useState<PageState[]>([
    { id: "root", title: "Root", groups: initialGroups },
  ]);
  const [recentItems, setRecentItems] = useState<CommandItem[]>([]);

  const inputRef = useRef<HTMLInputElement>(null);
  const listRef = useRef<HTMLDivElement>(null);
  const itemRefs = useRef<Map<number, HTMLDivElement>>(new Map());

  const isOpen = controlledOpen ?? internalOpen;
  const setOpen = onOpenChange ?? setInternalOpen;
  const currentPage = pages[pages.length - 1] || pages[0];

  // Load recent items from localStorage
  useEffect(() => {
    if (typeof window !== "undefined") {
      const saved = localStorage.getItem("jolyui-command-recent");
      if (saved) {
        try {
          setRecentItems(JSON.parse(saved));
        } catch (e) {
          console.error("Failed to load recent items", e);
        }
      }
    }
  }, []);

  const saveRecent = useCallback(
    (item: CommandItem) => {
      setRecentItems((prev) => {
        const filtered = prev.filter((i) => i.id !== item.id);
        const updated = [item, ...filtered].slice(0, maxRecent);
        localStorage.setItem("jolyui-command-recent", JSON.stringify(updated));
        return updated;
      });
    },
    [maxRecent],
  );

  // Flatten and filter items based on search query
  const flattenedItems = useMemo(() => {
    const items: FlattenedItem[] = [];
    const currentGroups = currentPage?.groups || [];

    // Add Recent group if on root page and query is empty
    if (
      pages.length === 1 &&
      query === "" &&
      showRecent &&
      recentItems.length > 0
    ) {
      items.push({ type: "group", groupId: "recent", groupHeading: "Recent" });
      recentItems.forEach((item) => {
        items.push({ type: "item", groupId: "recent", item });
      });
    }

    currentGroups.forEach((group) => {
      const matchedItems: FlattenedItem[] = [];

      group.items.forEach((item) => {
        if (item.disabled) return;

        const searchText = [item.label, ...(item.keywords || [])].join(" ");
        const match = fuzzySearch(query || "", searchText);

        if (match) {
          const labelMatch = fuzzySearch(query || "", item.label);
          matchedItems.push({
            type: "item",
            groupId: group.id,
            item,
            score: match.score,
            matches: labelMatch?.matches || [],
          });
        }
      });

      matchedItems.sort((a, b) => (b.score || 0) - (a.score || 0));

      if (matchedItems.length > 0) {
        items.push({
          type: "group",
          groupId: group.id,
          groupHeading: group.heading,
        });
        items.push(...matchedItems);
      }
    });

    return items;
  }, [currentPage?.groups, query, pages.length, showRecent, recentItems]);

  const selectableItems = useMemo(
    () => flattenedItems.filter((item) => item.type === "item"),
    [flattenedItems],
  );

  // Reset selection when query or page changes
  useEffect(() => {
    setSelectedIndex(0);
  }, []);

  // Keyboard shortcut to open (only in uncontrolled mode)
  useEffect(() => {
    if (controlledOpen !== undefined) return;

    const handleKeyDown = (e: KeyboardEvent) => {
      if ((e.metaKey || e.ctrlKey) && e.key === "k") {
        e.preventDefault();
        setOpen(!isOpen);
      }
    };

    document.addEventListener("keydown", handleKeyDown);
    return () => document.removeEventListener("keydown", handleKeyDown);
  }, [isOpen, setOpen, controlledOpen]);

  // Focus input when opened
  useEffect(() => {
    if (isOpen) {
      setQuery("");
      setSelectedIndex(0);
      setPages([{ id: "root", title: "Root", groups: initialGroups }]);
      setTimeout(() => inputRef.current?.focus(), 0);
    }
  }, [isOpen, initialGroups]);

  // Scroll selected item into view
  useEffect(() => {
    const selectedItem = itemRefs.current.get(selectedIndex);
    if (selectedItem && listRef.current) {
      selectedItem.scrollIntoView({ block: "nearest" });
    }
  }, [selectedIndex]);

  const handleSelect = useCallback(
    (item: CommandItem) => {
      if (item.children) {
        setPages((prev) => [
          ...prev,
          {
            id: item.id,
            title: item.label,
            groups: item.children || [],
          },
        ]);
        setQuery("");
        return;
      }

      if (item.onSelect) {
        item.onSelect();
        saveRecent(item);
        setOpen(false);
      }
    },
    [saveRecent, setOpen],
  );

  const handleBack = useCallback(() => {
    if (pages.length > 1) {
      setPages((prev) => prev.slice(0, -1));
      setQuery("");
    }
  }, [pages.length]);

  const handleKeyDown = useCallback(
    (e: React.KeyboardEvent) => {
      switch (e.key) {
        case "ArrowDown":
          e.preventDefault();
          setSelectedIndex((i) => (i < selectableItems.length - 1 ? i + 1 : 0));
          break;
        case "ArrowUp":
          e.preventDefault();
          setSelectedIndex((i) => (i > 0 ? i - 1 : selectableItems.length - 1));
          break;
        case "Enter": {
          e.preventDefault();
          const selected = selectableItems[selectedIndex]?.item;
          if (selected) {
            handleSelect(selected);
          }
          break;
        }
        case "Backspace":
          if (query === "" && pages.length > 1) {
            e.preventDefault();
            handleBack();
          }
          break;
        case "Escape":
          e.preventDefault();
          if (pages.length > 1) {
            handleBack();
          } else {
            setOpen(false);
          }
          break;
      }
    },
    [
      selectableItems,
      selectedIndex,
      setOpen,
      handleSelect,
      handleBack,
      query,
      pages.length,
    ],
  );

  return (
    <AnimatePresence>
      {isOpen && (
        <>
          <motion.div
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            className="fixed inset-0 z-50 bg-background/80 backdrop-blur-sm"
            onClick={() => setOpen(false)}
            aria-hidden="true"
          />

          <div
            role="dialog"
            aria-modal="true"
            className="fixed top-[20%] left-1/2 z-50 w-full max-w-xl -translate-x-1/2"
          >
            <motion.div
              initial={{ opacity: 0, scale: 0.95, y: 10 }}
              animate={{ opacity: 1, scale: 1, y: 0 }}
              exit={{ opacity: 0, scale: 0.95, y: 10 }}
              transition={{ duration: 0.2, ease: "easeOut" }}
              className="flex flex-col overflow-hidden rounded-xl border border-border bg-card shadow-2xl"
            >
              {/* Header / Search */}
              <div className="relative flex items-center gap-3 border-border border-b px-4">
                {pages.length > 1 ? (
                  <button
                    onClick={handleBack}
                    className="rounded-md p-1 transition-colors hover:bg-muted"
                  >
                    <X className="h-4 w-4 rotate-90 text-muted-foreground" />
                  </button>
                ) : (
                  <Search className="h-4 w-4 shrink-0 text-muted-foreground" />
                )}

                <div className="flex min-w-0 flex-1 items-center">
                  {pages.length > 1 && (
                    <div className="mr-2 flex shrink-0 items-center gap-1">
                      <span className="rounded bg-primary/10 px-1.5 py-0.5 font-medium text-primary text-xs">
                        {currentPage?.title || ""}
                      </span>
                      <span className="text-muted-foreground">/</span>
                    </div>
                  )}
                  <input
                    ref={inputRef}
                    type="text"
                    value={query}
                    onChange={(e) => setQuery(e.target.value)}
                    onKeyDown={handleKeyDown}
                    placeholder={
                      pages.length > 1
                        ? `Search in ${currentPage?.title || ""}...`
                        : placeholder
                    }
                    className="h-12 flex-1 bg-transparent text-foreground text-sm outline-none placeholder:text-muted-foreground"
                  />
                </div>

                {loading && (
                  <Loader2 className="h-4 w-4 animate-spin text-muted-foreground" />
                )}

                <div className="ml-2 flex items-center gap-1">
                  {shortcut.map((key, i) => (
                    <kbd
                      key={i}
                      className="hidden h-5 min-w-[20px] items-center justify-center rounded bg-muted px-1.5 font-mono text-[10px] text-muted-foreground sm:flex"
                    >
                      {key}
                    </kbd>
                  ))}
                </div>
              </div>

              {/* Results */}
              <div
                ref={listRef}
                className="max-h-[380px] overflow-y-auto scroll-smooth py-2"
              >
                <LayoutGroup id="command-list">
                  {flattenedItems.length === 0 ? (
                    <motion.div
                      initial={{ opacity: 0 }}
                      animate={{ opacity: 1 }}
                      className="space-y-2 py-12 text-center"
                    >
                      <Search className="mx-auto h-8 w-8 text-muted-foreground opacity-20" />
                      <p className="text-muted-foreground text-sm">
                        {emptyMessage}
                      </p>
                    </motion.div>
                  ) : (
                    flattenedItems.map((flatItem) => {
                      if (flatItem.type === "group") {
                        return (
                          <div
                            key={`group-${flatItem.groupId}`}
                            className="mt-2 px-3 py-2 font-bold text-[10px] text-muted-foreground/70 uppercase tracking-widest first:mt-0"
                          >
                            {flatItem.groupHeading}
                          </div>
                        );
                      }

                      const currentItemIndex = selectableItems.findIndex(
                        (si) => si.item?.id === flatItem.item?.id,
                      );
                      const isSelected = currentItemIndex === selectedIndex;
                      const item = flatItem.item;
                      if (!item) return null;
                      const Icon = item.icon;
                      const highlightedLabel = highlightMatches(
                        item.label,
                        flatItem.matches || [],
                      );

                      return (
                        <motion.div
                          layout
                          key={item.id}
                          ref={(el: HTMLDivElement | null) => {
                            if (el) itemRefs.current.set(currentItemIndex, el);
                          }}
                          className={cn(
                            "group relative mx-2 flex cursor-pointer items-center gap-3 rounded-lg px-3 py-2.5 transition-all duration-150",
                            isSelected &&
                              "bg-accent text-accent-foreground shadow-sm",
                          )}
                          onClick={() => handleSelect(item)}
                          onMouseEnter={() =>
                            setSelectedIndex(currentItemIndex)
                          }
                        >
                          {isSelected && (
                            <motion.div
                              layoutId="active-pill"
                              className="absolute inset-0 -z-10 rounded-lg bg-accent"
                              transition={{
                                type: "spring",
                                bounce: 0.2,
                                duration: 0.4,
                              }}
                            />
                          )}

                          <div
                            className={cn(
                              "flex h-8 w-8 items-center justify-center rounded-md border transition-colors",
                              isSelected
                                ? "border-primary/20 bg-background"
                                : "border-transparent bg-muted/50",
                            )}
                          >
                            {flatItem.groupId === "recent" ? (
                              <Clock className="h-4 w-4 text-muted-foreground" />
                            ) : Icon ? (
                              <Icon className="h-4 w-4 shrink-0 text-muted-foreground" />
                            ) : (
                              <div className="h-1 w-1 rounded-full bg-muted-foreground" />
                            )}
                          </div>

                          <div className="min-w-0 flex-1">
                            <div className="truncate font-medium text-foreground text-sm">
                              {highlightedLabel.map((part, i) => (
                                <span
                                  key={i}
                                  className={
                                    part.highlighted
                                      ? "font-semibold text-primary"
                                      : undefined
                                  }
                                >
                                  {part.text}
                                </span>
                              ))}
                            </div>
                            {item.description && (
                              <div className="mt-0.5 truncate text-muted-foreground text-xs">
                                {item.description}
                              </div>
                            )}
                          </div>

                          <div className="flex shrink-0 items-center gap-2">
                            {item.children && (
                              <ChevronRight className="h-4 w-4 text-muted-foreground opacity-50" />
                            )}
                            {item.shortcut && !item.children && (
                              <div className="flex items-center gap-1">
                                {item.shortcut.map((key, i) => (
                                  <kbd
                                    key={i}
                                    className="flex h-4 min-w-[18px] items-center justify-center rounded border border-border/50 bg-muted px-1 font-mono text-[9px] text-muted-foreground"
                                  >
                                    {key}
                                  </kbd>
                                ))}
                              </div>
                            )}
                          </div>
                        </motion.div>
                      );
                    })
                  )}
                </LayoutGroup>
              </div>

              {/* Footer */}
              <div className="flex items-center justify-between border-border border-t bg-muted/20 px-4 py-3 text-[10px] text-muted-foreground">
                <div className="flex items-center gap-4">
                  <span className="flex items-center gap-1.5">
                    <kbd className="rounded border border-border/50 bg-muted px-1 py-0.5 font-mono">
                      ↑↓
                    </kbd>
                    navigate
                  </span>
                  <span className="flex items-center gap-1.5">
                    <kbd className="rounded border border-border/50 bg-muted px-1 py-0.5 font-mono">
                      <CornerDownLeft className="h-2 w-2" />
                    </kbd>
                    select
                  </span>
                  {pages.length > 1 && (
                    <span className="flex items-center gap-1.5">
                      <kbd className="rounded border border-border/50 bg-muted px-1 py-0.5 font-mono">
                        esc
                      </kbd>
                      back
                    </span>
                  )}
                </div>
                <div className="flex items-center gap-2">
                  <span className="opacity-50">JolyUI Command</span>
                </div>
              </div>
            </motion.div>
          </div>
        </>
      )}
    </AnimatePresence>
  );
}

export interface FuzzyMatch {
  item: string;
  score: number;
  matches: [number, number][];
}

export function fuzzySearch(query: string, text: string): FuzzyMatch | null {
  if (!query) return { item: text, score: 1, matches: [] };

  const queryLower = query.toLowerCase();
  const textLower = text.toLowerCase();

  let queryIndex = 0;
  let score = 0;
  const matches: [number, number][] = [];
  let currentMatchStart = -1;
  let consecutiveMatches = 0;

  for (let i = 0; i < text.length && queryIndex < query.length; i++) {
    if (textLower[i] === queryLower[queryIndex]) {
      if (currentMatchStart === -1) currentMatchStart = i;
      consecutiveMatches++;
      queryIndex++;
      score += 1 + consecutiveMatches * 0.5;
      if (i === 0) score += 2;
      const prevChar = i > 0 ? text[i - 1] : "";
      if (prevChar && /[\s\-_]/.test(prevChar)) score += 1.5;
    } else {
      if (currentMatchStart !== -1) {
        matches.push([currentMatchStart, i]);
        currentMatchStart = -1;
        consecutiveMatches = 0;
      }
    }
  }

  if (currentMatchStart !== -1) matches.push([currentMatchStart, text.length]);
  if (queryIndex < query.length) return null;

  return { item: text, score: score / query.length, matches };
}

export function highlightMatches(
  text: string,
  matches: [number, number][],
): { text: string; highlighted: boolean }[] {
  if (matches.length === 0) return [{ text, highlighted: false }];

  const result: { text: string; highlighted: boolean }[] = [];
  let lastIndex = 0;

  for (const [start, end] of matches) {
    if (start > lastIndex) {
      result.push({ text: text.slice(lastIndex, start), highlighted: false });
    }
    result.push({ text: text.slice(start, end), highlighted: true });
    lastIndex = end;
  }

  if (lastIndex < text.length) {
    result.push({ text: text.slice(lastIndex), highlighted: false });
  }

  return result;
}
```
---

## Date Wheel Picker <a name="date-wheel-picker"></a>

iOS-style date wheel picker for React. 3D rotation effect with drag gestures and keyboard support. Mobile-friendly date selection component.

- **Registry Key**: `date-wheel-picker`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/date-wheel-picker.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
"use client";

import { useState } from "react";
import { DateWheelPicker } from "@/registry/default/ui/date-wheel-picker";

export default function DateWheelPickerDemo() {
  const [date, setDate] = useState<Date>(new Date());

  return (
    <div className="relative flex h-[300px] w-full flex-col items-center justify-center gap-4">
      <DateWheelPicker value={date} onChange={setDate} />
      <p className="text-muted-foreground text-sm">
        Selected: {date.toLocaleDateString()}
      </p>
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/date-wheel-picker.tsx`
```tsx
"use client";

import {
  animate,
  type MotionValue,
  motion,
  type PanInfo,
  useMotionValue,
  useTransform,
} from "framer-motion";
import * as React from "react";
import { cn } from "@/lib/utils";

export interface DateWheelPickerProps
  extends Omit<React.HTMLAttributes<HTMLDivElement>, "onChange"> {
  value?: Date;
  onChange: (date: Date) => void;
  minYear?: number;
  maxYear?: number;
  size?: "sm" | "md" | "lg";
  disabled?: boolean;
  locale?: string;
}

const ITEM_HEIGHT = 40;
const VISIBLE_ITEMS = 5;
const PERSPECTIVE_ORIGIN = ITEM_HEIGHT * 2;

function getMonthNames(locale?: string): string[] {
  const formatter = new Intl.DateTimeFormat(locale, { month: "long" });
  return Array.from({ length: 12 }, (_, i) =>
    formatter.format(new Date(2000, i, 1)),
  );
}

const sizeConfig = {
  sm: {
    height: ITEM_HEIGHT * VISIBLE_ITEMS * 0.8,
    itemHeight: ITEM_HEIGHT * 0.8,
    fontSize: "text-sm",
    gap: "gap-2",
  },
  md: {
    height: ITEM_HEIGHT * VISIBLE_ITEMS,
    itemHeight: ITEM_HEIGHT,
    fontSize: "text-base",
    gap: "gap-4",
  },
  lg: {
    height: ITEM_HEIGHT * VISIBLE_ITEMS * 1.2,
    itemHeight: ITEM_HEIGHT * 1.2,
    fontSize: "text-lg",
    gap: "gap-6",
  },
};

interface WheelItemProps {
  item: string | number;
  index: number;
  y: MotionValue<number>;
  itemHeight: number;
  visibleItems: number;
  centerOffset: number;
  isSelected: boolean;
  disabled?: boolean;
  onClick: () => void;
}

function WheelItem({
  item,
  index,
  y,
  itemHeight,
  visibleItems,
  centerOffset,
  isSelected,
  disabled,
  onClick,
}: WheelItemProps) {
  const itemY = useTransform(y, (latest) => {
    const offset = index * itemHeight + latest + centerOffset;
    return offset;
  });

  const rotateX = useTransform(
    itemY,
    [0, centerOffset, itemHeight * visibleItems],
    [45, 0, -45],
  );

  const scale = useTransform(
    itemY,
    [0, centerOffset, itemHeight * visibleItems],
    [0.8, 1, 0.8],
  );

  const opacity = useTransform(
    itemY,
    [
      0,
      centerOffset * 0.5,
      centerOffset,
      centerOffset * 1.5,
      itemHeight * visibleItems,
    ],
    [0.3, 0.6, 1, 0.6, 0.3],
  );

  return (
    <motion.div
      className="flex select-none items-center justify-center"
      style={{
        height: itemHeight,
        rotateX,
        scale,
        opacity,
        transformStyle: "preserve-3d",
        transformOrigin: `center center -${PERSPECTIVE_ORIGIN}px`,
      }}
      onClick={() => !disabled && onClick()}
    >
      <span
        className={cn(
          "font-medium transition-colors",
          isSelected ? "text-foreground" : "text-muted-foreground",
        )}
      >
        {item}
      </span>
    </motion.div>
  );
}

interface WheelColumnProps {
  items: (string | number)[];
  value: number;
  onChange: (index: number) => void;
  itemHeight: number;
  visibleItems: number;
  disabled?: boolean;
  className?: string;
  ariaLabel: string;
}

function WheelColumn({
  items,
  value,
  onChange,
  itemHeight,
  visibleItems,
  disabled,
  className,
  ariaLabel,
}: WheelColumnProps) {
  const containerRef = React.useRef<HTMLDivElement>(null);
  const y = useMotionValue(-value * itemHeight);
  const centerOffset = Math.floor(visibleItems / 2) * itemHeight;

  const valueRef = React.useRef(value);
  const onChangeRef = React.useRef(onChange);
  const itemsLengthRef = React.useRef(items.length);

  React.useEffect(() => {
    valueRef.current = value;
    onChangeRef.current = onChange;
    itemsLengthRef.current = items.length;
  });

  React.useEffect(() => {
    animate(y, -value * itemHeight, {
      type: "spring",
      stiffness: 300,
      damping: 30,
    });
  }, [value, itemHeight, y]);

  const handleDragEnd = (
    _: MouseEvent | TouchEvent | PointerEvent,
    info: PanInfo,
  ) => {
    if (disabled) return;

    const currentY = y.get();
    const velocity = info.velocity.y;
    const projectedY = currentY + velocity * 0.2;

    let newIndex = Math.round(-projectedY / itemHeight);
    newIndex = Math.max(0, Math.min(items.length - 1, newIndex));

    onChange(newIndex);
  };

  React.useEffect(() => {
    const container = containerRef.current;
    if (!container || disabled) return;

    const handleWheel = (e: WheelEvent) => {
      e.preventDefault();
      e.stopPropagation();

      const direction = e.deltaY > 0 ? 1 : -1;
      const currentValue = valueRef.current;
      const maxIndex = itemsLengthRef.current - 1;
      const newIndex = Math.max(
        0,
        Math.min(maxIndex, currentValue + direction),
      );

      if (newIndex !== currentValue) {
        onChangeRef.current(newIndex);
      }
    };

    container.addEventListener("wheel", handleWheel, { passive: false });

    return () => {
      container.removeEventListener("wheel", handleWheel);
    };
  }, [disabled]);

  const handleKeyDown = (e: React.KeyboardEvent) => {
    if (disabled) return;

    const maxIndex = items.length - 1;
    let newIndex = value;

    switch (e.key) {
      case "ArrowUp":
        e.preventDefault();
        newIndex = Math.max(0, value - 1);
        break;
      case "ArrowDown":
        e.preventDefault();
        newIndex = Math.min(maxIndex, value + 1);
        break;
      case "Home":
        e.preventDefault();
        newIndex = 0;
        break;
      case "End":
        e.preventDefault();
        newIndex = maxIndex;
        break;
      case "PageUp":
        e.preventDefault();
        newIndex = Math.max(0, value - 5);
        break;
      case "PageDown":
        e.preventDefault();
        newIndex = Math.min(maxIndex, value + 5);
        break;
      default:
        return;
    }

    if (newIndex !== value) {
      onChange(newIndex);
    }
  };

  const dragConstraints = React.useMemo(
    () => ({
      top: -(items.length - 1) * itemHeight,
      bottom: 0,
    }),
    [items.length, itemHeight],
  );

  return (
    <div
      ref={containerRef}
      className={cn(
        "relative overflow-hidden",
        disabled && "pointer-events-none opacity-50",
        className,
      )}
      style={{ height: itemHeight * visibleItems }}
      tabIndex={disabled ? -1 : 0}
      onKeyDown={handleKeyDown}
      role="spinbutton"
      aria-label={ariaLabel}
      aria-valuenow={value}
      aria-valuemin={0}
      aria-valuemax={items.length - 1}
      aria-valuetext={String(items[value])}
      aria-disabled={disabled}
    >
      <div
        className="pointer-events-none absolute inset-x-0 top-0 z-10"
        style={{
          height: centerOffset,
          background:
            "linear-gradient(to bottom, var(--background) 0%, transparent 100%)",
        }}
        aria-hidden="true"
      />
      <div
        className="pointer-events-none absolute inset-x-0 bottom-0 z-10"
        style={{
          height: centerOffset,
          background:
            "linear-gradient(to top, var(--background) 0%, transparent 100%)",
        }}
        aria-hidden="true"
      />

      <div
        className="pointer-events-none absolute inset-x-0 z-5 border-border border-y bg-muted/30"
        style={{
          top: centerOffset,
          height: itemHeight,
        }}
        aria-hidden="true"
      />

      <motion.div
        className="cursor-grab active:cursor-grabbing"
        style={{
          y,
          paddingTop: centerOffset,
          paddingBottom: centerOffset,
        }}
        drag="y"
        dragConstraints={dragConstraints}
        dragElastic={0.1}
        onDragEnd={handleDragEnd}
      >
        {items.map((item, index) => (
          <WheelItem
            key={`${item}-${index}`}
            item={item}
            index={index}
            y={y}
            itemHeight={itemHeight}
            visibleItems={visibleItems}
            centerOffset={centerOffset}
            isSelected={index === value}
            disabled={disabled}
            onClick={() => onChange(index)}
          />
        ))}
      </motion.div>
    </div>
  );
}

function getDaysInMonth(year: number, month: number): number {
  return new Date(year, month + 1, 0).getDate();
}

const DateWheelPicker = React.forwardRef<HTMLDivElement, DateWheelPickerProps>(
  (
    {
      value,
      onChange,
      minYear = 1920,
      maxYear = new Date().getFullYear(),
      size = "md",
      disabled = false,
      locale,
      className,
      ...props
    },
    ref,
  ) => {
    const config = sizeConfig[size];

    const months = React.useMemo(() => getMonthNames(locale), [locale]);

    const years = React.useMemo(() => {
      const arr: number[] = [];
      for (let y = maxYear; y >= minYear; y--) {
        arr.push(y);
      }
      return arr;
    }, [minYear, maxYear]);

    const [dateState, setDateState] = React.useState(() => {
      const currentDate = value || new Date();
      return {
        day: currentDate.getDate(),
        month: currentDate.getMonth(),
        year: currentDate.getFullYear(),
      };
    });

    const isInternalChange = React.useRef(false);

    const days = React.useMemo(() => {
      const daysInMonth = getDaysInMonth(dateState.year, dateState.month);
      return Array.from({ length: daysInMonth }, (_, i) => i + 1);
    }, [dateState.month, dateState.year]);

    const handleDayChange = React.useCallback((dayIndex: number) => {
      isInternalChange.current = true;
      setDateState((prev) => ({ ...prev, day: dayIndex + 1 }));
    }, []);

    const handleMonthChange = React.useCallback((monthIndex: number) => {
      isInternalChange.current = true;
      setDateState((prev) => {
        const daysInNewMonth = getDaysInMonth(prev.year, monthIndex);
        const adjustedDay = Math.min(prev.day, daysInNewMonth);
        return { ...prev, month: monthIndex, day: adjustedDay };
      });
    }, []);

    const handleYearChange = React.useCallback(
      (yearIndex: number) => {
        isInternalChange.current = true;
        setDateState((prev) => {
          const newYear = years[yearIndex] ?? prev.year;
          const daysInNewMonth = getDaysInMonth(newYear, prev.month);
          const adjustedDay = Math.min(prev.day, daysInNewMonth);
          return { ...prev, year: newYear, day: adjustedDay };
        });
      },
      [years],
    );

    React.useEffect(() => {
      if (isInternalChange.current) {
        const newDate = new Date(
          dateState.year,
          dateState.month,
          dateState.day,
        );
        onChange(newDate);
        isInternalChange.current = false;
      }
    }, [dateState, onChange]);

    React.useEffect(() => {
      if (value && !isInternalChange.current) {
        const valueDay = value.getDate();
        const valueMonth = value.getMonth();
        const valueYear = value.getFullYear();

        if (
          valueDay !== dateState.day ||
          valueMonth !== dateState.month ||
          valueYear !== dateState.year
        ) {
          setDateState({
            day: valueDay,
            month: valueMonth,
            year: valueYear,
          });
        }
      }
    }, [value, dateState.day, dateState.month, dateState.year]);

    const yearIndex = years.indexOf(dateState.year);

    return (
      <div
        ref={ref}
        className={cn(
          "flex items-center justify-center",
          config.gap,
          config.fontSize,
          disabled && "pointer-events-none opacity-50",
          className,
        )}
        style={{ perspective: "1000px" }}
        role="group"
        aria-label="Date picker"
        {...props}
      >
        <WheelColumn
          items={days}
          value={dateState.day - 1}
          onChange={handleDayChange}
          itemHeight={config.itemHeight}
          visibleItems={VISIBLE_ITEMS}
          disabled={disabled}
          className="w-16"
          ariaLabel="Select day"
        />

        <WheelColumn
          items={months}
          value={dateState.month}
          onChange={handleMonthChange}
          itemHeight={config.itemHeight}
          visibleItems={VISIBLE_ITEMS}
          disabled={disabled}
          className="w-28"
          ariaLabel="Select month"
        />

        <WheelColumn
          items={years}
          value={yearIndex >= 0 ? yearIndex : 0}
          onChange={handleYearChange}
          itemHeight={config.itemHeight}
          visibleItems={VISIBLE_ITEMS}
          disabled={disabled}
          className="w-20"
          ariaLabel="Select year"
        />
      </div>
    );
  },
);

DateWheelPicker.displayName = "DateWheelPicker";

export { DateWheelPicker };
```
---

## Dock <a name="dock"></a>

macOS-style dock component for React. Icon magnification on hover with spring animations. Customizable size, gap, and magnification strength.

- **Registry Key**: `dock`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/dock.json
```

#### Dependencies
- **NPM Packages**:
  - `lucide-react`
  - `motion`

#### How to Use
```tsx
import { Calculator, Calendar, Compass, Mail, Music } from "lucide-react";
import {
  Dock,
  DockIcon,
  DockItem,
  DockLabel,
} from "@/registry/default/ui/dock";

export default function DockDemo() {
  return (
    <div className="flex h-32 items-end justify-center">
      <Dock>
        <DockItem>
          <DockIcon>
            <Compass />
          </DockIcon>
          <DockLabel>Safari</DockLabel>
        </DockItem>

        <DockItem>
          <DockIcon>
            <Mail />
          </DockIcon>
          <DockLabel>Mail</DockLabel>
        </DockItem>

        <DockItem>
          <DockIcon>
            <Calendar />
          </DockIcon>
          <DockLabel>Calendar</DockLabel>
        </DockItem>

        <DockItem>
          <DockIcon>
            <Music />
          </DockIcon>
          <DockLabel>Music</DockLabel>
        </DockItem>

        <DockItem>
          <DockIcon>
            <Calculator />
          </DockIcon>
          <DockLabel>Calculator</DockLabel>
        </DockItem>
      </Dock>
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/dock.tsx`
```tsx
import { motion, useMotionValue, useSpring, useTransform } from "motion/react";
import * as React from "react";
import { cn } from "@/lib/utils";

interface DockProps {
  /** Distance threshold for magnification effect */
  magnification?: number;
  /** Maximum scale factor when hovered */
  maxScale?: number;
  /** Base size of dock items in pixels */
  iconSize?: number;
  /** Distance from mouse to apply magnification */
  distance?: number;
  className?: string;
  children?: React.ReactNode;
}

const DockContext = React.createContext<{
  mouseX: ReturnType<typeof useMotionValue<number>>;
  magnification: number;
  maxScale: number;
  iconSize: number;
  distance: number;
} | null>(null);

const Dock = React.forwardRef<HTMLDivElement, DockProps>(
  (
    {
      className,
      children,
      magnification = 60,
      maxScale = 1.5,
      iconSize = 48,
      distance = 140,
    },
    ref,
  ) => {
    const mouseX = useMotionValue(Infinity);

    return (
      <DockContext.Provider
        value={{ mouseX, magnification, maxScale, iconSize, distance }}
      >
        <motion.div
          ref={ref}
          onMouseMove={(e) => mouseX.set(e.pageX)}
          onMouseLeave={() => mouseX.set(Infinity)}
          className={cn(
            "dock mx-auto flex h-16 items-end gap-2 rounded-2xl border bg-background/80 px-3 pb-2 backdrop-blur-md",
            "shadow-foreground/5 shadow-lg",
            className,
          )}
        >
          {children}
        </motion.div>
      </DockContext.Provider>
    );
  },
);
Dock.displayName = "Dock";

interface DockItemProps {
  className?: string;
  children?: React.ReactNode;
  onClick?: () => void;
}

const DockItem = React.forwardRef<HTMLDivElement, DockItemProps>(
  ({ className, children, onClick }, _ref) => {
    const context = React.useContext(DockContext);
    const itemRef = React.useRef<HTMLDivElement>(null);

    if (!context) {
      throw new Error("DockItem must be used within a Dock");
    }

    const { mouseX, maxScale, iconSize, distance } = context;

    const distanceCalc = useTransform(mouseX, (val: number) => {
      const bounds = itemRef.current?.getBoundingClientRect() ?? {
        x: 0,
        width: 0,
      };
      return val - bounds.x - bounds.width / 2;
    });

    const widthSync = useTransform(
      distanceCalc,
      [-distance, 0, distance],
      [iconSize, iconSize * maxScale, iconSize],
    );

    const width = useSpring(widthSync, {
      mass: 0.1,
      stiffness: 150,
      damping: 12,
    });

    return (
      <motion.div
        ref={itemRef}
        style={{ width, height: width }}
        onClick={onClick}
        className={cn(
          "dock-item group relative flex aspect-square cursor-pointer items-center justify-center rounded-xl bg-muted transition-colors hover:bg-muted/80",
          className,
        )}
      >
        {children}
      </motion.div>
    );
  },
);
DockItem.displayName = "DockItem";

interface DockIconProps {
  className?: string;
  children?: React.ReactNode;
}

const DockIcon = React.forwardRef<HTMLDivElement, DockIconProps>(
  ({ className, children }, ref) => {
    return (
      <div
        ref={ref}
        className={cn(
          "dock-icon flex h-full w-full items-center justify-center text-foreground",
          "[&>svg]:h-1/2 [&>svg]:w-1/2",
          className,
        )}
      >
        {children}
      </div>
    );
  },
);
DockIcon.displayName = "DockIcon";

interface DockLabelProps {
  className?: string;
  children?: React.ReactNode;
}

const DockLabel = React.forwardRef<HTMLDivElement, DockLabelProps>(
  ({ className, children }, ref) => {
    return (
      <div
        ref={ref}
        className={cn(
          "dock-label pointer-events-none absolute -top-9 left-1/2 -translate-x-1/2 whitespace-nowrap rounded-md bg-foreground px-2 py-1 text-background text-xs opacity-0 transition-opacity group-hover:opacity-100",
          className,
        )}
      >
        {children}
        {/* Tooltip arrow */}
        <div className="absolute -bottom-1 left-1/2 h-2 w-2 -translate-x-1/2 rotate-45 bg-foreground" />
      </div>
    );
  },
);
DockLabel.displayName = "DockLabel";

interface DockSeparatorProps {
  className?: string;
}

const DockSeparator = React.forwardRef<HTMLDivElement, DockSeparatorProps>(
  ({ className }, ref) => {
    return (
      <div
        ref={ref}
        className={cn(
          "dock-separator mx-1 h-10 w-px self-center bg-border",
          className,
        )}
      />
    );
  },
);
DockSeparator.displayName = "DockSeparator";

export {
  Dock,
  DockIcon,
  DockItem,
  DockLabel,
  DockSeparator,
  type DockIconProps,
  type DockItemProps,
  type DockLabelProps,
  type DockProps,
  type DockSeparatorProps,
};
```
---

## Expanded Map <a name="expanded-map"></a>

Expandable location card with map preview for React. Click to reveal full map with 3D tilt animation. No Google Maps API required - uses OpenStreetMap tiles.

- **Registry Key**: `expanded-map`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/expanded-map.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
import { LocationMap } from "@/registry/default/ui/expanded-map";

export default function ExpandedMapDemo() {
  return (
    <div className="flex items-center justify-center p-12">
      <LocationMap />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/expanded-map.tsx`
```tsx
"use client";

import {
  AnimatePresence,
  motion,
  useMotionValue,
  useSpring,
  useTransform,
} from "motion/react";
import NextImage from "next/image";
import type React from "react";
import { useEffect, useMemo, useRef, useState } from "react";

interface LocationMapProps {
  /** Location name to display */
  location?: string;
  /** Latitude coordinate */
  latitude?: number;
  /** Longitude coordinate */
  longitude?: number;
  /** Zoom level for the map (1-18) */
  zoom?: number;
  /** Additional CSS classes */
  className?: string;
  /** Map tile provider */
  tileProvider?: "openstreetmap" | "carto-light" | "carto-dark";
}

// Convert lat/lng to tile coordinates
function latLngToTile(lat: number, lng: number, zoom: number) {
  const n = 2 ** zoom;
  const x = Math.floor(((lng + 180) / 360) * n);
  const latRad = (lat * Math.PI) / 180;
  const y = Math.floor(
    ((1 - Math.log(Math.tan(latRad) + 1 / Math.cos(latRad)) / Math.PI) / 2) * n,
  );
  return { x, y };
}

// Get tile URL based on provider
function getTileUrl(provider: string, x: number, y: number, z: number) {
  switch (provider) {
    case "carto-light":
      return `https://cartodb-basemaps-a.global.ssl.fastly.net/light_all/${z}/${x}/${y}.png`;
    case "carto-dark":
      return `https://cartodb-basemaps-a.global.ssl.fastly.net/dark_all/${z}/${x}/${y}.png`;
    default:
      return `https://tile.openstreetmap.org/${z}/${x}/${y}.png`;
  }
}

// Format coordinates for display
function formatCoordinates(lat: number, lng: number) {
  const latDir = lat >= 0 ? "N" : "S";
  const lngDir = lng >= 0 ? "E" : "W";
  return `${Math.abs(lat).toFixed(4)}° ${latDir}, ${Math.abs(lng).toFixed(4)}° ${lngDir}`;
}

export function LocationMap({
  location = "San Francisco, CA",
  latitude = 37.7749,
  longitude = -122.4194,
  zoom = 14,
  className,
  tileProvider = "carto-light",
}: LocationMapProps) {
  const [isHovered, setIsHovered] = useState(false);
  const [isExpanded, setIsExpanded] = useState(false);
  const [tilesLoaded, setTilesLoaded] = useState(false);
  const containerRef = useRef<HTMLDivElement>(null);

  const mouseX = useMotionValue(0);
  const mouseY = useMotionValue(0);

  const rotateX = useTransform(mouseY, [-50, 50], [8, -8]);
  const rotateY = useTransform(mouseX, [-50, 50], [-8, 8]);

  const springRotateX = useSpring(rotateX, { stiffness: 300, damping: 30 });
  const springRotateY = useSpring(rotateY, { stiffness: 300, damping: 30 });

  const coordinates = useMemo(
    () => formatCoordinates(latitude, longitude),
    [latitude, longitude],
  );

  // Generate tile URLs for a 3x3 grid around the center tile
  const tiles = useMemo(() => {
    const centerTile = latLngToTile(latitude, longitude, zoom);
    const tileUrls: { url: string; offsetX: number; offsetY: number }[] = [];

    for (let dy = -1; dy <= 1; dy++) {
      for (let dx = -1; dx <= 1; dx++) {
        tileUrls.push({
          url: getTileUrl(
            tileProvider,
            centerTile.x + dx,
            centerTile.y + dy,
            zoom,
          ),
          offsetX: dx,
          offsetY: dy,
        });
      }
    }

    return tileUrls;
  }, [latitude, longitude, zoom, tileProvider]);

  // Preload tiles
  useEffect(() => {
    let loadedCount = 0;
    const totalTiles = tiles.length;

    tiles.forEach((tile) => {
      const img = new Image();
      img.crossOrigin = "anonymous";
      img.onload = () => {
        loadedCount++;
        if (loadedCount === totalTiles) {
          setTilesLoaded(true);
        }
      };
      img.onerror = () => {
        loadedCount++;
        if (loadedCount === totalTiles) {
          setTilesLoaded(true);
        }
      };
      img.src = tile.url;
    });
  }, [tiles]);

  const handleMouseMove = (e: React.MouseEvent) => {
    if (!containerRef.current) return;
    const rect = containerRef.current.getBoundingClientRect();
    const centerX = rect.left + rect.width / 2;
    const centerY = rect.top + rect.height / 2;
    mouseX.set(e.clientX - centerX);
    mouseY.set(e.clientY - centerY);
  };

  const handleMouseLeave = () => {
    mouseX.set(0);
    mouseY.set(0);
    setIsHovered(false);
  };

  const handleClick = () => {
    setIsExpanded(!isExpanded);
  };

  return (
    <motion.div
      ref={containerRef}
      className={`relative cursor-pointer select-none ${className}`}
      style={{
        perspective: 1000,
      }}
      onMouseMove={handleMouseMove}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={handleMouseLeave}
      onClick={handleClick}
    >
      <motion.div
        className="relative overflow-hidden rounded-2xl border border-border bg-background"
        style={{
          rotateX: springRotateX,
          rotateY: springRotateY,
          transformStyle: "preserve-3d",
        }}
        animate={{
          width: isExpanded ? 360 : 240,
          height: isExpanded ? 280 : 140,
        }}
        transition={{
          type: "spring",
          stiffness: 400,
          damping: 35,
        }}
      >
        {/* Subtle gradient overlay */}
        <div className="pointer-events-none absolute inset-0 z-20 bg-gradient-to-br from-muted/20 via-transparent to-muted/40" />

        <AnimatePresence>
          {isExpanded && (
            <motion.div
              className="pointer-events-none absolute inset-0"
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              exit={{ opacity: 0 }}
              transition={{ duration: 0.4, delay: 0.1 }}
            >
              {/* Real map tiles */}
              <div className="absolute inset-0 overflow-hidden">
                <div
                  className="absolute"
                  style={{
                    width: "768px", // 3 tiles * 256px
                    height: "768px",
                    left: "50%",
                    top: "50%",
                    transform: "translate(-50%, -50%)",
                  }}
                >
                  {tiles.map((tile, index) => (
                    <motion.div
                      key={index}
                      className="absolute"
                      style={{
                        width: "256px",
                        height: "256px",
                        left: `${(tile.offsetX + 1) * 256}px`,
                        top: `${(tile.offsetY + 1) * 256}px`,
                      }}
                      initial={{ opacity: 0 }}
                      animate={{ opacity: tilesLoaded ? 1 : 0 }}
                      transition={{ duration: 0.3, delay: index * 0.05 }}
                    >
                      <NextImage
                        src={tile.url}
                        alt=""
                        width={256}
                        height={256}
                        unoptimized
                        crossOrigin="anonymous"
                        className="h-full w-full"
                      />
                    </motion.div>
                  ))}
                </div>
              </div>

              {/* Map loading placeholder */}
              {!tilesLoaded && (
                <div className="absolute inset-0 animate-pulse bg-muted" />
              )}

              {/* Location marker */}
              <motion.div
                className="absolute top-1/2 left-1/2 z-10 -translate-x-1/2 -translate-y-1/2"
                initial={{ scale: 0, y: -20 }}
                animate={{ scale: 1, y: 0 }}
                transition={{
                  type: "spring",
                  stiffness: 400,
                  damping: 20,
                  delay: 0.3,
                }}
              >
                <svg
                  width="32"
                  height="32"
                  viewBox="0 0 24 24"
                  fill="none"
                  className="drop-shadow-lg"
                  style={{
                    filter: "drop-shadow(0 0 10px rgba(52, 211, 153, 0.5))",
                  }}
                >
                  <path
                    d="M12 2C8.13 2 5 5.13 5 9c0 5.25 7 13 7 13s7-7.75 7-13c0-3.87-3.13-7-7-7z"
                    fill="#34D399"
                  />
                  <circle cx="12" cy="9" r="2.5" className="fill-background" />
                </svg>
              </motion.div>

              {/* Gradient overlays for better text readability */}
              <div className="absolute inset-0 z-10 bg-gradient-to-t from-background via-transparent to-transparent opacity-70" />
              <div className="absolute inset-0 z-10 bg-gradient-to-b from-background/50 via-transparent to-transparent" />
            </motion.div>
          )}
        </AnimatePresence>

        {/* Grid pattern - only show when collapsed */}
        <motion.div
          className="absolute inset-0 opacity-[0.03]"
          animate={{ opacity: isExpanded ? 0 : 0.03 }}
          transition={{ duration: 0.3 }}
        >
          <svg width="100%" height="100%" className="absolute inset-0">
            <defs>
              <pattern
                id="grid"
                width="20"
                height="20"
                patternUnits="userSpaceOnUse"
              >
                <path
                  d="M 20 0 L 0 0 0 20"
                  fill="none"
                  className="stroke-foreground"
                  strokeWidth="0.5"
                />
              </pattern>
            </defs>
            <rect width="100%" height="100%" fill="url(#grid)" />
          </svg>
        </motion.div>

        {/* Content */}
        <div className="relative z-20 flex h-full flex-col justify-between p-5">
          {/* Top section */}
          <div className="flex items-start justify-between">
            <div className="relative">
              <motion.div
                className="relative"
                animate={{
                  opacity: isExpanded ? 0 : 1,
                }}
                transition={{ duration: 0.3 }}
              >
                {/* Map Icon SVG */}
                <motion.svg
                  width="18"
                  height="18"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  strokeWidth="2"
                  strokeLinecap="round"
                  strokeLinejoin="round"
                  className="text-emerald-400"
                  animate={{
                    filter: isHovered
                      ? "drop-shadow(0 0 8px rgba(52, 211, 153, 0.6))"
                      : "drop-shadow(0 0 4px rgba(52, 211, 153, 0.3))",
                  }}
                  transition={{ duration: 0.3 }}
                >
                  <polygon points="3 6 9 3 15 6 21 3 21 18 15 21 9 18 3 21" />
                  <line x1="9" x2="9" y1="3" y2="18" />
                  <line x1="15" x2="15" y1="6" y2="21" />
                </motion.svg>
              </motion.div>
            </div>
          </div>

          {/* Bottom section */}
          <div className="space-y-1">
            <motion.h3
              className="font-medium text-foreground text-sm tracking-tight"
              animate={{
                x: isHovered ? 4 : 0,
              }}
              transition={{ type: "spring", stiffness: 400, damping: 25 }}
            >
              {location}
            </motion.h3>

            <AnimatePresence>
              {isExpanded && (
                <motion.p
                  className="font-mono text-muted-foreground text-xs"
                  initial={{ opacity: 0, y: -10, height: 0 }}
                  animate={{ opacity: 1, y: 0, height: "auto" }}
                  exit={{ opacity: 0, y: -10, height: 0 }}
                  transition={{ duration: 0.25 }}
                >
                  {coordinates}
                </motion.p>
              )}
            </AnimatePresence>

            {/* Animated underline */}
            <motion.div
              className="h-px bg-gradient-to-r from-emerald-500/50 via-emerald-400/30 to-transparent"
              initial={{ scaleX: 0, originX: 0 }}
              animate={{
                scaleX: isHovered || isExpanded ? 1 : 0.3,
              }}
              transition={{ duration: 0.4, ease: "easeOut" }}
            />
          </div>
        </div>
      </motion.div>
    </motion.div>
  );
}
```
---

## Falling Text <a name="falling-text"></a>

A physics-based text animation component that makes words fall and interact with realistic physics. Supports multiple triggers, customizable physics, and mouse interactions.

- **Registry Key**: `falling-text`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/falling-text.json
```

#### Dependencies
- **NPM Packages**:
  - `matter-js`

#### How to Use
```tsx
import FallingText from "@/registry/default/ui/falling-text";

export default function FallingTextDemo() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center">
      <FallingText
        text="Jolyui makes building interfaces fun and interactive"
        highlightWords={["Jolyui", "fun", "interactive"]}
        trigger="auto"
        fontSize="2rem"
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/falling-text.tsx`
```tsx
"use client";
import Matter from "matter-js";
import { useCallback, useEffect, useRef, useState } from "react";

export interface FallingTextProps {
  /** The text content to display and animate */
  text: string;
  /** Words to highlight with special styling */
  highlightWords?: string[];
  /** When to trigger the falling animation */
  trigger?: "auto" | "scroll" | "click" | "hover";
  /** Background color for the physics canvas */
  backgroundColor?: string;
  /** Show physics wireframes for debugging */
  wireframes?: boolean;
  /** Gravity strength (default: 1) */
  gravity?: number;
  /** Mouse interaction stiffness (0-1, default: 0.2) */
  mouseConstraintStiffness?: number;
  /** Font size for the text */
  fontSize?: string;
  /** Custom className for the container */
  className?: string;
  /** Callback when animation starts */
  onAnimationStart?: () => void;
  /** Callback when animation ends (all bodies settled) */
  onAnimationEnd?: () => void;
  /** Physics properties for word bodies */
  physicsOptions?: {
    restitution?: number; // Bounciness (0-1)
    frictionAir?: number; // Air resistance
    friction?: number; // Surface friction
    density?: number; // Mass density
  };
  /** Initial velocity range for words */
  initialVelocity?: {
    x?: number; // Horizontal velocity range
    y?: number; // Vertical velocity range
    angular?: number; // Angular velocity range
  };
  /** Custom highlight styles */
  highlightClassName?: string;
  /** Word spacing in pixels */
  wordSpacing?: number;
  /** Minimum container height */
  minHeight?: string;
  /** Enable/disable mouse interactions */
  enableMouseInteraction?: boolean;
  /** Reset trigger - increment to reset animation */
  resetKey?: number;
}

const FallingText: React.FC<FallingTextProps> = ({
  text,
  highlightWords = [],
  trigger = "auto",
  backgroundColor = "transparent",
  wireframes = false,
  gravity = 1,
  mouseConstraintStiffness = 0.2,
  fontSize = "1rem",
  className = "",
  onAnimationStart,
  onAnimationEnd,
  physicsOptions = {},
  initialVelocity = {},
  highlightClassName = "text-cyan-500 font-bold",
  wordSpacing = 2,
  minHeight = "300px",
  enableMouseInteraction = true,
  resetKey = 0,
}) => {
  const containerRef = useRef<HTMLDivElement | null>(null);
  const textRef = useRef<HTMLDivElement | null>(null);
  const canvasContainerRef = useRef<HTMLDivElement | null>(null);
  const engineRef = useRef<Matter.Engine | null>(null);
  const renderRef = useRef<Matter.Render | null>(null);
  const runnerRef = useRef<Matter.Runner | null>(null);
  const animationFrameRef = useRef<number | null>(null);
  const hasStartedRef = useRef(false);

  const [effectStarted, setEffectStarted] = useState(false);
  const [isReady, setIsReady] = useState(false);

  // Merge default physics options with user-provided ones
  const mergedPhysicsOptions = {
    restitution: 0.8,
    frictionAir: 0.01,
    friction: 0.2,
    density: 0.001,
    ...physicsOptions,
  };

  const mergedInitialVelocity = {
    x: 5,
    y: 0,
    angular: 0.05,
    ...initialVelocity,
  };

  // Cleanup function
  const cleanup = useCallback(() => {
    if (animationFrameRef.current) {
      cancelAnimationFrame(animationFrameRef.current);
      animationFrameRef.current = null;
    }

    if (runnerRef.current && engineRef.current) {
      Matter.Runner.stop(runnerRef.current);
      runnerRef.current = null;
    }

    if (renderRef.current) {
      Matter.Render.stop(renderRef.current);
      if (renderRef.current.canvas && canvasContainerRef.current) {
        try {
          canvasContainerRef.current.removeChild(renderRef.current.canvas);
        } catch (_e) {
          // Canvas might already be removed
        }
      }
      renderRef.current = null;
    }

    if (engineRef.current) {
      Matter.World.clear(engineRef.current.world, false);
      Matter.Engine.clear(engineRef.current);
      engineRef.current = null;
    }

    hasStartedRef.current = false;
  }, []);

  // Reset animation when resetKey changes
  useEffect(() => {
    if (resetKey > 0) {
      cleanup();
      setEffectStarted(false);
      setIsReady(false);
      // Trigger re-initialization
      setTimeout(() => setIsReady(true), 50);
    }
  }, [resetKey, cleanup]);

  // Initialize text spans
  useEffect(() => {
    if (!textRef.current || !text) return;

    const words = text.split(" ").filter((word) => word.length > 0);

    const newHTML = words
      .map((word) => {
        const isHighlighted = highlightWords.some((hw) =>
          word.toLowerCase().startsWith(hw.toLowerCase()),
        );
        return `<span
          class="inline-block select-none transition-colors duration-200 ${
            isHighlighted ? highlightClassName : ""
          }"
          style="margin: 0 ${wordSpacing}px;"
        >
          ${word}
        </span>`;
      })
      .join(" ");

    textRef.current.innerHTML = newHTML;
    setIsReady(true);
  }, [text, highlightWords, highlightClassName, wordSpacing]);

  // Handle trigger mechanisms
  useEffect(() => {
    if (trigger === "auto") {
      setEffectStarted(true);
      return;
    }

    if (trigger === "scroll" && containerRef.current) {
      const observer = new IntersectionObserver(
        ([entry]) => {
          if (entry?.isIntersecting) {
            setEffectStarted(true);
            observer.disconnect();
          }
        },
        { threshold: 0.1 },
      );
      observer.observe(containerRef.current);
      return () => observer.disconnect();
    }
  }, [trigger]);

  // Main physics effect
  useEffect(() => {
    if (!effectStarted || !isReady || hasStartedRef.current) return;

    const {
      Engine,
      Render,
      World,
      Bodies,
      Runner,
      Mouse,
      MouseConstraint,
      Body,
    } = Matter;

    if (
      !containerRef.current ||
      !canvasContainerRef.current ||
      !textRef.current
    )
      return;

    const containerRect = containerRef.current.getBoundingClientRect();
    const width = containerRect.width;
    const height = containerRect.height;

    if (width <= 0 || height <= 0) {
      console.warn("FallingText: Container has invalid dimensions");
      return;
    }

    hasStartedRef.current = true;
    onAnimationStart?.();

    // Create engine
    const engine = Engine.create();
    engine.world.gravity.y = gravity;
    engineRef.current = engine;

    // Create renderer
    const render = Render.create({
      element: canvasContainerRef.current,
      engine,
      options: {
        width,
        height,
        background: backgroundColor,
        wireframes,
      },
    });
    renderRef.current = render;

    // Create boundaries
    const boundaryOptions = {
      isStatic: true,
      render: { fillStyle: "transparent" },
    };

    const floor = Bodies.rectangle(
      width / 2,
      height + 25,
      width,
      50,
      boundaryOptions,
    );
    const leftWall = Bodies.rectangle(
      -25,
      height / 2,
      50,
      height,
      boundaryOptions,
    );
    const rightWall = Bodies.rectangle(
      width + 25,
      height / 2,
      50,
      height,
      boundaryOptions,
    );
    const ceiling = Bodies.rectangle(
      width / 2,
      -25,
      width,
      50,
      boundaryOptions,
    );

    // Create word bodies
    const wordSpans = textRef.current.querySelectorAll("span");
    const wordBodies = Array.from(wordSpans).map((elem) => {
      const rect = elem.getBoundingClientRect();
      const x = rect.left - containerRect.left + rect.width / 2;
      const y = rect.top - containerRect.top + rect.height / 2;

      const body = Bodies.rectangle(x, y, rect.width, rect.height, {
        render: { fillStyle: "transparent" },
        ...mergedPhysicsOptions,
      });

      // Set initial velocities
      Body.setVelocity(body, {
        x: (Math.random() - 0.5) * mergedInitialVelocity.x,
        y: (Math.random() - 0.5) * mergedInitialVelocity.y,
      });
      Body.setAngularVelocity(
        body,
        (Math.random() - 0.5) * mergedInitialVelocity.angular,
      );

      return { elem: elem as HTMLElement, body };
    });

    // Position elements initially
    wordBodies.forEach(({ elem, body }) => {
      elem.style.position = "absolute";
      elem.style.left = `${body.position.x}px`;
      elem.style.top = `${body.position.y}px`;
      elem.style.transform = "translate(-50%, -50%)";
      elem.style.willChange = "transform";
    });

    // Add mouse interaction
    let mouseConstraint: Matter.MouseConstraint | null = null;
    if (enableMouseInteraction) {
      const mouse = Mouse.create(containerRef.current);
      mouseConstraint = MouseConstraint.create(engine, {
        mouse,
        constraint: {
          stiffness: mouseConstraintStiffness,
          render: { visible: false },
        },
      });
      render.mouse = mouse;
    }

    // Add all bodies to world
    const bodiesToAdd = [
      floor,
      leftWall,
      rightWall,
      ceiling,
      ...wordBodies.map((wb) => wb.body),
    ];
    if (mouseConstraint) {
      bodiesToAdd.push(mouseConstraint as any);
    }
    World.add(engine.world, bodiesToAdd);

    // Create runner
    const runner = Runner.create();
    runnerRef.current = runner;
    Runner.run(runner, engine);
    Render.run(render);

    // Update loop
    let settledCount = 0;
    const updateLoop = () => {
      wordBodies.forEach(({ body, elem }) => {
        const { x, y } = body.position;
        elem.style.left = `${x}px`;
        elem.style.top = `${y}px`;
        elem.style.transform = `translate(-50%, -50%) rotate(${body.angle}rad)`;
      });

      // Check if bodies have settled
      const allSettled = wordBodies.every(({ body }) => {
        const speed = Math.abs(body.velocity.x) + Math.abs(body.velocity.y);
        return speed < 0.1;
      });

      if (allSettled) {
        settledCount++;
        if (settledCount > 60) {
          // Settled for 1 second at 60fps
          onAnimationEnd?.();
          return;
        }
      } else {
        settledCount = 0;
      }

      animationFrameRef.current = requestAnimationFrame(updateLoop);
    };
    updateLoop();

    return cleanup;
  }, [
    effectStarted,
    isReady,
    gravity,
    wireframes,
    backgroundColor,
    mouseConstraintStiffness,
    mergedPhysicsOptions,
    mergedInitialVelocity,
    enableMouseInteraction,
    cleanup,
    onAnimationStart,
    onAnimationEnd,
  ]);

  const handleTrigger = useCallback(() => {
    if (!effectStarted && (trigger === "click" || trigger === "hover")) {
      setEffectStarted(true);
    }
  }, [effectStarted, trigger]);

  return (
    // biome-ignore lint/a11y/noStaticElementInteractions: This is a decorative visual effect that responds to mouse events for aesthetic purposes
    <div
      ref={containerRef}
      className={`relative z-[1] w-full overflow-hidden pt-8 text-center ${className}`}
      style={{ minHeight }}
      onClick={trigger === "click" ? handleTrigger : undefined}
      onMouseEnter={trigger === "hover" ? handleTrigger : undefined}
      role="presentation"
      aria-label={
        trigger !== "auto" ? "Click or hover to animate text" : undefined
      }
    >
      <div
        ref={textRef}
        className="pointer-events-none inline-block"
        style={{
          fontSize,
          lineHeight: 1.4,
        }}
        aria-live="polite"
      />

      <div
        className="pointer-events-none absolute inset-0"
        ref={canvasContainerRef}
        aria-hidden="true"
      />
    </div>
  );
};

export default FallingText;
```
---

## Feedback Widget <a name="feedback-widget"></a>

Vercel-style feedback widget for React. Emoji rating scale with optional text area. Perfect for collecting user feedback in SaaS apps.

- **Registry Key**: `feedback-widget`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/feedback-widget.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `react-markdown`
  - `@radix-ui/react-toggle-group`

#### How to Use
```tsx
"use client";

import { FeedbackWidget } from "@/registry/default/ui/feedback-widget";

export default function FeedbackWidgetDemo() {
  return (
    <div className="relative flex h-[400px] w-full items-center justify-center">
      <FeedbackWidget
        onSubmit={(data) => {
          console.log("Feedback submitted:", data);
        }}
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/feedback-widget.tsx`
```tsx
"use client";

import * as ToggleGroup from "@radix-ui/react-toggle-group";
import { AnimatePresence, motion } from "motion/react";
import * as React from "react";
import ReactMarkdown from "react-markdown";
import { cn } from "@/lib/utils";

const EMOJIS = [
  {
    id: "very-sad",
    label: "Terrible",
    icon: (
      <svg
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth="2"
        strokeLinecap="round"
        strokeLinejoin="round"
      >
        <circle cx="12" cy="12" r="10" />
        <path d="M16 16s-1.5-2-4-2-4 2-4 2" />
        <path d="M9 9h.01" />
        <path d="M15 9h.01" />
        <path d="M9 13v2" stroke="#3b82f6" />
        <path d="M15 13v2" stroke="#3b82f6" />
      </svg>
    ),
  },
  {
    id: "sad",
    label: "Bad",
    icon: (
      <svg
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth="2"
        strokeLinecap="round"
        strokeLinejoin="round"
      >
        <circle cx="12" cy="12" r="10" />
        <path d="M16 16s-1.5-2-4-2-4 2-4 2" />
        <line x1="9" y1="9" x2="9.01" y2="9" />
        <line x1="15" y1="9" x2="15.01" y2="9" />
      </svg>
    ),
  },
  {
    id: "neutral",
    label: "Okay",
    icon: (
      <svg
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth="2"
        strokeLinecap="round"
        strokeLinejoin="round"
      >
        <circle cx="12" cy="12" r="10" />
        <path d="M8 13s1.5 2 4 2 4-2 4-2" />
        <line x1="9" y1="9" x2="9.01" y2="9" />
        <line x1="15" y1="9" x2="15.01" y2="9" />
      </svg>
    ),
  },
  {
    id: "happy",
    label: "Amazing",
    icon: (
      <svg
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth="2"
        strokeLinecap="round"
        strokeLinejoin="round"
      >
        <circle cx="12" cy="12" r="10" />
        <path d="M8 13s1.5 2 4 2 4-2 4-2" />
        <path
          d="M9 9l.5 1.5l1.5 .5l-1.5 .5l-.5 1.5l-.5-1.5l-1.5-.5l1.5-.5z"
          fill="#f97316"
          stroke="none"
        />
        <path
          d="M15 9l.5 1.5l1.5 .5l-1.5 .5l-.5 1.5l-.5-1.5l-1.5-.5l1.5-.5z"
          fill="#f97316"
          stroke="none"
        />
      </svg>
    ),
  },
];

interface FeedbackWidgetProps {
  onSubmit?: (data: { rating: string; feedback: string }) => void;
  onClose?: () => void;
  className?: string;
  /** Text shown in the collapsed state */
  label?: string;
  /** Placeholder for the textarea */
  placeholder?: string;
}

export function FeedbackWidget({
  onSubmit,
  onClose,
  className,
  label = "Was this helpful?",
  placeholder = "Your feedback...",
}: FeedbackWidgetProps) {
  const [value, setValue] = React.useState<string>("");
  const [feedback, setFeedback] = React.useState("");
  const [isPreview, setIsPreview] = React.useState(false);
  const [isSubmitting, setIsSubmitting] = React.useState(false);
  const isExpanded = value !== "";
  const containerRef = React.useRef<HTMLDivElement>(null);

  const springTransition = {
    type: "spring",
    stiffness: 300,
    damping: 30,
    mass: 1,
  } as const;

  const handleValueChange = (val: string) => {
    if (val === "" || val === value) {
      setValue("");
      setIsPreview(false);
      onClose?.();
    } else {
      setValue(val);
      // Auto-focus the textarea after expansion
      setTimeout(() => {
        containerRef.current?.querySelector("textarea")?.focus();
      }, 100);
    }
  };

  const handleSend = async () => {
    if (!feedback.trim()) return;

    setIsSubmitting(true);
    try {
      await onSubmit?.({ rating: value, feedback });
      setValue("");
      setFeedback("");
      setIsPreview(false);
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <div className={cn("flex items-center justify-center p-4", className)}>
      <motion.div
        ref={containerRef}
        layout
        transition={springTransition}
        initial={false}
        className={cn(
          "overflow-hidden border border-zinc-200 bg-white text-zinc-900 shadow-[0_32px_64px_-16px_rgba(0,0,0,0.15)] dark:border-white/10 dark:bg-zinc-950 dark:text-white dark:shadow-[0_32px_64px_-16px_rgba(0,0,0,0.8)]",
          isExpanded ? "w-full max-w-[420px] rounded-[28px]" : "rounded-full",
        )}
      >
        <motion.div
          layout="position"
          className="px-4 py-2 md:px-5 md:py-2.5"
          transition={springTransition}
        >
          <div className="flex items-center justify-between gap-6">
            <motion.span
              layout="position"
              transition={springTransition}
              className="ml-2 cursor-default select-none whitespace-nowrap font-medium text-[14px] text-zinc-600 dark:text-zinc-400"
            >
              {label}
            </motion.span>

            <ToggleGroup.Root
              type="single"
              value={value}
              onValueChange={handleValueChange}
              className="flex items-center gap-1.5"
            >
              {EMOJIS.map((emoji) => (
                <ToggleGroup.Item key={emoji.id} value={emoji.id} asChild>
                  <button
                    title={emoji.label}
                    className={cn(
                      "relative rounded-full p-2 outline-none transition-colors focus-visible:ring-2 focus-visible:ring-blue-500",
                      value === emoji.id
                        ? "text-white"
                        : "text-zinc-400 hover:bg-zinc-100 hover:text-zinc-600 dark:text-zinc-500 dark:hover:bg-white/5 dark:hover:text-zinc-300",
                    )}
                  >
                    <motion.div
                      layout="position"
                      transition={springTransition}
                      className="relative z-10 flex h-5 w-5 scale-110 items-center justify-center transition-transform active:scale-90"
                    >
                      {emoji.icon}
                    </motion.div>
                    {value === emoji.id && (
                      <motion.div
                        layoutId="active-bg"
                        className="absolute inset-0 rounded-full bg-blue-600"
                        transition={springTransition}
                      />
                    )}
                  </button>
                </ToggleGroup.Item>
              ))}
            </ToggleGroup.Root>
          </div>

          <AnimatePresence mode="popLayout" initial={false}>
            {isExpanded && (
              <motion.div
                initial={{
                  height: 0,
                  opacity: 0,
                  scale: 0.98,
                  filter: "blur(4px)",
                }}
                animate={{
                  height: "auto",
                  opacity: 1,
                  scale: 1,
                  filter: "blur(0px)",
                }}
                exit={{
                  height: 0,
                  opacity: 0,
                  scale: 0.98,
                  filter: "blur(4px)",
                  transition: {
                    height: { duration: 0.3, ease: [0.32, 0, 0.67, 0] },
                    opacity: { duration: 0.15 },
                    scale: { duration: 0.2 },
                    filter: { duration: 0.2 },
                  },
                }}
                transition={{
                  height: { ...springTransition, bounce: 0 },
                  opacity: { duration: 0.25 },
                  scale: { ...springTransition, damping: 25 },
                  filter: { duration: 0.3 },
                }}
                className="overflow-hidden"
              >
                <div className="px-1 pt-6 pb-2">
                  <div className="mb-2.5 flex items-center justify-between">
                    <span className="select-none font-bold text-[10px] text-zinc-500 uppercase tracking-[0.1em] dark:text-zinc-500">
                      {isPreview ? "Preview" : "Feedback"}
                    </span>
                    <button
                      onClick={() => setIsPreview(!isPreview)}
                      className="rounded-md bg-zinc-100 px-2 py-0.5 font-semibold text-[11px] text-zinc-600 transition-colors hover:bg-zinc-200 hover:text-zinc-900 dark:bg-white/5 dark:text-zinc-400 dark:hover:bg-white/10 dark:hover:text-white"
                    >
                      {isPreview ? "Edit" : "Preview"}
                    </button>
                  </div>

                  <div className="group/textarea relative">
                    <AnimatePresence mode="wait">
                      {isPreview ? (
                        <motion.div
                          key="preview"
                          initial={{ opacity: 0, y: 5 }}
                          animate={{ opacity: 1, y: 0 }}
                          exit={{ opacity: 0, y: -5 }}
                          className="prose prose-sm scrollbar-none dark:prose-invert h-[140px] w-full max-w-none overflow-y-auto rounded-2xl border border-zinc-200 bg-zinc-50 p-4 text-[14px] text-zinc-700 leading-relaxed dark:border-white/5 dark:bg-zinc-900/50 dark:text-zinc-300"
                        >
                          <ReactMarkdown>
                            {feedback || "*Nothing to preview...*"}
                          </ReactMarkdown>
                        </motion.div>
                      ) : (
                        <motion.textarea
                          key="editor"
                          initial={{ opacity: 0, y: 5 }}
                          animate={{ opacity: 1, y: 0 }}
                          exit={{ opacity: 0, y: -5 }}
                          autoFocus
                          placeholder={placeholder}
                          value={feedback}
                          onChange={(e) => setFeedback(e.target.value)}
                          className="scrollbar-none h-[140px] w-full resize-none rounded-2xl border border-zinc-200 bg-zinc-50 p-4 text-[14px] text-zinc-800 leading-relaxed transition-all placeholder:text-zinc-400 focus:border-zinc-300 focus:outline-none dark:border-white/5 dark:bg-zinc-900/50 dark:text-zinc-200 dark:focus:border-white/20 dark:placeholder:text-zinc-600"
                        />
                      )}
                    </AnimatePresence>

                    {!isPreview && (
                      <div className="pointer-events-none absolute right-4 bottom-3 flex select-none items-center gap-1.5 opacity-40 transition-opacity group-focus-within/textarea:opacity-80">
                        <span className="font-bold text-[10px] text-zinc-400 tracking-tight dark:text-zinc-500">
                          M↓
                        </span>
                        <span className="font-bold text-[10px] text-zinc-400 tracking-tight dark:text-zinc-500">
                          supported
                        </span>
                      </div>
                    )}
                  </div>
                </div>

                <motion.div
                  initial={{ y: 20, opacity: 0 }}
                  animate={{ y: 0, opacity: 1 }}
                  exit={{ y: 20, opacity: 0 }}
                  transition={{ delay: 0.1, ...springTransition }}
                  className="mt-3 flex items-center justify-between border-zinc-200 border-t pt-4 dark:border-white/5"
                >
                  <p className="font-medium text-[11px] text-zinc-500 dark:text-zinc-500">
                    We appreciate your input.
                  </p>
                  <button
                    onClick={handleSend}
                    disabled={!feedback.trim() || isSubmitting}
                    className="relative rounded-xl bg-zinc-900 px-6 py-2 font-bold text-[13px] text-white transition-all hover:bg-zinc-800 active:scale-95 disabled:pointer-events-none disabled:opacity-30 disabled:grayscale dark:bg-white dark:text-black dark:hover:bg-zinc-200"
                  >
                    {isSubmitting ? (
                      <motion.div
                        animate={{ rotate: 360 }}
                        transition={{
                          repeat: Number.POSITIVE_INFINITY,
                          duration: 1,
                          ease: "linear",
                        }}
                        className="h-4 w-4 rounded-full border-2 border-white/20 border-t-white dark:border-black/20 dark:border-t-black"
                      />
                    ) : (
                      "Send Feedback"
                    )}
                  </button>
                </motion.div>
              </motion.div>
            )}
          </AnimatePresence>
        </motion.div>
      </motion.div>
    </div>
  );
}
```
---

## File Tree <a name="file-tree"></a>

VS Code-style file tree component for React. Expandable folders, file icons, selection states, and keyboard navigation. Perfect for IDE-like interfaces.

- **Registry Key**: `file-tree`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/file-tree.json
```

#### Dependencies
- **NPM Packages**:
  - `lucide-react`
  - `motion`

#### Component Source Code
##### File: `ui/file-tree.tsx`
```tsx
import { ChevronRight, File, Folder, FolderOpen } from "lucide-react";
import { AnimatePresence, motion } from "motion/react";
import * as React from "react";
import { cn } from "@/lib/utils";

export interface TreeNode {
  id: string;
  name: string;
  type: "file" | "folder";
  children?: TreeNode[];
  icon?: React.ReactNode;
}

interface FileTreeContextValue {
  selectedId: string | null;
  setSelectedId: (id: string | null) => void;
  expandedIds: Set<string>;
  toggleExpanded: (id: string) => void;
}

const FileTreeContext = React.createContext<FileTreeContextValue | null>(null);

function useFileTree() {
  const context = React.useContext(FileTreeContext);
  if (!context) {
    throw new Error("useFileTree must be used within a FileTree");
  }
  return context;
}

function getAllFolderIds(nodes: TreeNode[]): string[] {
  const ids: string[] = [];
  for (const node of nodes) {
    if (node.type === "folder") {
      ids.push(node.id);
      if (node.children) {
        ids.push(...getAllFolderIds(node.children));
      }
    }
  }
  return ids;
}

interface FileTreeProps {
  data: TreeNode[];
  className?: string;
  defaultExpandedIds?: string[];
  expandAllByDefault?: boolean;
  onSelect?: (node: TreeNode) => void;
}

export function FileTree({
  data,
  className,
  defaultExpandedIds = [],
  expandAllByDefault = false,
  onSelect,
}: FileTreeProps) {
  const [selectedId, setSelectedId] = React.useState<string | null>(null);
  const [expandedIds, setExpandedIds] = React.useState<Set<string>>(
    new Set(expandAllByDefault ? getAllFolderIds(data) : defaultExpandedIds),
  );

  const toggleExpanded = React.useCallback((id: string) => {
    setExpandedIds((prev) => {
      const next = new Set(prev);
      if (next.has(id)) {
        next.delete(id);
      } else {
        next.add(id);
      }
      return next;
    });
  }, []);

  const handleSelect = React.useCallback(
    (node: TreeNode) => {
      setSelectedId(node.id);
      onSelect?.(node);
    },
    [onSelect],
  );

  return (
    <FileTreeContext.Provider
      value={{
        selectedId,
        setSelectedId: (id) => setSelectedId(id),
        expandedIds,
        toggleExpanded,
      }}
    >
      <div
        className={cn(
          "rounded-lg border border-border bg-card p-2 font-mono text-sm",
          className,
        )}
        role="tree"
        aria-label="File tree"
      >
        <TreeNodeList nodes={data} level={0} onSelect={handleSelect} />
      </div>
    </FileTreeContext.Provider>
  );
}

interface TreeNodeListProps {
  nodes: TreeNode[];
  level: number;
  onSelect: (node: TreeNode) => void;
}

function TreeNodeList({ nodes, level, onSelect }: TreeNodeListProps) {
  return (
    <ul className="space-y-0.5" role="group">
      {nodes.map((node, index) => (
        <TreeNodeItem
          key={node.id}
          node={node}
          level={level}
          onSelect={onSelect}
          isLast={index === nodes.length - 1}
        />
      ))}
    </ul>
  );
}

interface TreeNodeItemProps {
  node: TreeNode;
  level: number;
  onSelect: (node: TreeNode) => void;
  isLast: boolean;
}

function TreeNodeItem({
  node,
  level,
  onSelect,
  isLast: _isLast,
}: TreeNodeItemProps) {
  const { selectedId, expandedIds, toggleExpanded } = useFileTree();
  const isExpanded = expandedIds.has(node.id);
  const isSelected = selectedId === node.id;
  const hasChildren =
    node.type === "folder" && node.children && node.children.length > 0;

  const handleClick = () => {
    if (node.type === "folder") {
      toggleExpanded(node.id);
    }
    onSelect(node);
  };

  const handleKeyDown = (e: React.KeyboardEvent) => {
    if (e.key === "Enter" || e.key === " ") {
      e.preventDefault();
      handleClick();
    }
    if (e.key === "ArrowRight" && node.type === "folder" && !isExpanded) {
      e.preventDefault();
      toggleExpanded(node.id);
    }
    if (e.key === "ArrowLeft" && node.type === "folder" && isExpanded) {
      e.preventDefault();
      toggleExpanded(node.id);
    }
  };

  const getFileIcon = () => {
    if (node.icon) return node.icon;
    if (node.type === "folder") {
      return isExpanded ? (
        <FolderOpen className="h-4 w-4 text-tree-folder" />
      ) : (
        <Folder className="h-4 w-4 text-tree-folder" />
      );
    }
    return <File className="h-4 w-4 text-tree-file" />;
  };

  return (
    <li
      role="treeitem"
      aria-expanded={node.type === "folder" ? isExpanded : undefined}
      tabIndex={0}
    >
      <motion.div
        className={cn(
          "group relative flex cursor-pointer select-none items-center gap-1 rounded-md px-2 py-1.5 transition-colors",
          isSelected
            ? "bg-tree-selected-bg text-foreground"
            : "text-muted-foreground hover:bg-tree-hover hover:text-foreground",
        )}
        onClick={handleClick}
        onKeyDown={handleKeyDown}
        tabIndex={0}
        style={{ paddingLeft: `${level * 12 + 8}px` }}
        whileTap={{ scale: 0.98 }}
        layout
      >
        {/* Indent lines */}
        {level > 0 && (
          <div
            className="absolute top-0 left-0 h-full"
            style={{ width: `${level * 12}px` }}
          >
            {Array.from({ length: level }).map((_, i) => (
              <div
                key={i}
                className="absolute top-0 h-full w-px bg-tree-line opacity-30"
                style={{ left: `${i * 12 + 12}px` }}
              />
            ))}
          </div>
        )}

        {/* Chevron for folders */}
        {node.type === "folder" ? (
          <motion.div
            animate={{ rotate: isExpanded ? 90 : 0 }}
            transition={{ duration: 0.15, ease: "easeOut" }}
            className="flex h-4 w-4 shrink-0 items-center justify-center"
          >
            <ChevronRight className="h-3.5 w-3.5 text-muted-foreground" />
          </motion.div>
        ) : (
          <div className="h-4 w-4 shrink-0" />
        )}

        {/* Icon */}
        <motion.div
          className="shrink-0"
          initial={false}
          animate={{ scale: isSelected ? 1.1 : 1 }}
          transition={{ duration: 0.15 }}
        >
          {getFileIcon()}
        </motion.div>

        {/* Name */}
        <span className="truncate text-[13px]">{node.name}</span>

        {/* Selection indicator */}
        {isSelected && (
          <motion.div
            className="absolute inset-y-0 left-0 w-0.5 rounded-full bg-tree-selected"
            layoutId="selection-indicator"
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
          />
        )}
      </motion.div>

      {/* Children */}
      <AnimatePresence initial={false}>
        {hasChildren && isExpanded && (
          <motion.div
            initial={{ height: 0, opacity: 0 }}
            animate={{ height: "auto", opacity: 1 }}
            exit={{ height: 0, opacity: 0 }}
            transition={{
              height: { duration: 0.2, ease: [0.4, 0, 0.2, 1] },
              opacity: { duration: 0.15, ease: "easeOut" },
            }}
            style={{ overflow: "hidden" }}
          >
            <TreeNodeList
              nodes={node.children || []}
              level={level + 1}
              onSelect={onSelect}
            />
          </motion.div>
        )}
      </AnimatePresence>
    </li>
  );
}

export { FileTreeContext, useFileTree };
```
---

## GitHub Contributors <a name="github-contributors"></a>

GitHub contributors avatar grid for React. Displays contributor avatars with tooltips showing stats. Perfect for open-source project landing pages.

- **Registry Key**: `github-contributors`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/github-contributors.json
```

#### Dependencies
- **NPM Packages**:
  - `@radix-ui/react-tooltip`
  - `lucide-react`
  - `motion`

#### How to Use
```tsx
import { GitHubContributors } from "../ui/github-contributors";

export default function GitHubContributorsDemo() {
  return (
    <div className="flex flex-col gap-8">
      <div className="flex flex-col gap-3">
        <h3 className="font-medium text-sm">GitHub Contributors</h3>
        <GitHubContributors repo="vercel/next.js" limit={12} />
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/github-contributors.tsx`
```tsx
"use client";

import { ExternalLink } from "lucide-react";
import { motion } from "motion/react";
import { useEffect, useMemo, useState } from "react";
import { Card, CardContent, CardTitle } from "@/components/ui/card";
import { Skeleton } from "@/components/ui/skeleton";
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from "@/components/ui/tooltip";

interface Contributor {
  id: number;
  login: string;
  avatar_url: string;
  html_url: string;
  contributions: number;
}

interface GitHubContributorsProps {
  repo: string; // e.g. "vercel/next.js"
  limit?: number; // number of avatars to show (not counting the +more tile)
  className?: string;
  token?: string; // optional GitHub token to increase rate limit
}

export function GitHubContributors({
  repo,
  limit = 12,
  className = "",
  token,
}: GitHubContributorsProps) {
  const [contributors, setContributors] = useState<Contributor[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
  const [totalCount, setTotalCount] = useState<number | null>(null);

  useEffect(() => {
    if (!repo) return;
    setLoading(true);
    setError(null);
    setTotalCount(null);

    const headers: Record<string, string> = {
      Accept: "application/vnd.github.v3+json",
    };
    if (token) headers.Authorization = `token ${token}`;

    const fetchList = async () => {
      try {
        const listRes = await fetch(
          `https://api.github.com/repos/${repo}/contributors?per_page=${limit}`,
          { headers },
        );
        if (!listRes.ok)
          throw new Error(
            `GitHub API: ${listRes.status} ${listRes.statusText}`,
          );
        const listData: Contributor[] = await listRes.json();
        setContributors(listData.slice(0, limit));

        // Probe for total contributors (per_page=1 -> last page = total count)
        try {
          const probeRes = await fetch(
            `https://api.github.com/repos/${repo}/contributors?per_page=1`,
            { headers },
          );
          if (probeRes.ok) {
            const link = probeRes.headers.get("link");
            if (link) {
              const m = link.match(
                /<[^>]+[?&]page=(\d+)[^>]*>\s*;\s*rel="last"/,
              );
              if (m?.[1]) {
                const lastPage = parseInt(m[1], 10);
                if (Number.isFinite(lastPage)) setTotalCount(lastPage);
              }
            } else {
              const probeData = await probeRes.json();
              if (Array.isArray(probeData)) setTotalCount(probeData.length);
            }
          }
        } catch {
          // ignore probe errors
        }
      } catch (err: unknown) {
        setError((err as Error).message || "Failed to load contributors");
        setContributors([]);
      } finally {
        setLoading(false);
      }
    };

    fetchList();
  }, [repo, limit, token]);

  const shown = contributors.length;
  const remaining =
    totalCount !== null ? Math.max(0, totalCount - shown) : null;

  // compute max contributions among shown contributors to render the progress bar
  const maxContrib = useMemo(() => {
    if (!contributors || contributors.length === 0) return 1;
    return Math.max(...contributors.map((c) => c.contributions), 1);
  }, [contributors]);

  const repoUrl = `https://github.com/${repo}`;
  const contributorsUrl = `${repoUrl}/graphs/contributors`;

  return (
    <Card
      className={`overflow-hidden rounded-lg border bg-background shadow-sm ${className}`}
      aria-live="polite"
    >
      {/* Content */}
      <CardContent className="px-4 py-3">
        {error && (
          <p className="mb-2 text-destructive text-sm">Failed: {error}</p>
        )}

        {loading ? (
          <div className="grid grid-cols-5 gap-3 sm:grid-cols-8 md:grid-cols-10 lg:grid-cols-12">
            {Array.from({ length: limit }).map((_, i) => (
              <div key={i} className="flex items-center justify-center">
                <Skeleton className="h-10 w-10 rounded-full" />
              </div>
            ))}
          </div>
        ) : (
          <div className="grid grid-cols-5 items-center gap-3 sm:grid-cols-8 md:grid-cols-10 lg:grid-cols-11">
            {contributors.map((c, idx) => {
              const pct = Math.round((c.contributions / maxContrib) * 100);
              const isTop = idx === 0; // highlight the top contributor in shown list
              return (
                <div
                  key={c.id}
                  className="relative flex items-center justify-center"
                >
                  {/* top badge */}
                  {isTop && (
                    <div className="absolute -top-1 -right-1 z-10">
                      <div className="flex h-4 w-4 items-center justify-center rounded-full border border-white bg-yellow-400/90 font-semibold text-[10px] text-white shadow-sm">
                        <span>★</span>
                      </div>
                    </div>
                  )}

                  <Tooltip>
                    <TooltipTrigger asChild>
                      <motion.a
                        href={c.html_url}
                        target="_blank"
                        rel="noopener noreferrer"
                        title={`${c.login} — ${c.contributions} contributions`}
                        className="relative flex h-10 w-10 items-center justify-center overflow-hidden rounded-full border bg-muted/10 transition hover:bg-muted focus:outline-none focus:ring-2 focus:ring-ring"
                        whileHover={isTop ? { scale: 1.07 } : { scale: 1.04 }}
                        whileFocus={{ scale: 1.04 }}
                        onClick={(e) => e.stopPropagation()}
                        aria-label={`${c.login} GitHub profile`}
                      >
                        {/* biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx */}
                        <img
                          src={c.avatar_url}
                          alt={c.login}
                          width={40}
                          height={40}
                          className="h-full w-full object-cover"
                        />
                        {/* subtle ring on hover via pseudo element class; already handled by tailwind tokens */}
                      </motion.a>
                    </TooltipTrigger>

                    <TooltipContent
                      side="top"
                      align="center"
                      className="w-64 bg-transparent p-0 shadow-none"
                    >
                      <motion.div
                        initial={{ opacity: 0, scale: 0.96, y: 6 }}
                        animate={{ opacity: 1, scale: 1, y: 0 }}
                        transition={{ duration: 0.12 }}
                        className="rounded-lg border bg-popover p-3 text-popover-foreground shadow-md"
                        role="dialog"
                        aria-label={`${c.login} contributor details`}
                      >
                        <div className="flex items-center gap-3">
                          <div className="h-12 w-12 flex-shrink-0 overflow-hidden rounded-md border">
                            {/* biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx */}
                            <img
                              src={c.avatar_url}
                              alt={c.login}
                              width={48}
                              height={48}
                              className="object-cover"
                            />
                          </div>

                          <div className="min-w-0 flex-1">
                            <div className="flex items-center gap-2">
                              <div className="truncate font-medium text-foreground">
                                {c.login}
                              </div>
                              <div className="ml-auto font-mono text-muted-foreground text-xs">
                                #{c.id}
                              </div>
                            </div>

                            <div className="mt-1 text-muted-foreground text-xs">
                              {c.contributions.toLocaleString()} contributions
                            </div>

                            {/* mini contribution bar */}
                            <div className="mt-2">
                              <div className="h-2 w-full overflow-hidden rounded-full bg-muted">
                                <div
                                  className="h-2 rounded-full bg-primary transition-all duration-300"
                                  style={{ width: `${pct}%` }}
                                  aria-hidden
                                />
                              </div>
                              <div className="mt-1 text-[11px] text-muted-foreground">
                                {pct}% of top contributor
                              </div>
                            </div>
                          </div>
                        </div>

                        <div className="mt-3 flex items-center justify-between gap-2">
                          <a
                            href={c.html_url}
                            target="_blank"
                            rel="noopener noreferrer"
                            className="inline-flex items-center gap-2 rounded-md bg-secondary px-2 py-1 font-medium text-secondary-foreground text-sm transition-colors hover:bg-secondary/80"
                            onClick={(e) => e.stopPropagation()}
                            aria-label={`Open ${c.login} on GitHub`}
                          >
                            <ExternalLink className="h-4 w-4" />
                            <span className="font-medium text-sm">
                              View profile
                            </span>
                          </a>

                          <div className="text-muted-foreground text-xs">
                            Contributions:{" "}
                            <span className="font-medium text-foreground">
                              {c.contributions}
                            </span>
                          </div>
                        </div>
                      </motion.div>
                    </TooltipContent>
                  </Tooltip>
                </div>
              );
            })}

            {/* +N tile (or generic + tile) */}
            {remaining !== null && remaining > 0 ? (
              <a
                href={contributorsUrl}
                target="_blank"
                rel="noopener noreferrer"
                className="flex h-10 w-10 items-center justify-center rounded-full border bg-muted/5 text-muted-foreground text-xs transition hover:bg-muted"
                aria-label={`View all ${totalCount} contributors`}
              >
                +{remaining}
              </a>
            ) : remaining === null &&
              !loading &&
              contributors.length === limit ? (
              <a
                href={contributorsUrl}
                target="_blank"
                rel="noopener noreferrer"
                className="flex h-10 w-10 items-center justify-center rounded-full border bg-muted/5 text-muted-foreground text-xs transition hover:bg-muted"
                aria-label={`View more contributors`}
              >
                +
              </a>
            ) : null}
          </div>
        )}
      </CardContent>

      {/* Footer CTA */}
      <div className="flex items-center justify-between gap-3 border-t bg-background/50 px-4 py-3">
        <div className="min-w-0">
          <CardTitle className="m-0 truncate font-semibold text-sm">
            {repo}
          </CardTitle>
          <div className="text-muted-foreground text-xs">
            {loading ? (
              <span>Loading…</span>
            ) : error ? (
              <span className="text-destructive">Failed to load</span>
            ) : (
              <span>
                Showing <span className="font-medium">{shown}</span>
                {totalCount ? (
                  <>
                    {" "}
                    of <span className="font-medium">{totalCount}</span>
                  </>
                ) : shown === limit ? (
                  <> (more)</>
                ) : null}
              </span>
            )}
          </div>
        </div>

        <div className="flex items-center gap-2">
          <a
            href={repoUrl}
            target="_blank"
            rel="noopener noreferrer"
            className="inline-flex items-center gap-2 rounded-md px-2 py-1 font-medium text-xs transition hover:bg-muted"
            aria-label={`Open ${repo} on GitHub`}
          >
            <ExternalLink className="h-4 w-4" />
            <span>Open repo</span>
          </a>
        </div>
      </div>
    </Card>
  );
}
```
---

## GitHub Star Button <a name="github-star"></a>

Animated GitHub star counter for React. Real-time API data, rolling numbers, and confetti effects. Showcase your open-source project's popularity.

- **Registry Key**: `github-star`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/github-star.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
import { GitHubStarButton } from "@/registry/default/ui/github-star";

export default function GitHubStarDemo() {
  return (
    <div className="flex flex-col items-center justify-center gap-8 p-8">
      <div className="flex flex-wrap items-center justify-center gap-6">
        <GitHubStarButton owner="johuniq" repo="jolyui" stars={120} />
        <GitHubStarButton owner="facebook" repo="react" />
        <GitHubStarButton owner="vercel" repo="next.js" />
      </div>

      <div className="space-y-2 text-center">
        <p className="text-muted-foreground text-sm">
          Real-time GitHub stars with premium rolling animations and particle
          effects.
        </p>
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/github-star.tsx`
```tsx
"use client";

import { AnimatePresence, motion } from "motion/react";
import * as React from "react";
import { cn } from "@/lib/utils";

export interface GitHubStarButtonProps {
  /**
   * The owner of the GitHub repository
   * @example "johuniq"
   */
  owner: string;
  /**
   * The name of the GitHub repository
   * @example "jolyui"
   */
  repo: string;
  /**
   * Manual star count override. If provided, the component will not fetch from GitHub API.
   */
  stars?: number;
  /**
   * Additional CSS classes for the button
   */
  className?: string;
}

interface Particle {
  id: number;
  x: number;
  y: number;
  angle: number;
  scale: number;
}

const StarIcon = ({
  className,
  filled,
}: {
  className?: string;
  filled?: boolean;
}) => (
  <svg
    viewBox="0 0 16 16"
    className={className}
    fill={filled ? "currentColor" : "none"}
    stroke="currentColor"
    strokeWidth={filled ? 0 : 1.5}
    strokeLinecap="round"
    strokeLinejoin="round"
  >
    <path d="M8 1.5l1.85 4.1 4.65.55-3.5 3.15.95 4.6L8 11.7l-4 2.2.95-4.6-3.5-3.15 4.65-.55L8 1.5z" />
  </svg>
);

const GitHubIcon = ({ className }: { className?: string }) => (
  <svg viewBox="0 0 16 16" className={className} fill="currentColor">
    <path d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0016 8c0-4.42-3.58-8-8-8z" />
  </svg>
);

function formatNumber(num: number): string {
  if (num >= 1000000) {
    return `${(num / 1000000).toFixed(1).replace(/\.0$/, "")}M`;
  }
  if (num >= 1000) {
    return `${(num / 1000).toFixed(1).replace(/\.0$/, "")}k`;
  }
  return num.toLocaleString();
}

function useGitHubStars(owner: string, repo: string, manualStars?: number) {
  const [stars, setStars] = React.useState<number>(manualStars ?? 0);
  const [loading, setLoading] = React.useState(!manualStars);

  React.useEffect(() => {
    if (manualStars !== undefined) {
      setStars(manualStars);
      setLoading(false);
      return;
    }

    fetch(`https://api.github.com/repos/${owner}/${repo}`)
      .then((response) => response.json())
      .then((data) => {
        if (data && typeof data.stargazers_count === "number") {
          setStars(data.stargazers_count);
        }
      })
      .catch(console.error)
      .finally(() => setLoading(false));
  }, [owner, repo, manualStars]);

  return { stars, loading };
}

const AnimatedDigit = ({ digit }: { digit: string }) => {
  const isNumber = /\d/.test(digit);

  if (!isNumber) {
    return <span className="inline-block px-0.5">{digit}</span>;
  }

  const num = parseInt(digit, 10);

  return (
    <span className="relative inline-block h-[1em] w-[0.6em] overflow-hidden">
      <motion.span
        className="absolute top-0 left-0 flex w-full flex-col items-center"
        initial={{ y: 0 }}
        animate={{ y: `${-num * 10}%` }}
        transition={{
          type: "spring",
          stiffness: 100,
          damping: 15,
          mass: 1,
        }}
      >
        {[0, 1, 2, 3, 4, 5, 6, 7, 8, 9].map((n) => (
          <span
            key={n}
            className="flex h-[1em] items-center justify-center leading-none"
          >
            {n}
          </span>
        ))}
      </motion.span>
    </span>
  );
};

function RollingNumber({ value }: { value: number }) {
  const formatted = formatNumber(value);
  const digits = formatted.split("");

  return (
    <div className="flex items-center">
      {digits.map((digit, i) => (
        <AnimatedDigit key={`${i}-${digit}`} digit={digit} />
      ))}
    </div>
  );
}

function StarParticles({ particles }: { particles: Particle[] }) {
  return (
    <AnimatePresence>
      {particles.map((particle) => (
        <motion.div
          key={particle.id}
          className="pointer-events-none absolute"
          initial={{
            opacity: 1,
            scale: particle.scale,
            x: 0,
            y: 0,
          }}
          animate={{
            opacity: 0,
            scale: 0,
            x: Math.cos(particle.angle) * 60,
            y: Math.sin(particle.angle) * 60,
          }}
          exit={{ opacity: 0 }}
          transition={{ duration: 0.7, ease: [0.32, 0.72, 0, 1] }}
          style={{
            left: particle.x,
            top: particle.y,
          }}
        >
          <StarIcon className="h-3 w-3 text-star" filled />
        </motion.div>
      ))}
    </AnimatePresence>
  );
}

export function GitHubStarButton({
  owner,
  repo,
  stars: manualStars,
  className,
}: GitHubStarButtonProps) {
  const { stars, loading } = useGitHubStars(owner, repo, manualStars);
  const [localStars, setLocalStars] = React.useState(0);
  const [isHovered, setIsHovered] = React.useState(false);
  const [isStarred, setIsStarred] = React.useState(false);
  const [particles, setParticles] = React.useState<Particle[]>([]);
  const buttonRef = React.useRef<HTMLAnchorElement>(null);

  React.useEffect(() => {
    if (stars > 0) {
      setLocalStars(stars);
    }
  }, [stars]);

  const handleClick = (e: React.MouseEvent) => {
    if (!isStarred) {
      e.preventDefault();
      setIsStarred(true);
      setLocalStars((prev) => prev + 1);

      const centerX = 20;
      const centerY = 20;

      const newParticles: Particle[] = Array.from({ length: 12 }, (_, i) => ({
        id: Date.now() + i,
        x: centerX,
        y: centerY,
        angle: (Math.PI * 2 * i) / 12 + Math.random() * 0.3,
        scale: 0.4 + Math.random() * 0.6,
      }));

      setParticles(newParticles);
      setTimeout(() => setParticles([]), 800);

      // After simulation, navigate to the repo
      setTimeout(() => {
        window.open(`https://github.com/${owner}/${repo}`, "_blank");
      }, 600);
    }
  };

  const repoUrl = `https://github.com/${owner}/${repo}`;

  if (loading && localStars === 0) {
    return (
      <div
        className={cn(
          "relative inline-flex animate-pulse items-center gap-3 rounded-xl border border-border bg-card px-4 py-2.5",
          className,
        )}
      >
        <div className="h-5 w-5 rounded-full bg-muted" />
        <div className="h-5 w-px bg-border" />
        <div className="h-5 w-20 rounded bg-muted" />
      </div>
    );
  }

  return (
    <motion.a
      ref={buttonRef}
      href={repoUrl}
      target="_blank"
      rel="noopener noreferrer"
      className={cn(
        "relative inline-flex items-center gap-3 rounded-xl px-4 py-2.5",
        "border border-border bg-card",
        "shadow-sm transition-all duration-300 hover:shadow-lg",
        "group cursor-pointer no-underline",
        className,
      )}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
      onClick={handleClick}
      whileHover={{ scale: 1.03, y: -2 }}
      whileTap={{ scale: 0.97 }}
      transition={{ type: "spring", stiffness: 400, damping: 17 }}
    >
      {/* GitHub Icon */}
      <GitHubIcon className="h-5 w-5 text-foreground transition-colors" />

      {/* Divider */}
      <div className="h-5 w-px bg-border" />

      {/* Star Section */}
      <div className="relative flex items-center gap-2">
        <StarParticles particles={particles} />

        <motion.div
          className="relative"
          animate={{
            rotate: isHovered || isStarred ? [0, -15, 15, -10, 10, 0] : 0,
            scale: isHovered || isStarred ? 1.2 : 1,
          }}
          transition={{
            rotate: { duration: 0.5, ease: "easeInOut" },
            scale: { type: "spring", stiffness: 300, damping: 15 },
          }}
        >
          <StarIcon
            className={cn(
              "h-5 w-5 transition-colors duration-300",
              isHovered || isStarred ? "text-star" : "text-muted-foreground",
            )}
            filled={isHovered || isStarred}
          />

          {/* Glow effect */}
          <motion.div
            className="absolute inset-0 blur-lg"
            initial={{ opacity: 0 }}
            animate={{ opacity: isHovered || isStarred ? 0.8 : 0 }}
            transition={{ duration: 0.3 }}
          >
            <StarIcon className="h-5 w-5 text-star-glow" filled />
          </motion.div>
        </motion.div>

        {/* Divider */}

        {/* Count */}
        <div className="min-w-[3rem] font-mono font-semibold text-foreground text-sm tabular-nums">
          <RollingNumber value={localStars} />
        </div>
      </div>

      {/* Hover shimmer effect */}
      <motion.div
        className="pointer-events-none absolute inset-0 overflow-hidden rounded-xl"
        initial={{ opacity: 0 }}
        animate={{ opacity: isHovered ? 1 : 0 }}
        transition={{ duration: 0.3 }}
      >
        <motion.div
          className="absolute inset-0 bg-gradient-to-r from-transparent via-star/10 to-transparent"
          animate={{
            x: isHovered ? ["100%", "-100%"] : "100%",
          }}
          transition={{
            duration: 1.2,
            repeat: isHovered ? Infinity : 0,
            repeatDelay: 0.8,
            ease: "easeInOut",
          }}
        />
      </motion.div>
    </motion.a>
  );
}

export default GitHubStarButton;
```
---

## Glitch Text <a name="glitch-text"></a>

Cyberpunk-style glitch text effect for React. Digital distortion animation with character scrambling. Perfect for hero sections and tech websites.

- **Registry Key**: `glitch-text`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/glitch-text.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
"use client";

import { GlitchText } from "@/registry/default/ui/glitch-text";

export default function GlitchTextDemo() {
  return (
    <div className="flex items-center justify-center">
      <GlitchText
        words={["Hello", "World", "Glitch", "Effect"]}
        className="font-bold text-4xl text-foreground"
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/glitch-text.tsx`
```tsx
import * as React from "react";
import { cn } from "@/lib/utils";

interface GlitchTextProps {
  words: string[];
  className?: string;
  interval?: number;
  glitchDuration?: number;
}

const GlitchText = React.forwardRef<HTMLSpanElement, GlitchTextProps>(
  ({ words, className, interval = 3000, glitchDuration = 300 }, ref) => {
    const [currentIndex, setCurrentIndex] = React.useState(0);
    const [isGlitching, setIsGlitching] = React.useState(false);
    const [displayText, setDisplayText] = React.useState(words[0] || "");

    const glitchChars = "!<>-_\\/[]{}—=+*^?#________";

    React.useEffect(() => {
      const timer = setInterval(() => {
        setIsGlitching(true);

        const nextIndex = (currentIndex + 1) % words.length;
        const targetWord = words[nextIndex] || "";
        const iterations = 10;
        let iteration = 0;

        const glitchInterval = setInterval(() => {
          setDisplayText(
            targetWord
              .split("")
              .map((char, i) => {
                if (i < iteration) return char;
                return glitchChars[
                  Math.floor(Math.random() * glitchChars.length)
                ];
              })
              .join(""),
          );

          iteration += 1;
          if (iteration > targetWord.length) {
            clearInterval(glitchInterval);
            setDisplayText(targetWord);
            setCurrentIndex(nextIndex);
            setIsGlitching(false);
          }
        }, glitchDuration / iterations);
      }, interval);

      return () => clearInterval(timer);
    }, [currentIndex, words, interval, glitchDuration]);

    return (
      <span
        ref={ref}
        className={cn(
          "inline-block font-mono",
          isGlitching && "text-primary",
          className,
        )}
      >
        {displayText}
      </span>
    );
  },
);
GlitchText.displayName = "GlitchText";

export { GlitchText };
```
---

## Glowing Button <a name="glowing-button"></a>

- **Registry Key**: `glowing-button`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/glowing-button.json
```

#### Dependencies
- **NPM Packages**:
  - `@radix-ui/react-slot`
  - `class-variance-authority`
  - `motion`

#### How to Use
```tsx
import { GlowingButton } from "@/registry/default/ui/glowing-button";

export default function GlowingButtonDemo() {
  return (
    <div className="flex flex-wrap items-center gap-4">
      <GlowingButton>Get Started</GlowingButton>
      <GlowingButton variant="purple">Learn More</GlowingButton>
      <GlowingButton variant="pink">Try Now</GlowingButton>
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/glowing-button.tsx`
```tsx
"use client";

import { Slot } from "@radix-ui/react-slot";
import { type VariantProps, cva } from "class-variance-authority";
import { motion, useMotionTemplate, useMotionValue } from "motion/react";
import * as React from "react";
import { cn } from "@/lib/utils";

const glowingButtonVariants = cva(
  "relative inline-flex items-center justify-center whitespace-nowrap rounded-full text-sm font-semibold transition-colors focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:pointer-events-none disabled:opacity-50",
  {
    variants: {
      variant: {
        default:
          "bg-gradient-to-br from-blue-400 to-blue-600 text-white hover:from-blue-500 hover:to-blue-700",
        purple:
          "bg-gradient-to-br from-purple-400 to-purple-600 text-white hover:from-purple-500 hover:to-purple-700",
        pink: "bg-gradient-to-br from-pink-400 to-pink-600 text-white hover:from-pink-500 hover:to-pink-700",
        green:
          "bg-gradient-to-br from-green-400 to-green-600 text-white hover:from-green-500 hover:to-green-700",
        orange:
          "bg-gradient-to-br from-orange-400 to-orange-600 text-white hover:from-orange-500 hover:to-orange-700",
        rainbow:
          "bg-gradient-to-br from-purple-400 via-pink-500 to-orange-500 text-white hover:from-purple-500 hover:via-pink-600 hover:to-orange-600",
      },
      size: {
        default: "h-9 px-4 py-2",
        sm: "h-8 px-3 text-xs",
        lg: "h-11 px-8 text-base",
        icon: "h-9 w-9",
      },
      glow: {
        none: "",
        subtle: "",
        medium: "",
        intense: "",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
      glow: "medium",
    },
  }
);

const glowColors = {
  default: "rgba(96, 165, 250, 0.4)",
  purple: "rgba(192, 132, 252, 0.4)",
  pink: "rgba(244, 114, 182, 0.4)",
  green: "rgba(74, 222, 128, 0.4)",
  orange: "rgba(251, 146, 60, 0.4)",
  rainbow: "rgba(192, 132, 252, 0.4)",
};

export interface GlowingButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof glowingButtonVariants> {
  asChild?: boolean;
}

const GlowingButton = React.forwardRef<HTMLButtonElement, GlowingButtonProps>(
  (
    { className, variant = "default", size, glow = "medium", asChild = false, ...props },
    ref
  ) => {
    const Comp = asChild ? Slot : "button";
    const mouseX = useMotionValue(0);
    const mouseY = useMotionValue(0);

    const handleMouseMove = React.useCallback(
      (e: React.MouseEvent<HTMLDivElement>) => {
        const { left, top } = e.currentTarget.getBoundingClientRect();
        mouseX.set(e.clientX - left);
        mouseY.set(e.clientY - top);
      },
      [mouseX, mouseY]
    );

    const glowOpacity = {
      none: 0,
      subtle: 0.3,
      medium: 0.4,
      intense: 0.6,
    }[glow || "medium"];

    const glowBlur = {
      none: "0px",
      subtle: "8px",
      medium: "16px",
      intense: "24px",
    }[glow || "medium"];

    const glowSpread = {
      none: "0px",
      subtle: "4px",
      medium: "8px",
      intense: "12px",
    }[glow || "medium"];

    const background = useMotionTemplate`radial-gradient(circle at ${mouseX}px ${mouseY}px, ${glowColors[variant || "default"]}, transparent 80%)`;

    return (
      <div
        className="relative inline-flex group"
        onMouseMove={handleMouseMove}
      >
        {glow !== "none" && (
          <>
            {/* Static glow */}
            <div
              className="absolute inset-0 rounded-full opacity-40 blur-lg pointer-events-none transition-all duration-300 ease-out group-hover:opacity-60 group-hover:blur-xl"
              style={{
                backgroundColor: glowColors[variant || "default"],
                margin: `-${glowSpread}`,
                filter: `blur(${glowBlur})`,
                opacity: glowOpacity,
              }}
            />
            {/* Dynamic glow on hover */}
            <motion.div
              className="absolute inset-0 rounded-full pointer-events-none opacity-0 group-hover:opacity-100 transition-opacity duration-300"
              style={{
                background,
                margin: `-${glowSpread}`,
                filter: `blur(${glowBlur})`,
              }}
            />
          </>
        )}
        <Comp
          className={cn(glowingButtonVariants({ variant, size, className }))}
          ref={ref}
          {...props}
        />
      </div>
    );
  }
);
GlowingButton.displayName = "GlowingButton";

export { GlowingButton, glowingButtonVariants };
```
---

## Gooey Text Morphing <a name="gooey-text-morphing"></a>

Liquid gooey text morph effect for React. SVG filter-based animation with smooth blob-like transitions. Unique eye-catching text animation.

- **Registry Key**: `gooey-text-morphing`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/gooey-text-morphing.json
```

#### How to Use
```tsx
"use client";

import { GooeyText } from "@/registry/default/ui/gooey-text-morphing";

export default function GooeyTextMorphingDemo() {
  const texts = ["Why", "is", "it", "so", "satisfying", "to", "watch?"];

  return (
    <div className="flex h-[300px] w-full items-center justify-center overflow-hidden rounded-xl border bg-background">
      <GooeyText
        texts={texts}
        morphTime={1}
        cooldownTime={0.5}
        textClassName="font-bold tracking-tighter"
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/gooey-text-morphing.tsx`
```tsx
"use client";

import * as React from "react";
import { cn } from "@/lib/utils";

interface GooeyTextProps {
  texts: string[];
  morphTime?: number;
  cooldownTime?: number;
  className?: string;
  textClassName?: string;
}

export function GooeyText({
  texts,
  morphTime = 1,
  cooldownTime = 0.25,
  className,
  textClassName,
}: GooeyTextProps) {
  const text1Ref = React.useRef<HTMLSpanElement>(null);
  const text2Ref = React.useRef<HTMLSpanElement>(null);

  React.useEffect(() => {
    let textIndex = texts.length - 1;
    let time = new Date();
    let morph = 0;
    let cooldown = cooldownTime;

    const setMorph = (fraction: number) => {
      if (text1Ref.current && text2Ref.current) {
        text2Ref.current.style.filter = `blur(${Math.min(8 / fraction - 8, 100)}px)`;
        text2Ref.current.style.opacity = `${fraction ** 0.4 * 100}%`;

        fraction = 1 - fraction;
        text1Ref.current.style.filter = `blur(${Math.min(8 / fraction - 8, 100)}px)`;
        text1Ref.current.style.opacity = `${fraction ** 0.4 * 100}%`;
      }
    };

    const doCooldown = () => {
      morph = 0;
      if (text1Ref.current && text2Ref.current) {
        text2Ref.current.style.filter = "";
        text2Ref.current.style.opacity = "100%";
        text1Ref.current.style.filter = "";
        text1Ref.current.style.opacity = "0%";
      }
    };

    const doMorph = () => {
      morph -= cooldown;
      cooldown = 0;
      let fraction = morph / morphTime;

      if (fraction > 1) {
        cooldown = cooldownTime;
        fraction = 1;
      }

      setMorph(fraction);
    };

    function animate() {
      requestAnimationFrame(animate);
      const newTime = new Date();
      const shouldIncrementIndex = cooldown > 0;
      const dt = (newTime.getTime() - time.getTime()) / 1000;
      time = newTime;

      cooldown -= dt;

      if (cooldown <= 0) {
        if (shouldIncrementIndex) {
          textIndex = (textIndex + 1) % texts.length;
          if (text1Ref.current && text2Ref.current) {
            text1Ref.current.textContent =
              texts[textIndex % texts.length] ?? "";
            text2Ref.current.textContent =
              texts[(textIndex + 1) % texts.length] ?? "";
          }
        }
        doMorph();
      } else {
        doCooldown();
      }
    }

    animate();

    return () => {
      // Cleanup function if needed
    };
  }, [texts, morphTime, cooldownTime]);

  return (
    <div className={cn("relative", className)}>
      <svg className="absolute h-0 w-0" aria-hidden="true" focusable="false">
        <defs>
          <filter id="threshold">
            <feColorMatrix
              in="SourceGraphic"
              type="matrix"
              values="1 0 0 0 0
                      0 1 0 0 0
                      0 0 1 0 0
                      0 0 0 255 -140"
            />
          </filter>
        </defs>
      </svg>

      <div
        className="flex items-center justify-center"
        style={{ filter: "url(#threshold)" }}
      >
        <span
          ref={text1Ref}
          className={cn(
            "absolute inline-block select-none text-center text-6xl md:text-[60pt]",
            "text-foreground",
            textClassName,
          )}
        />
        <span
          ref={text2Ref}
          className={cn(
            "absolute inline-block select-none text-center text-6xl md:text-[60pt]",
            "text-foreground",
            textClassName,
          )}
        />
      </div>
    </div>
  );
}
```
---

## Hero Banner <a name="hero-banner"></a>

- **Registry Key**: `hero-banner`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/hero-banner.json
```

#### How to Use
```tsx
"use client";

import ResponsiveHeroBanner from "@/registry/default/ui/hero-banner";

export default function HeroBannerDemo() {
  return <ResponsiveHeroBanner />;
}
```
#### Component Source Code
##### File: `ui/hero-banner.tsx`
```tsx
"use client";

import React, { useState } from 'react';

interface NavLink {
    label: string;
    href: string;
    isActive?: boolean;
}

interface Partner {
    logoUrl: string;
    href: string;
}

interface ResponsiveHeroBannerProps {
    logoUrl?: string;
    backgroundImageUrl?: string;
    navLinks?: NavLink[];
    ctaButtonText?: string;
    ctaButtonHref?: string;
    badgeText?: string;
    badgeLabel?: string;
    title?: string;
    titleLine2?: string;
    description?: string;
    primaryButtonText?: string;
    primaryButtonHref?: string;
    secondaryButtonText?: string;
    secondaryButtonHref?: string;
    partnersTitle?: string;
    partners?: Partner[];
}

const ResponsiveHeroBanner: React.FC<ResponsiveHeroBannerProps> = ({
    logoUrl = "https://hoirqrkdgbmvpwutwuwj.supabase.co/storage/v1/object/public/assets/assets/febf2421-4a9a-42d6-871d-ff4f9518021c_1600w.png",
    backgroundImageUrl = "https://hoirqrkdgbmvpwutwuwj.supabase.co/storage/v1/object/public/assets/assets/0e2dbea0-c0a9-413f-a57b-af279633c0df_3840w.jpg",
    navLinks = [
        { label: "Home", href: "#", isActive: true },
        { label: "Missions", href: "#" },
        { label: "Destinations", href: "#" },
        { label: "Technology", href: "#" },
        { label: "Book Flight", href: "#" }
    ],
    ctaButtonText = "Reserve Seat",
    ctaButtonHref = "#",
    badgeLabel = "New",
    badgeText = "First Commercial Flight to Mars 2026",
    title = "Journey Beyond Earth",
    titleLine2 = "Into the Cosmos",
    description = "Experience the cosmos like never before. Our advanced spacecraft and cutting-edge technology make interplanetary travel accessible, safe, and unforgettable.",
    primaryButtonText = "Book Your Journey",
    primaryButtonHref = "#",
    secondaryButtonText = "Watch Launch",
    secondaryButtonHref = "#",
    partnersTitle = "Partnering with leading space agencies worldwide",
    partners = [
        { logoUrl: "https://hoirqrkdgbmvpwutwuwj.supabase.co/storage/v1/object/public/assets/assets/f7466370-2832-4fdd-84c2-0932bb0dd850_800w.png", href: "#" },
        { logoUrl: "https://hoirqrkdgbmvpwutwuwj.supabase.co/storage/v1/object/public/assets/assets/0a9a71ec-268b-4689-a510-56f57e9d4f13_1600w.png", href: "#" },
        { logoUrl: "https://hoirqrkdgbmvpwutwuwj.supabase.co/storage/v1/object/public/assets/assets/a9ed4369-748a-49f8-9995-55d6c876bbff_1600w.png", href: "#" },
        { logoUrl: "https://hoirqrkdgbmvpwutwuwj.supabase.co/storage/v1/object/public/assets/assets/0d8966a4-8525-4e11-9d5d-2d7390b2c798_1600w.png", href: "#" },
        { logoUrl: "https://hoirqrkdgbmvpwutwuwj.supabase.co/storage/v1/object/public/assets/assets/2ed33c8b-b8b2-4176-967f-3d785fed07d8_1600w.png", href: "#" }
    ]
}) => {
    const [mobileMenuOpen, setMobileMenuOpen] = useState(false);

    return (
        <section className="w-full isolate min-h-screen overflow-hidden relative">
            <img
                src={backgroundImageUrl}
                alt=""
                className="w-full h-full object-cover absolute top-0 right-0 bottom-0 left-0"
            />
            <div className="pointer-events-none absolute inset-0 ring-1 ring-black/30" />

            <header className="z-10 xl:top-4 relative">
                <div className="mx-6">
                    <div className="flex items-center justify-between pt-4">
                        <a
                            href="#"
                            className="inline-flex items-center justify-center bg-center w-[100px] h-[40px] bg-cover rounded"
                            style={{ backgroundImage: `url(${logoUrl})` }}
                        />

                        <nav className="hidden md:flex items-center gap-2">
                            <div className="flex items-center gap-1 rounded-full bg-white/5 px-1 py-1 ring-1 ring-white/10 backdrop-blur">
                                {navLinks.map((link, index) => (
                                    <a
                                        key={index}
                                        href={link.href}
                                        className={`px-3 py-2 text-sm font-medium hover:text-white font-sans transition-colors ${link.isActive ? 'text-white/90' : 'text-white/80'
                                            }`}
                                    >
                                        {link.label}
                                    </a>
                                ))}
                                <a
                                    href={ctaButtonHref}
                                    className="ml-1 inline-flex items-center gap-2 rounded-full bg-white px-3.5 py-2 text-sm font-medium text-neutral-900 hover:bg-white/90 font-sans transition-colors"
                                >
                                    {ctaButtonText}
                                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className="h-4 w-4">
                                        <path d="M7 7h10v10" />
                                        <path d="M7 17 17 7" />
                                    </svg>
                                </a>
                            </div>
                        </nav>

                        <button
                            onClick={() => setMobileMenuOpen(!mobileMenuOpen)}
                            className="md:hidden inline-flex h-10 w-10 items-center justify-center rounded-full bg-white/10 ring-1 ring-white/15 backdrop-blur"
                            aria-expanded={mobileMenuOpen}
                            aria-label="Toggle menu"
                        >
                            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className="h-5 w-5 text-white/90">
                                <path d="M4 5h16" />
                                <path d="M4 12h16" />
                                <path d="M4 19h16" />
                            </svg>
                        </button>
                    </div>
                </div>
            </header>

            <div className="z-10 relative">
                <div className="sm:pt-28 md:pt-32 lg:pt-40 max-w-7xl mx-auto pt-28 px-6 pb-16">
                    <div className="mx-auto max-w-3xl text-center">
                        <div className="mb-6 inline-flex items-center gap-3 rounded-full bg-white/10 px-2.5 py-2 ring-1 ring-white/15 backdrop-blur animate-fade-slide-in-1">
                            <span className="inline-flex items-center text-xs font-medium text-neutral-900 bg-white/90 rounded-full py-0.5 px-2 font-sans">
                                {badgeLabel}
                            </span>
                            <span className="text-sm font-medium text-white/90 font-sans">
                                {badgeText}
                            </span>
                        </div>

                        <h1 className="sm:text-5xl md:text-6xl lg:text-7xl leading-tight text-4xl text-white tracking-tight font-instrument-serif font-normal animate-fade-slide-in-2">
                            {title}
                            <br className="hidden sm:block" />
                            {titleLine2}
                        </h1>

                        <p className="sm:text-lg animate-fade-slide-in-3 text-base text-white/80 max-w-2xl mt-6 mx-auto">
                            {description}
                        </p>

                        <div className="flex flex-col sm:flex-row sm:gap-4 mt-10 gap-3 items-center justify-center animate-fade-slide-in-4">
                            <a
                                href={primaryButtonHref}
                                className="inline-flex items-center gap-2 hover:bg-white/15 text-sm font-medium text-white bg-white/10 ring-white/15 ring-1 rounded-full py-3 px-5 font-sans transition-colors"
                            >
                                {primaryButtonText}
                                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className="h-4 w-4">
                                    <path d="M5 12h14" />
                                    <path d="m12 5 7 7-7 7" />
                                </svg>
                            </a>
                            <a
                                href={secondaryButtonHref}
                                className="inline-flex items-center gap-2 rounded-full bg-transparent px-5 py-3 text-sm font-medium text-white/90 hover:text-white font-sans transition-colors"
                            >
                                {secondaryButtonText}
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className="w-4 h-4">
                                    <path d="M5 5a2 2 0 0 1 3.008-1.728l11.997 6.998a2 2 0 0 1 .003 3.458l-12 7A2 2 0 0 1 5 19z" />
                                </svg>
                            </a>
                        </div>
                    </div>

                    <div className="mx-auto mt-20 max-w-5xl">
                        <p className="animate-fade-slide-in-1 text-sm text-white/70 text-center">
                            {partnersTitle}
                        </p>
                        <div className="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-5 animate-fade-slide-in-2 text-white/70 mt-6 items-center justify-items-center gap-4">
                            {partners.map((partner, index) => (
                                <a
                                    key={index}
                                    href={partner.href}
                                    className="inline-flex items-center justify-center bg-center w-[120px] h-[36px] bg-cover rounded-full opacity-80 hover:opacity-100 transition-opacity"
                                    style={{ backgroundImage: `url(${partner.logoUrl})` }}
                                />
                            ))}
                        </div>
                    </div>
                </div>
            </div>
        </section>
    );
};

export default ResponsiveHeroBanner;
```
---

## Highlight Text <a name="highlight-text"></a>

Animated text highlighter for React. SVG underlines, boxes, circles, and marker effects. Draw attention to key words with hand-drawn style animations.

- **Registry Key**: `highlight-text`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/highlight-text.json
```

#### Dependencies
- **NPM Packages**:
  - `class-variance-authority`

#### How to Use
```tsx
import { HighlightText } from "@/registry/default/ui/highlight-text";

export default function HighlightTextDemo() {
  return (
    <div className="flex flex-col items-center justify-center py-20">
      <h1 className="max-w-2xl text-center font-bold text-4xl leading-relaxed tracking-tight">
        Make your text <HighlightText>stand out</HighlightText> with animated
        highlights.
      </h1>
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/highlight-text.tsx`
```tsx
import { cva } from "class-variance-authority";
import * as React from "react";
import { cn } from "@/lib/utils";

const highlightVariants = cva("relative inline-block", {
  variants: {
    variant: {
      underline: "",
      box: "",
      circle: "",
      marker: "",
    },
    color: {
      primary:
        "[--highlight-color:hsl(var(--highlight-primary))] [&_path]:[stroke:var(--highlight-color)]",
      secondary:
        "[--highlight-color:hsl(var(--highlight-secondary))] [&_path]:[stroke:var(--highlight-color)]",
      accent:
        "[--highlight-color:hsl(var(--highlight-accent))] [&_path]:[stroke:var(--highlight-color)]",
      destructive:
        "[--highlight-color:hsl(var(--highlight-destructive))] [&_path]:[stroke:var(--highlight-color)]",
    },
  },
  defaultVariants: {
    variant: "underline",
    color: "primary",
  },
});

type HighlightColor = "primary" | "secondary" | "accent" | "destructive";
type HighlightVariant = "underline" | "box" | "circle" | "marker";

export interface HighlightTextProps
  extends Omit<React.HTMLAttributes<HTMLSpanElement>, "color"> {
  children: React.ReactNode;
  variant?: HighlightVariant;
  color?: HighlightColor;
  animationDuration?: number;
  animationDelay?: number;
  strokeWidth?: number;
  animate?: boolean;
}

const HighlightText = React.forwardRef<HTMLSpanElement, HighlightTextProps>(
  (
    {
      className,
      variant = "underline",
      color = "primary",
      children,
      animationDuration = 0.8,
      animationDelay = 0,
      strokeWidth = 2,
      animate = true,
      ...props
    },
    ref,
  ) => {
    const [isVisible, setIsVisible] = React.useState(!animate);
    const [dimensions, setDimensions] = React.useState({ width: 0, height: 0 });
    const containerRef = React.useRef<HTMLSpanElement>(null);

    React.useEffect(() => {
      const element = containerRef.current;
      if (!element) return;

      const updateDimensions = () => {
        setDimensions({
          width: element.offsetWidth,
          height: element.offsetHeight,
        });
      };

      updateDimensions();

      const resizeObserver = new ResizeObserver(updateDimensions);
      resizeObserver.observe(element);

      return () => resizeObserver.disconnect();
    }, []);

    React.useEffect(() => {
      if (!animate) {
        setIsVisible(true);
        return;
      }

      const element = containerRef.current;
      if (!element) return;

      const observer = new IntersectionObserver(
        ([entry]) => {
          if (entry?.isIntersecting) {
            setIsVisible(true);
            observer.disconnect();
          }
        },
        { threshold: 0.5, rootMargin: "0px 0px -50px 0px" },
      );

      observer.observe(element);
      return () => observer.disconnect();
    }, [animate]);

    const renderHighlight = () => {
      const { width, height } = dimensions;
      if (width === 0 || height === 0) return null;

      const padding = 8;
      const svgWidth = width + padding * 2;
      const svgHeight = height + padding * 2;

      const baseStyles: React.CSSProperties = {
        position: "absolute",
        top: -padding,
        left: -padding,
        width: svgWidth,
        height: svgHeight,
        pointerEvents: "none",
        overflow: "visible",
      };

      const pathStyles: React.CSSProperties = {
        fill: "none",
        stroke: "var(--highlight-color)",
        strokeWidth,
        strokeLinecap: "round",
        strokeLinejoin: "round",
        transition: `stroke-dashoffset ${animationDuration}s cubic-bezier(0.65, 0, 0.35, 1) ${animationDelay}s`,
      };

      switch (variant) {
        case "underline": {
          const y = svgHeight - padding + 2;
          const pathLength = width + 10;
          const d = `M ${padding - 2} ${y} Q ${padding + width * 0.25} ${y - 3} ${padding + width * 0.5} ${y} T ${padding + width + 2} ${y}`;

          return (
            <svg style={baseStyles} aria-hidden="true">
              <path
                d={d}
                style={{
                  ...pathStyles,
                  strokeDasharray: pathLength,
                  strokeDashoffset: isVisible ? 0 : pathLength,
                }}
              />
            </svg>
          );
        }

        case "box": {
          const boxPadding = 4;
          const pathLength = (width + height + boxPadding * 2) * 2 + 20;
          const d = `
            M ${padding - boxPadding} ${padding - boxPadding + 2}
            L ${padding + width + boxPadding} ${padding - boxPadding}
            L ${padding + width + boxPadding + 2} ${padding + height + boxPadding}
            L ${padding - boxPadding + 1} ${padding + height + boxPadding + 2}
            Z
          `;

          return (
            <svg style={baseStyles} aria-hidden="true">
              <path
                d={d}
                style={{
                  ...pathStyles,
                  strokeDasharray: pathLength,
                  strokeDashoffset: isVisible ? 0 : pathLength,
                }}
              />
            </svg>
          );
        }

        case "circle": {
          const cx = padding + width / 2;
          const cy = padding + height / 2;
          const rx = width / 2 + 6;
          const ry = height / 2 + 6;
          const pathLength = Math.PI * 2 * Math.max(rx, ry);

          // Hand-drawn ellipse using bezier curves
          const d = `
            M ${cx - rx} ${cy}
            C ${cx - rx} ${cy - ry * 0.55} ${cx - rx * 0.55} ${cy - ry - 2} ${cx} ${cy - ry}
            C ${cx + rx * 0.55} ${cy - ry + 2} ${cx + rx + 1} ${cy - ry * 0.55} ${cx + rx} ${cy + 2}
            C ${cx + rx - 1} ${cy + ry * 0.55} ${cx + rx * 0.55} ${cy + ry + 1} ${cx - 2} ${cy + ry}
            C ${cx - rx * 0.55} ${cy + ry - 1} ${cx - rx + 2} ${cy + ry * 0.55} ${cx - rx} ${cy}
          `;

          return (
            <svg style={baseStyles} aria-hidden="true">
              <path
                d={d}
                style={{
                  ...pathStyles,
                  strokeDasharray: pathLength,
                  strokeDashoffset: isVisible ? 0 : pathLength,
                }}
              />
            </svg>
          );
        }

        case "marker": {
          const markerHeight = height + 4;
          const y1 = padding - 2;
          const _y2 = padding + markerHeight;

          return (
            <svg style={baseStyles} aria-hidden="true">
              <rect
                x={padding - 2}
                y={y1}
                width={width + 4}
                height={markerHeight}
                rx={2}
                style={{
                  fill: "var(--highlight-color)",
                  opacity: isVisible ? 0.3 : 0,
                  transition: `opacity ${animationDuration}s cubic-bezier(0.65, 0, 0.35, 1) ${animationDelay}s`,
                }}
              />
            </svg>
          );
        }

        default:
          return null;
      }
    };

    return (
      <span
        ref={(node) => {
          (
            containerRef as React.MutableRefObject<HTMLSpanElement | null>
          ).current = node;
          if (typeof ref === "function") {
            ref(node);
          } else if (ref) {
            ref.current = node;
          }
        }}
        className={cn(highlightVariants({ variant, color, className }))}
        {...props}
      >
        {renderHighlight()}
        <span className="relative z-10">{children}</span>
      </span>
    );
  },
);

HighlightText.displayName = "HighlightText";

export { HighlightText, highlightVariants };
```
---

## Hover Preview <a name="hover-preview"></a>

Link preview card on hover for React. Show images, descriptions, and metadata on link hover. Perfect for portfolios, blogs, and documentation sites.

- **Registry Key**: `hover-preview`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/hover-preview.json
```

#### How to Use
```tsx
import {
  HoverPreviewLink,
  HoverPreviewProvider,
} from "@/registry/default/ui/hover-preview";

const previewData = {
  midjourney: {
    image:
      "https://images.unsplash.com/photo-1695144244472-a4543101ef35?w=560&h=320&fit=crop",
    title: "Midjourney",
    subtitle: "Create stunning AI-generated artwork",
  },
  stable: {
    image:
      "https://images.unsplash.com/photo-1712002641088-9d76f9080889?w=560&h=320&fit=crop",
    title: "Stable Diffusion",
    subtitle: "Open-source generative AI model",
  },
  leonardo: {
    image:
      "https://images.unsplash.com/photo-1718241905696-cb34c2c07bed?w=560&h=320&fit=crop",
    title: "Leonardo AI",
    subtitle: "Production-ready creative assets",
  },
};

export default function HoverPreviewDemo() {
  return (
    <HoverPreviewProvider data={previewData} className="p-8">
      <p className="max-w-2xl text-lg text-muted-foreground leading-relaxed">
        Explore{" "}
        <HoverPreviewLink previewKey="midjourney">Midjourney</HoverPreviewLink>{" "}
        for breathtaking AI-generated artwork and illustrations. For open-source
        freedom try{" "}
        <HoverPreviewLink previewKey="stable">
          Stable Diffusion
        </HoverPreviewLink>{" "}
        or generate production assets with{" "}
        <HoverPreviewLink previewKey="leonardo">Leonardo AI</HoverPreviewLink>.
      </p>
    </HoverPreviewProvider>
  );
}
```
#### Component Source Code
##### File: `ui/hover-preview.tsx`
```tsx
"use client";

import NextImage from "next/image";
import type React from "react";
import {
  createContext,
  forwardRef,
  useCallback,
  useContext,
  useEffect,
  useRef,
  useState,
} from "react";
import { cn } from "@/lib/cn";

// ==========================================
// TYPES & INTERFACES
// ==========================================

export interface PreviewData {
  /** Image URL for the preview card */
  image: string;
  /** Title displayed in the preview card */
  title: string;
  /** Subtitle or description displayed below the title */
  subtitle?: string;
}

export interface HoverPreviewLinkProps {
  /** Unique key to identify which preview data to show */
  previewKey: string;
  /** Content to render as the hoverable link */
  children: React.ReactNode;
  /** Additional CSS classes for the link */
  className?: string;
}

export interface HoverPreviewCardProps {
  /** Width of the preview card in pixels */
  width?: number;
  /** Border radius of the card */
  borderRadius?: number;
  /** Additional CSS classes for the card */
  className?: string;
}

export interface HoverPreviewProviderProps {
  /** Preview data object with keys matching the previewKey in HoverPreviewLink */
  data: Record<string, PreviewData>;
  /** Children components (should include HoverPreviewLink components) */
  children: React.ReactNode;
  /** Card configuration options */
  cardProps?: HoverPreviewCardProps;
  /** Offset distance from cursor in pixels */
  cursorOffset?: number;
  /** Whether to preload all images on mount */
  preloadImages?: boolean;
  /** Additional CSS classes for the container */
  className?: string;
}

// ==========================================
// CONTEXT
// ==========================================

interface HoverPreviewContextValue {
  data: Record<string, PreviewData>;
  activePreview: PreviewData | null;
  position: { x: number; y: number };
  isVisible: boolean;
  cardProps: HoverPreviewCardProps;
  handleHoverStart: (key: string, e: React.MouseEvent) => void;
  handleHoverMove: (e: React.MouseEvent) => void;
  handleHoverEnd: () => void;
}

const HoverPreviewContext = createContext<HoverPreviewContextValue | null>(
  null,
);

function useHoverPreview() {
  const context = useContext(HoverPreviewContext);
  if (!context) {
    throw new Error(
      "HoverPreviewLink must be used within a HoverPreviewProvider",
    );
  }
  return context;
}

// ==========================================
// COMPONENTS
// ==========================================

export function HoverPreviewProvider({
  data,
  children,
  cardProps = {},
  cursorOffset = 20,
  preloadImages = true,
  className,
}: HoverPreviewProviderProps) {
  const [activePreview, setActivePreview] = useState<PreviewData | null>(null);
  const [position, setPosition] = useState({ x: 0, y: 0 });
  const [isVisible, setIsVisible] = useState(false);
  const cardRef = useRef<HTMLDivElement>(null);

  const cardWidth = cardProps.width ?? 300;
  const cardHeight = 250;

  // Preload all images on mount
  useEffect(() => {
    if (!preloadImages) return;
    Object.values(data).forEach((item) => {
      const img = new Image();
      img.crossOrigin = "anonymous";
      img.src = item.image;
    });
  }, [data, preloadImages]);

  const updatePosition = useCallback(
    (e: React.MouseEvent | MouseEvent) => {
      let x = e.clientX - cardWidth / 2;
      let y = e.clientY - cardHeight - cursorOffset;

      // Boundary checks
      if (x + cardWidth > window.innerWidth - 20) {
        x = window.innerWidth - cardWidth - 20;
      }
      if (x < 20) {
        x = 20;
      }
      if (y < 20) {
        y = e.clientY + cursorOffset;
      }

      setPosition({ x, y });
    },
    [cardWidth, cursorOffset],
  );

  const handleHoverStart = useCallback(
    (key: string, e: React.MouseEvent) => {
      const previewData = data[key];
      if (previewData) {
        setActivePreview(previewData);
        setIsVisible(true);
        updatePosition(e);
      }
    },
    [data, updatePosition],
  );

  const handleHoverMove = useCallback(
    (e: React.MouseEvent) => {
      if (isVisible) {
        updatePosition(e);
      }
    },
    [isVisible, updatePosition],
  );

  const handleHoverEnd = useCallback(() => {
    setIsVisible(false);
  }, []);

  const contextValue: HoverPreviewContextValue = {
    data,
    activePreview,
    position,
    isVisible,
    cardProps: { width: cardWidth, ...cardProps },
    handleHoverStart,
    handleHoverMove,
    handleHoverEnd,
  };

  return (
    <HoverPreviewContext.Provider value={contextValue}>
      <div className={cn("relative", className)}>
        {children}
        <HoverPreviewCard ref={cardRef} />
      </div>
    </HoverPreviewContext.Provider>
  );
}

export function HoverPreviewLink({
  previewKey,
  children,
  className,
}: HoverPreviewLinkProps) {
  const { handleHoverStart, handleHoverMove, handleHoverEnd } =
    useHoverPreview();

  return (
    <span
      role="button"
      tabIndex={0}
      className={cn(
        "relative inline-block cursor-pointer font-semibold text-foreground transition-colors",
        "after:absolute after:bottom-0 after:left-0 after:h-0.5 after:w-0 after:bg-gradient-to-r after:from-primary after:to-primary/60 after:transition-all after:duration-300",
        "hover:after:w-full",
        className,
      )}
      onMouseEnter={(e) => handleHoverStart(previewKey, e)}
      onMouseMove={handleHoverMove}
      onMouseLeave={handleHoverEnd}
      onFocus={(e) =>
        handleHoverStart(previewKey, e as unknown as React.MouseEvent)
      }
    >
      {children}
    </span>
  );
}

const HoverPreviewCard = forwardRef<HTMLDivElement>((_, ref) => {
  const { activePreview, position, isVisible, cardProps } = useHoverPreview();

  if (!activePreview) return null;

  return (
    <div
      ref={ref}
      className={cn(
        "pointer-events-none fixed z-50 transition-all duration-200",
        isVisible
          ? "scale-100 opacity-100"
          : "translate-y-2 scale-95 opacity-0",
      )}
      style={{
        left: `${position.x}px`,
        top: `${position.y}px`,
        width: cardProps.width,
      }}
    >
      <div
        className={cn(
          "overflow-hidden border border-border/50 bg-card/95 p-2 shadow-2xl backdrop-blur-md",
          cardProps.className,
        )}
        style={{ borderRadius: cardProps.borderRadius ?? 16 }}
      >
        <NextImage
          src={activePreview.image}
          alt={activePreview.title || ""}
          width={300}
          height={169}
          unoptimized
          className="aspect-video w-full rounded-lg object-cover"
        />
        <div className="px-2 pt-3 pb-1">
          <div className="font-semibold text-foreground text-sm">
            {activePreview.title}
          </div>
          {activePreview.subtitle && (
            <div className="mt-1 text-muted-foreground text-xs">
              {activePreview.subtitle}
            </div>
          )}
        </div>
      </div>
    </div>
  );
});

HoverPreviewCard.displayName = "HoverPreviewCard";

// ==========================================
// EXPORTS
// ==========================================

export { HoverPreviewContext, useHoverPreview };
```
---

## Image Comparison <a name="image-comparison"></a>

Before/after image comparison slider for React. Drag handle, hover reveal, and lens modes. Perfect for photo editing, design, and AI image showcases.

- **Registry Key**: `image-comparison`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/image-comparison.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
import { ImageComparison } from "@/registry/default/ui/image-comparison";

export default function ImageComparisonDemo() {
  return (
    <ImageComparison
      beforeImage="https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=800&h=500&fit=crop&sat=-100"
      afterImage="https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=800&h=500&fit=crop"
      beforeLabel="Grayscale"
      afterLabel="Color"
      className="aspect-video w-full"
    />
  );
}
```
#### Component Source Code
##### File: `ui/image-comparison.tsx`
```tsx
import { GripHorizontal, GripVertical } from "lucide-react";
import { motion, useMotionValue, useTransform } from "motion/react";
import type React from "react";
import { useCallback, useEffect, useRef, useState } from "react";
import { cn } from "@/lib/utils";

// Basic Comparison Slider
interface ImageComparisonProps {
  beforeImage: string;
  afterImage: string;
  beforeLabel?: string;
  afterLabel?: string;
  className?: string;
  initialPosition?: number;
  orientation?: "horizontal" | "vertical";
  showLabels?: boolean;
  sliderColor?: string;
}

export function ImageComparison({
  beforeImage,
  afterImage,
  beforeLabel = "Before",
  afterLabel = "After",
  className,
  initialPosition = 50,
  orientation = "horizontal",
  showLabels = true,
  sliderColor = "hsl(var(--background))",
}: ImageComparisonProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const [position, setPosition] = useState(initialPosition);
  const [isDragging, setIsDragging] = useState(false);

  const handleMove = useCallback(
    (clientX: number, clientY: number) => {
      if (!containerRef.current) return;

      const rect = containerRef.current.getBoundingClientRect();
      let newPosition: number;

      if (orientation === "horizontal") {
        newPosition = ((clientX - rect.left) / rect.width) * 100;
      } else {
        newPosition = ((clientY - rect.top) / rect.height) * 100;
      }

      setPosition(Math.max(0, Math.min(100, newPosition)));
    },
    [orientation],
  );

  const handleMouseDown = (e: React.MouseEvent) => {
    e.preventDefault();
    setIsDragging(true);
  };

  const handleTouchStart = () => {
    setIsDragging(true);
  };

  useEffect(() => {
    const handleMouseMove = (e: MouseEvent) => {
      if (isDragging) {
        handleMove(e.clientX, e.clientY);
      }
    };

    const handleTouchMove = (e: TouchEvent) => {
      if (isDragging && e.touches[0]) {
        handleMove(e.touches[0].clientX, e.touches[0].clientY);
      }
    };

    const handleEnd = () => {
      setIsDragging(false);
    };

    if (isDragging) {
      window.addEventListener("mousemove", handleMouseMove);
      window.addEventListener("mouseup", handleEnd);
      window.addEventListener("touchmove", handleTouchMove);
      window.addEventListener("touchend", handleEnd);
    }

    return () => {
      window.removeEventListener("mousemove", handleMouseMove);
      window.removeEventListener("mouseup", handleEnd);
      window.removeEventListener("touchmove", handleTouchMove);
      window.removeEventListener("touchend", handleEnd);
    };
  }, [isDragging, handleMove]);

  return (
    <div
      ref={containerRef}
      className={cn(
        "relative select-none overflow-hidden rounded-xl",
        className,
      )}
    >
      {/* After Image (Background) */}
      {/* biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx */}
      <img
        src={afterImage}
        alt={afterLabel}
        className="h-full w-full object-cover"
        draggable={false}
      />

      {/* Before Image (Clipped) */}
      <div
        className="absolute inset-0 overflow-hidden"
        style={{
          clipPath:
            orientation === "horizontal"
              ? `inset(0 ${100 - position}% 0 0)`
              : `inset(0 0 ${100 - position}% 0)`,
        }}
      >
        {/* biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx */}
        <img
          src={beforeImage}
          alt={beforeLabel}
          className="h-full w-full object-cover"
          draggable={false}
        />
      </div>

      {/* Slider Line */}
      <div
        className={cn(
          "absolute z-10",
          orientation === "horizontal"
            ? "top-0 h-full w-0.5 -translate-x-1/2"
            : "left-0 h-0.5 w-full -translate-y-1/2",
        )}
        style={{
          [orientation === "horizontal" ? "left" : "top"]: `${position}%`,
          backgroundColor: sliderColor,
          boxShadow: "0 0 10px rgba(0,0,0,0.3)",
        }}
      />

      {/* Slider Handle */}
      <motion.div
        className={cn(
          "absolute z-20 flex cursor-grab items-center justify-center rounded-full border-2 bg-background shadow-lg active:cursor-grabbing",
          orientation === "horizontal"
            ? "h-10 w-10 -translate-x-1/2 -translate-y-1/2"
            : "h-10 w-10 -translate-x-1/2 -translate-y-1/2",
        )}
        style={{
          [orientation === "horizontal" ? "left" : "left"]:
            orientation === "horizontal" ? `${position}%` : "50%",
          [orientation === "horizontal" ? "top" : "top"]:
            orientation === "horizontal" ? "50%" : `${position}%`,
          borderColor: sliderColor,
        }}
        onMouseDown={handleMouseDown}
        onTouchStart={handleTouchStart}
        whileHover={{ scale: 1.1 }}
        whileTap={{ scale: 0.95 }}
      >
        {orientation === "horizontal" ? (
          <GripVertical className="h-5 w-5 text-muted-foreground" />
        ) : (
          <GripHorizontal className="h-5 w-5 text-muted-foreground" />
        )}
      </motion.div>

      {/* Labels */}
      {showLabels && (
        <>
          <div
            className={cn(
              "absolute rounded-md bg-background/80 px-2 py-1 font-medium text-xs backdrop-blur-sm",
              orientation === "horizontal" ? "top-3 left-3" : "top-3 left-3",
            )}
          >
            {beforeLabel}
          </div>
          <div
            className={cn(
              "absolute rounded-md bg-background/80 px-2 py-1 font-medium text-xs backdrop-blur-sm",
              orientation === "horizontal"
                ? "top-3 right-3"
                : "bottom-3 left-3",
            )}
          >
            {afterLabel}
          </div>
        </>
      )}
    </div>
  );
}

// Hover Reveal Comparison
interface ImageComparisonHoverProps {
  beforeImage: string;
  afterImage: string;
  beforeLabel?: string;
  afterLabel?: string;
  className?: string;
  showLabels?: boolean;
}

export function ImageComparisonHover({
  beforeImage,
  afterImage,
  beforeLabel = "Before",
  afterLabel = "After",
  className,
  showLabels = true,
}: ImageComparisonHoverProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const [position, setPosition] = useState(50);

  const handleMouseMove = (e: React.MouseEvent) => {
    if (!containerRef.current) return;
    const rect = containerRef.current.getBoundingClientRect();
    const x = ((e.clientX - rect.left) / rect.width) * 100;
    setPosition(Math.max(0, Math.min(100, x)));
  };

  const handleMouseLeave = () => {
    setPosition(50);
  };

  return (
    <div
      ref={containerRef}
      className={cn(
        "relative cursor-ew-resize select-none overflow-hidden rounded-xl",
        className,
      )}
      onMouseMove={handleMouseMove}
      onMouseLeave={handleMouseLeave}
      role="button"
      tabIndex={0}
    >
      {/* After Image */}
      {/* biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx */}
      <img
        src={afterImage}
        alt={afterLabel}
        className="h-full w-full object-cover"
        draggable={false}
      />

      {/* Before Image */}
      <motion.div
        className="absolute inset-0 overflow-hidden"
        animate={{ clipPath: `inset(0 ${100 - position}% 0 0)` }}
        transition={{ type: "tween", duration: 0.1 }}
      >
        {/* biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx */}
        <img
          src={beforeImage}
          alt={beforeLabel}
          className="h-full w-full object-cover"
          draggable={false}
        />
      </motion.div>

      {/* Divider Line */}
      {/* <motion.div
        className="absolute top-0 h-full w-0.5 bg-background"
        animate={{ left: `${position}%` }}
        transition={{ type: "tween", duration: 0.1 }}
        style={{ transform: "translateX(-50%)", boxShadow: "0 0 10px rgba(0,0,0,0.3)" }}
      /> */}

      {/* Labels */}
      {showLabels && (
        <>
          <div className="absolute top-3 left-3 rounded-md bg-background/80 px-2 py-1 font-medium text-xs backdrop-blur-sm">
            {beforeLabel}
          </div>
          <div className="absolute top-3 right-3 rounded-md bg-background/80 px-2 py-1 font-medium text-xs backdrop-blur-sm">
            {afterLabel}
          </div>
        </>
      )}
    </div>
  );
}

// Split View Comparison
interface ImageComparisonSplitProps {
  beforeImage: string;
  afterImage: string;
  beforeLabel?: string;
  afterLabel?: string;
  className?: string;
  gap?: number;
}

export function ImageComparisonSplit({
  beforeImage,
  afterImage,
  beforeLabel = "Before",
  afterLabel = "After",
  className,
  gap = 4,
}: ImageComparisonSplitProps) {
  return (
    <div
      className={cn("flex overflow-hidden rounded-xl", className)}
      style={{ gap }}
    >
      <div className="relative flex-1 overflow-hidden">
        {/* biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx */}
        <img
          src={beforeImage}
          alt={beforeLabel}
          className="h-full w-full object-cover"
          draggable={false}
        />
        <div className="absolute bottom-3 left-3 rounded-md bg-background/80 px-2 py-1 font-medium text-xs backdrop-blur-sm">
          {beforeLabel}
        </div>
      </div>
      <div className="relative flex-1 overflow-hidden">
        {/* biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx */}
        <img
          src={afterImage}
          alt={afterLabel}
          className="h-full w-full object-cover"
          draggable={false}
        />
        <div className="absolute right-3 bottom-3 rounded-md bg-background/80 px-2 py-1 font-medium text-xs backdrop-blur-sm">
          {afterLabel}
        </div>
      </div>
    </div>
  );
}

// Fade Toggle Comparison
interface ImageComparisonFadeProps {
  beforeImage: string;
  afterImage: string;
  beforeLabel?: string;
  afterLabel?: string;
  className?: string;
  showLabels?: boolean;
}

export function ImageComparisonFade({
  beforeImage,
  afterImage,
  beforeLabel = "Before",
  afterLabel = "After",
  className,
  showLabels = true,
}: ImageComparisonFadeProps) {
  const [showBefore, setShowBefore] = useState(true);

  return (
    <div
      className={cn(
        "group relative cursor-pointer select-none overflow-hidden rounded-xl",
        className,
      )}
      onClick={() => setShowBefore(!showBefore)}
      role="button"
      tabIndex={0}
    >
      {/* After Image */}
      {/* biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx */}
      <img
        src={afterImage}
        alt={afterLabel}
        className="h-full w-full object-cover"
        draggable={false}
      />

      {/* Before Image with fade */}
      <motion.div
        className="absolute inset-0"
        animate={{ opacity: showBefore ? 1 : 0 }}
        transition={{ duration: 0.5 }}
      >
        {/* biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx */}
        <img
          src={beforeImage}
          alt={beforeLabel}
          className="h-full w-full object-cover"
          draggable={false}
        />
      </motion.div>

      {/* Label */}
      {showLabels && (
        <motion.div
          className="absolute top-3 left-1/2 -translate-x-1/2 rounded-md bg-background/80 px-3 py-1.5 font-medium text-sm backdrop-blur-sm"
          key={showBefore ? "before" : "after"}
          initial={{ opacity: 0, y: -10 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.2 }}
        >
          {showBefore ? beforeLabel : afterLabel}
        </motion.div>
      )}

      {/* Click hint */}
      <div className="absolute bottom-3 left-1/2 -translate-x-1/2 rounded-md bg-background/80 px-2 py-1 text-muted-foreground text-xs opacity-0 backdrop-blur-sm transition-opacity group-hover:opacity-100">
        Click to toggle
      </div>
    </div>
  );
}

// Swipe Comparison
interface ImageComparisonSwipeProps {
  beforeImage: string;
  afterImage: string;
  beforeLabel?: string;
  afterLabel?: string;
  className?: string;
}

export function ImageComparisonSwipe({
  beforeImage,
  afterImage,
  beforeLabel = "Before",
  afterLabel = "After",
  className,
}: ImageComparisonSwipeProps) {
  const x = useMotionValue(0);
  const containerRef = useRef<HTMLDivElement>(null);
  const [containerWidth, setContainerWidth] = useState(0);

  useEffect(() => {
    if (containerRef.current) {
      setContainerWidth(containerRef.current.offsetWidth);
    }
  }, []);

  const clipPath = useTransform(
    x,
    [-containerWidth / 2, containerWidth / 2],
    [0, 100],
  );
  const displayClipPath = useTransform(
    clipPath,
    (v) => `inset(0 ${100 - (50 + v / 2)}% 0 0)`,
  );
  const linePosition = useTransform(clipPath, (v) => `${50 + v / 2}%`);

  return (
    <div
      ref={containerRef}
      className={cn(
        "relative select-none overflow-hidden rounded-xl",
        className,
      )}
    >
      {/* After Image */}
      {/* biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx */}
      <img
        src={afterImage}
        alt={afterLabel}
        className="h-full w-full object-cover"
        draggable={false}
      />

      {/* Before Image */}
      <motion.div
        className="absolute inset-0 overflow-hidden"
        style={{ clipPath: displayClipPath }}
      >
        {/* biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx */}
        <img
          src={beforeImage}
          alt={beforeLabel}
          className="h-full w-full object-cover"
          draggable={false}
        />
      </motion.div>

      {/* Slider Line */}
      <motion.div
        className="absolute top-0 h-full w-0.5 bg-background"
        style={{
          left: linePosition,
          transform: "translateX(-50%)",
          boxShadow: "0 0 10px rgba(0,0,0,0.3)",
        }}
      />

      {/* Draggable Handle */}
      <motion.div
        className="absolute top-1/2 left-1/2 z-20 flex h-12 w-12 -translate-y-1/2 cursor-grab items-center justify-center rounded-full border-2 border-background bg-background shadow-lg active:cursor-grabbing"
        drag="x"
        dragConstraints={{
          left: -containerWidth / 2 + 20,
          right: containerWidth / 2 - 20,
        }}
        dragElastic={0}
        style={{ x }}
        whileHover={{ scale: 1.1 }}
        whileTap={{ scale: 0.95 }}
      >
        <GripVertical className="h-5 w-5 text-muted-foreground" />
      </motion.div>

      {/* Labels */}
      <div className="absolute top-3 left-3 rounded-md bg-background/80 px-2 py-1 font-medium text-xs backdrop-blur-sm">
        {beforeLabel}
      </div>
      <div className="absolute top-3 right-3 rounded-md bg-background/80 px-2 py-1 font-medium text-xs backdrop-blur-sm">
        {afterLabel}
      </div>
    </div>
  );
}

// Lens Comparison (magnifying glass effect)
interface ImageComparisonLensProps {
  beforeImage: string;
  afterImage: string;
  className?: string;
  lensSize?: number;
}

export function ImageComparisonLens({
  beforeImage,
  afterImage,
  className,
  lensSize = 150,
}: ImageComparisonLensProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const [lensPosition, setLensPosition] = useState({ x: 50, y: 50 });
  const [isHovering, setIsHovering] = useState(false);

  const handleMouseMove = (e: React.MouseEvent) => {
    if (!containerRef.current) return;
    const rect = containerRef.current.getBoundingClientRect();
    const x = ((e.clientX - rect.left) / rect.width) * 100;
    const y = ((e.clientY - rect.top) / rect.height) * 100;
    setLensPosition({ x, y });
  };

  return (
    <div
      ref={containerRef}
      className={cn(
        "relative cursor-none select-none overflow-hidden rounded-xl",
        className,
      )}
      onMouseMove={handleMouseMove}
      onMouseEnter={() => setIsHovering(true)}
      onMouseLeave={() => setIsHovering(false)}
      role="button"
      tabIndex={0}
    >
      {/* Before Image (Background) */}
      {/* biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx */}
      <img
        src={beforeImage}
        alt="Before"
        className="h-full w-full object-cover"
        draggable={false}
      />

      {/* Lens showing After Image */}
      <motion.div
        className="pointer-events-none absolute overflow-hidden rounded-full border-2 border-background shadow-2xl"
        style={{
          width: lensSize,
          height: lensSize,
          left: `calc(${lensPosition.x}% - ${lensSize / 2}px)`,
          top: `calc(${lensPosition.y}% - ${lensSize / 2}px)`,
        }}
        animate={{ opacity: isHovering ? 1 : 0, scale: isHovering ? 1 : 0.8 }}
        transition={{ duration: 0.2 }}
      >
        <div
          className="h-full w-full"
          style={{
            backgroundImage: `url(${afterImage})`,
            backgroundSize: containerRef.current
              ? `${containerRef.current.offsetWidth}px ${containerRef.current.offsetHeight}px`
              : "100% 100%",
            backgroundPosition: `${lensPosition.x}% ${lensPosition.y}%`,
          }}
        />
      </motion.div>

      {/* Labels */}
      <div className="absolute top-3 left-3 rounded-md bg-background/80 px-2 py-1 font-medium text-xs backdrop-blur-sm">
        Before
      </div>
      <div className="absolute top-3 right-3 rounded-md bg-background/80 px-2 py-1 font-medium text-xs backdrop-blur-sm">
        Hover to see After
      </div>
    </div>
  );
}

export type {
  ImageComparisonFadeProps,
  ImageComparisonHoverProps,
  ImageComparisonLensProps,
  ImageComparisonProps,
  ImageComparisonSplitProps,
  ImageComparisonSwipeProps,
};
```
---

## Image Sphere <a name="image-sphere"></a>

3D rotating image gallery sphere for React. Drag to rotate, click to expand. Built with CSS 3D transforms and spring physics. Perfect for portfolios.

- **Registry Key**: `image-sphere`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/image-sphere.json
```

#### Dependencies
- **NPM Packages**:
  - `lucide-react`

#### How to Use
```tsx
import SphereImageGrid from "@/registry/default/ui/image-sphere";

const sampleImages = [
  {
    id: "1",
    src: "https://images.unsplash.com/photo-1535713875002-d1d0cf377fde?w=150&h=150&fit=crop",
    alt: "User avatar 1",
    title: "Alex Johnson",
    description: "Senior Software Engineer",
  },
  {
    id: "2",
    src: "https://images.unsplash.com/photo-1494790108377-be9c29b29330?w=150&h=150&fit=crop",
    alt: "User avatar 2",
    title: "Sarah Miller",
    description: "Product Designer",
  },
  {
    id: "3",
    src: "https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?w=150&h=150&fit=crop",
    alt: "User avatar 3",
    title: "Michael Chen",
    description: "Data Scientist",
  },
  {
    id: "4",
    src: "https://images.unsplash.com/photo-1438761681033-6461ffad8d80?w=150&h=150&fit=crop",
    alt: "User avatar 4",
    title: "Emma Wilson",
    description: "UX Researcher",
  },
  {
    id: "5",
    src: "https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?w=150&h=150&fit=crop",
    alt: "User avatar 5",
    title: "David Brown",
    description: "Frontend Developer",
  },
  {
    id: "6",
    src: "https://images.unsplash.com/photo-1544005313-94ddf0286df2?w=150&h=150&fit=crop",
    alt: "User avatar 6",
    title: "Lisa Anderson",
    description: "Marketing Lead",
  },
  {
    id: "7",
    src: "https://images.unsplash.com/photo-1500648767791-00dcc994a43e?w=150&h=150&fit=crop",
    alt: "User avatar 7",
    title: "James Taylor",
    description: "DevOps Engineer",
  },
  {
    id: "8",
    src: "https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=150&h=150&fit=crop",
    alt: "User avatar 8",
    title: "Olivia Davis",
    description: "Project Manager",
  },
];

export default function ImageSphereDemo() {
  return (
    <div className="flex items-center justify-center p-8">
      <SphereImageGrid
        images={sampleImages}
        containerSize={400}
        sphereRadius={180}
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/image-sphere.tsx`
```tsx
import { X } from "lucide-react";
import Image from "next/image";
import type React from "react";
import { useCallback, useEffect, useRef, useState } from "react";

export interface Position3D {
  x: number;
  y: number;
  z: number;
}

export interface SphericalPosition {
  theta: number; // Azimuth angle in degrees
  phi: number; // Polar angle in degrees
  radius: number; // Distance from center
}

export interface WorldPosition extends Position3D {
  scale: number;
  zIndex: number;
  isVisible: boolean;
  fadeOpacity: number;
  originalIndex: number;
}

export interface ImageData {
  id: string;
  src: string;
  alt: string;
  title?: string;
  description?: string;
}

export interface SphereImageGridProps {
  images?: ImageData[];
  containerSize?: number;
  sphereRadius?: number;
  dragSensitivity?: number;
  momentumDecay?: number;
  maxRotationSpeed?: number;
  baseImageScale?: number;
  hoverScale?: number;
  perspective?: number;
  autoRotate?: boolean;
  autoRotateSpeed?: number;
  className?: string;
}

interface RotationState {
  x: number;
  y: number;
  z: number;
}

interface VelocityState {
  x: number;
  y: number;
}

interface MousePosition {
  x: number;
  y: number;
}

// ==========================================
// CONSTANTS & CONFIGURATION
// ==========================================

const SPHERE_MATH = {
  degreesToRadians: (degrees: number): number => degrees * (Math.PI / 180),
  radiansToDegrees: (radians: number): number => radians * (180 / Math.PI),

  sphericalToCartesian: (
    radius: number,
    theta: number,
    phi: number,
  ): Position3D => ({
    x: radius * Math.sin(phi) * Math.cos(theta),
    y: radius * Math.cos(phi),
    z: radius * Math.sin(phi) * Math.sin(theta),
  }),

  calculateDistance: (
    pos: Position3D,
    center: Position3D = { x: 0, y: 0, z: 0 },
  ): number => {
    const dx = pos.x - center.x;
    const dy = pos.y - center.y;
    const dz = pos.z - center.z;
    return Math.sqrt(dx * dx + dy * dy + dz * dz);
  },

  normalizeAngle: (angle: number): number => {
    while (angle > 180) angle -= 360;
    while (angle < -180) angle += 360;
    return angle;
  },
};

// ==========================================
// MAIN COMPONENT
// ==========================================

const SphereImageGrid: React.FC<SphereImageGridProps> = ({
  images = [],
  containerSize = 400,
  sphereRadius = 200,
  dragSensitivity = 0.5,
  momentumDecay = 0.95,
  maxRotationSpeed = 5,
  baseImageScale = 0.12,
  hoverScale: _hoverScale = 1.2,
  perspective = 1000,
  autoRotate = false,
  autoRotateSpeed = 0.3,
  className = "",
}) => {
  // ==========================================
  // STATE & REFS
  // ==========================================

  const [isMounted, setIsMounted] = useState<boolean>(false);
  const [rotation, setRotation] = useState<RotationState>({
    x: 15,
    y: 15,
    z: 0,
  });
  const [velocity, setVelocity] = useState<VelocityState>({ x: 0, y: 0 });
  const [isDragging, setIsDragging] = useState<boolean>(false);
  const [selectedImage, setSelectedImage] = useState<ImageData | null>(null);
  const [imagePositions, setImagePositions] = useState<SphericalPosition[]>([]);
  const [hoveredIndex, setHoveredIndex] = useState<number | null>(null);

  const containerRef = useRef<HTMLDivElement>(null);
  const lastMousePos = useRef<MousePosition>({ x: 0, y: 0 });
  const animationFrame = useRef<number | null>(null);

  // ==========================================
  // COMPUTED VALUES
  // ==========================================

  const actualSphereRadius = sphereRadius || containerSize * 0.5;
  const baseImageSize = containerSize * baseImageScale;

  // ==========================================
  // UTILITY FUNCTIONS
  // ==========================================

  const generateSpherePositions = useCallback((): SphericalPosition[] => {
    const positions: SphericalPosition[] = [];
    const imageCount = images.length;

    // Use Fibonacci sphere distribution for even coverage
    const goldenRatio = (1 + Math.sqrt(5)) / 2;
    const angleIncrement = (2 * Math.PI) / goldenRatio;

    for (let i = 0; i < imageCount; i++) {
      // Fibonacci sphere distribution
      const t = i / imageCount;
      const inclination = Math.acos(1 - 2 * t);
      const azimuth = angleIncrement * i;

      // Convert to degrees and focus on front hemisphere
      let phi = inclination * (180 / Math.PI);
      let theta = (azimuth * (180 / Math.PI)) % 360;

      // Better pole coverage - reach poles but avoid extreme mathematical issues
      const poleBonus = (Math.abs(phi - 90) / 90) ** 0.6 * 35; // Moderate boost toward poles
      if (phi < 90) {
        phi = Math.max(5, phi - poleBonus); // Reach closer to top pole (15° minimum)
      } else {
        phi = Math.min(175, phi + poleBonus); // Reach closer to bottom pole (165° maximum)
      }

      // Map to fuller vertical range - covers poles but avoids extremes
      phi = 15 + (phi / 180) * 150; // Map to 15-165 degrees for pole coverage with stability

      // Add slight randomization to prevent perfect patterns
      const randomOffset = (Math.random() - 0.5) * 20;
      theta = (theta + randomOffset) % 360;
      phi = Math.max(0, Math.min(180, phi + (Math.random() - 0.5) * 10));

      positions.push({
        theta: theta,
        phi: phi,
        radius: actualSphereRadius,
      });
    }

    return positions;
  }, [images.length, actualSphereRadius]);

  const calculateWorldPositions = useCallback((): WorldPosition[] => {
    const positions = imagePositions.map((pos, index) => {
      // Apply rotation using proper 3D rotation matrices
      const thetaRad = SPHERE_MATH.degreesToRadians(pos.theta);
      const phiRad = SPHERE_MATH.degreesToRadians(pos.phi);
      const rotXRad = SPHERE_MATH.degreesToRadians(rotation.x);
      const rotYRad = SPHERE_MATH.degreesToRadians(rotation.y);

      // Initial position on sphere
      let x = pos.radius * Math.sin(phiRad) * Math.cos(thetaRad);
      let y = pos.radius * Math.cos(phiRad);
      let z = pos.radius * Math.sin(phiRad) * Math.sin(thetaRad);

      // Apply Y-axis rotation (horizontal drag)
      const x1 = x * Math.cos(rotYRad) + z * Math.sin(rotYRad);
      const z1 = -x * Math.sin(rotYRad) + z * Math.cos(rotYRad);
      x = x1;
      z = z1;

      // Apply X-axis rotation (vertical drag)
      const y2 = y * Math.cos(rotXRad) - z * Math.sin(rotXRad);
      const z2 = y * Math.sin(rotXRad) + z * Math.cos(rotXRad);
      y = y2;
      z = z2;

      const worldPos: Position3D = { x, y, z };

      // Calculate visibility with smooth fade zones
      const fadeZoneStart = -10; // Start fading out
      const fadeZoneEnd = -30; // Completely hidden
      const isVisible = worldPos.z > fadeZoneEnd;

      // Calculate fade opacity based on Z position
      let fadeOpacity = 1;
      if (worldPos.z <= fadeZoneStart) {
        // Linear fade from 1 to 0 as Z goes from fadeZoneStart to fadeZoneEnd
        fadeOpacity = Math.max(
          0,
          (worldPos.z - fadeZoneEnd) / (fadeZoneStart - fadeZoneEnd),
        );
      }

      // Check if this image originated from a pole position
      const isPoleImage = pos.phi < 30 || pos.phi > 150; // Images from extreme angles

      // Calculate distance from center for scaling (in 2D screen space)
      const distanceFromCenter = Math.sqrt(
        worldPos.x * worldPos.x + worldPos.y * worldPos.y,
      );
      const maxDistance = actualSphereRadius;
      const distanceRatio = Math.min(distanceFromCenter / maxDistance, 1);

      // Scale based on distance from center - be more forgiving for pole images
      const distancePenalty = isPoleImage ? 0.4 : 0.7; // Less penalty for pole images
      const centerScale = Math.max(0.3, 1 - distanceRatio * distancePenalty);

      // Also consider Z-depth for additional scaling
      const depthScale =
        (worldPos.z + actualSphereRadius) / (2 * actualSphereRadius);
      const scale = centerScale * Math.max(0.5, 0.8 + depthScale * 0.3);

      return {
        ...worldPos,
        scale,
        zIndex: Math.round(1000 + worldPos.z),
        isVisible,
        fadeOpacity,
        originalIndex: index,
      };
    });

    // Apply collision detection to prevent overlaps
    const adjustedPositions = [...positions];

    for (let i = 0; i < adjustedPositions.length; i++) {
      const pos = adjustedPositions[i];
      if (!pos?.isVisible) continue;

      let adjustedScale = pos.scale;
      const imageSize = baseImageSize * adjustedScale;

      // Check for overlaps with other visible images
      for (let j = 0; j < adjustedPositions.length; j++) {
        if (i === j) continue;

        const other = adjustedPositions[j];
        if (!other?.isVisible) continue;

        const otherSize = baseImageSize * other.scale;

        // Calculate 2D distance between images on screen
        const dx = pos.x - other.x;
        const dy = pos.y - other.y;
        const distance = Math.sqrt(dx * dx + dy * dy);

        // Minimum distance to prevent overlap (with more generous padding)
        const minDistance = (imageSize + otherSize) / 2 + 25;

        if (distance < minDistance && distance > 0) {
          // More aggressive scale reduction to prevent overlap
          const overlap = minDistance - distance;
          const reductionFactor = Math.max(
            0.4,
            1 - (overlap / minDistance) * 0.6,
          );
          adjustedScale = Math.min(
            adjustedScale,
            adjustedScale * reductionFactor,
          );
        }
      }

      adjustedPositions[i] = {
        ...pos,
        scale: Math.max(0.25, adjustedScale), // Ensure minimum scale
        zIndex: pos.zIndex ?? 0, // Ensure zIndex is a number
      };
    }

    return adjustedPositions;
  }, [imagePositions, rotation, actualSphereRadius, baseImageSize]);

  const clampRotationSpeed = useCallback(
    (speed: number): number => {
      return Math.max(-maxRotationSpeed, Math.min(maxRotationSpeed, speed));
    },
    [maxRotationSpeed],
  );

  // ==========================================
  // PHYSICS & MOMENTUM
  // ==========================================

  const updateMomentum = useCallback(() => {
    if (isDragging) return;

    setVelocity((prev) => {
      const newVelocity = {
        x: prev.x * momentumDecay,
        y: prev.y * momentumDecay,
      };

      // Stop animation if velocity is too low and auto-rotate is off
      if (
        !autoRotate &&
        Math.abs(newVelocity.x) < 0.01 &&
        Math.abs(newVelocity.y) < 0.01
      ) {
        return { x: 0, y: 0 };
      }

      return newVelocity;
    });

    setRotation((prev) => {
      let newY = prev.y;

      // Add auto-rotation to Y axis (horizontal rotation)
      if (autoRotate) {
        newY += autoRotateSpeed;
      }

      // Add momentum-based rotation
      newY += clampRotationSpeed(velocity.y);

      return {
        x: SPHERE_MATH.normalizeAngle(prev.x + clampRotationSpeed(velocity.x)),
        y: SPHERE_MATH.normalizeAngle(newY),
        z: prev.z,
      };
    });
  }, [
    isDragging,
    momentumDecay,
    velocity,
    clampRotationSpeed,
    autoRotate,
    autoRotateSpeed,
  ]);

  // ==========================================
  // EVENT HANDLERS
  // ==========================================

  const handleMouseDown = useCallback((e: React.MouseEvent) => {
    e.preventDefault();
    setIsDragging(true);
    setVelocity({ x: 0, y: 0 });
    lastMousePos.current = { x: e.clientX, y: e.clientY };
  }, []);

  const handleMouseMove = useCallback(
    (e: MouseEvent) => {
      if (!isDragging) return;

      const deltaX = e.clientX - lastMousePos.current.x;
      const deltaY = e.clientY - lastMousePos.current.y;

      const rotationDelta = {
        x: -deltaY * dragSensitivity,
        y: deltaX * dragSensitivity,
      };

      setRotation((prev) => ({
        x: SPHERE_MATH.normalizeAngle(
          prev.x + clampRotationSpeed(rotationDelta.x),
        ),
        y: SPHERE_MATH.normalizeAngle(
          prev.y + clampRotationSpeed(rotationDelta.y),
        ),
        z: prev.z,
      }));

      // Update velocity for momentum
      setVelocity({
        x: clampRotationSpeed(rotationDelta.x),
        y: clampRotationSpeed(rotationDelta.y),
      });

      lastMousePos.current = { x: e.clientX, y: e.clientY };
    },
    [isDragging, dragSensitivity, clampRotationSpeed],
  );

  const handleMouseUp = useCallback(() => {
    setIsDragging(false);
  }, []);

  const handleTouchStart = useCallback((e: React.TouchEvent) => {
    e.preventDefault();
    const touch = e.touches[0];
    if (touch) {
      setIsDragging(true);
      setVelocity({ x: 0, y: 0 });
      lastMousePos.current = { x: touch.clientX, y: touch.clientY };
    }
  }, []);

  const handleTouchMove = useCallback(
    (e: TouchEvent) => {
      if (!isDragging) return;
      e.preventDefault();

      const touch = e.touches[0];
      if (!touch) return;

      const deltaX = touch.clientX - lastMousePos.current.x;
      const deltaY = touch.clientY - lastMousePos.current.y;

      const rotationDelta = {
        x: -deltaY * dragSensitivity,
        y: deltaX * dragSensitivity,
      };

      setRotation((prev) => ({
        x: SPHERE_MATH.normalizeAngle(
          prev.x + clampRotationSpeed(rotationDelta.x),
        ),
        y: SPHERE_MATH.normalizeAngle(
          prev.y + clampRotationSpeed(rotationDelta.y),
        ),
        z: prev.z,
      }));

      setVelocity({
        x: clampRotationSpeed(rotationDelta.x),
        y: clampRotationSpeed(rotationDelta.y),
      });

      lastMousePos.current = { x: touch.clientX, y: touch.clientY };
    },
    [isDragging, dragSensitivity, clampRotationSpeed],
  );

  const handleTouchEnd = useCallback(() => {
    setIsDragging(false);
  }, []);

  // ==========================================
  // EFFECTS & LIFECYCLE
  // ==========================================

  useEffect(() => {
    setIsMounted(true);
  }, []);

  useEffect(() => {
    setImagePositions(generateSpherePositions());
  }, [generateSpherePositions]);

  useEffect(() => {
    const animate = () => {
      updateMomentum();
      animationFrame.current = requestAnimationFrame(animate);
    };

    if (isMounted) {
      animationFrame.current = requestAnimationFrame(animate);
    }

    return () => {
      if (animationFrame.current) {
        cancelAnimationFrame(animationFrame.current);
      }
    };
  }, [isMounted, updateMomentum]);

  useEffect(() => {
    if (!isMounted) return;

    const container = containerRef.current;
    if (!container) return;

    // Mouse events
    document.addEventListener("mousemove", handleMouseMove);
    document.addEventListener("mouseup", handleMouseUp);

    // Touch events
    document.addEventListener("touchmove", handleTouchMove, { passive: false });
    document.addEventListener("touchend", handleTouchEnd);

    return () => {
      document.removeEventListener("mousemove", handleMouseMove);
      document.removeEventListener("mouseup", handleMouseUp);
      document.removeEventListener("touchmove", handleTouchMove);
      document.removeEventListener("touchend", handleTouchEnd);
    };
  }, [
    isMounted,
    handleMouseMove,
    handleMouseUp,
    handleTouchMove,
    handleTouchEnd,
  ]);

  // ==========================================
  // RENDER HELPERS
  // ==========================================

  // Calculate world positions once per render
  const worldPositions = calculateWorldPositions();

  const renderImageNode = useCallback(
    (image: ImageData, index: number) => {
      const position = worldPositions[index];

      if (!position || !position.isVisible) return null;

      const imageSize = baseImageSize * position.scale;
      const isHovered = hoveredIndex === index;
      const finalScale = isHovered ? Math.min(1.2, 1.2 / position.scale) : 1;

      return (
        <div
          key={image.id}
          role="button"
          tabIndex={0}
          className="absolute cursor-pointer select-none transition-transform duration-200 ease-out"
          style={{
            width: `${imageSize}px`,
            height: `${imageSize}px`,
            left: `${containerSize / 2 + position.x}px`,
            top: `${containerSize / 2 + position.y}px`,
            opacity: position.fadeOpacity,
            transform: `translate(-50%, -50%) scale(${finalScale})`,
            zIndex: position.zIndex,
          }}
          onMouseEnter={() => setHoveredIndex(index)}
          onMouseLeave={() => setHoveredIndex(null)}
          onClick={() => setSelectedImage(image)}
          onKeyDown={(e) => e.key === "Enter" && setSelectedImage(image)}
        >
          <div className="relative h-full w-full overflow-hidden rounded-full border-2 border-white/20 shadow-lg">
            <Image
              src={image.src}
              alt={image.alt}
              fill
              className="object-cover"
              draggable={false}
              loading={index < 3 ? "eager" : "lazy"}
              unoptimized
            />
          </div>
        </div>
      );
    },
    [worldPositions, baseImageSize, containerSize, hoveredIndex],
  );

  const renderSpotlightModal = () => {
    if (!selectedImage) return null;

    return (
      <div
        className="fixed inset-0 z-50 flex items-center justify-center bg-black/30 p-4"
        onClick={() => setSelectedImage(null)}
        role="button"
        tabIndex={0}
        onKeyDown={(e) => e.key === "Escape" && setSelectedImage(null)}
        style={{
          animation: "fadeIn 0.3s ease-out",
        }}
      >
        <div
          className="w-full max-w-md overflow-hidden rounded-xl bg-white"
          onClick={(e) => e.stopPropagation()}
          role="dialog"
          aria-modal="true"
          style={{
            animation: "scaleIn 0.3s ease-out",
          }}
        >
          <div className="relative aspect-square">
            <Image
              src={selectedImage.src}
              alt={selectedImage.alt}
              fill
              className="object-cover"
              unoptimized
            />
            <button
              onClick={() => setSelectedImage(null)}
              className="absolute top-2 right-2 flex h-8 w-8 cursor-pointer items-center justify-center rounded-full bg-black bg-opacity-50 text-white transition-all hover:bg-opacity-70"
            >
              <X size={16} />
            </button>
          </div>

          {(selectedImage.title || selectedImage.description) && (
            <div className="p-6">
              {selectedImage.title && (
                <h3 className="mb-2 font-bold text-xl">
                  {selectedImage.title}
                </h3>
              )}
              {selectedImage.description && (
                <p className="text-gray-600">{selectedImage.description}</p>
              )}
            </div>
          )}
        </div>
      </div>
    );
  };

  // ==========================================
  // EARLY RETURNS
  // ==========================================

  if (!isMounted) {
    return (
      <div
        className="flex animate-pulse items-center justify-center rounded-lg bg-gray-100"
        style={{ width: containerSize, height: containerSize }}
      >
        <div className="text-gray-400">Loading...</div>
      </div>
    );
  }

  if (!images.length) {
    return (
      <div
        className="flex items-center justify-center rounded-lg border-2 border-gray-300 border-dashed bg-gray-50"
        style={{ width: containerSize, height: containerSize }}
      >
        <div className="text-center text-gray-400">
          <p>No images provided</p>
          <p className="text-sm">Add images to the images prop</p>
        </div>
      </div>
    );
  }

  // ==========================================
  // MAIN RENDER
  // ==========================================

  return (
    <>
      <style>{`
        @keyframes fadeIn {
          from { opacity: 0; }
          to { opacity: 1; }
        }
        @keyframes scaleIn {
          from { transform: scale(0.8); opacity: 0; }
          to { transform: scale(1); opacity: 1; }
        }
      `}</style>

      <div
        ref={containerRef}
        className={`relative cursor-grab select-none active:cursor-grabbing ${className}`}
        style={{
          width: containerSize,
          height: containerSize,
          perspective: `${perspective}px`,
        }}
        onMouseDown={handleMouseDown}
        onTouchStart={handleTouchStart}
        role="button"
        aria-label="Interactive 3D Image Sphere"
        tabIndex={0}
      >
        <div className="relative h-full w-full" style={{ zIndex: 10 }}>
          {images.map((image, index) => renderImageNode(image, index))}
        </div>
      </div>

      {renderSpotlightModal()}
    </>
  );
};

export default SphereImageGrid;
```
---

## Lazy Video <a name="lazy-video"></a>

- **Registry Key**: `lazy-video`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/lazy-video.json
```

#### Component Source Code
##### File: `ui/lazy-video.tsx`
```tsx
"use client";

import type React from "react";
import { useEffect, useRef, useState } from "react";

export interface LazyVideoProps
  extends React.VideoHTMLAttributes<HTMLVideoElement> {
  src: string;
  fallback?: string;
  threshold?: number;
  rootMargin?: string;
}

export function LazyVideo({
  src,
  className = "",
  poster,
  autoPlay = false,
  loop = false,
  muted = true,
  controls = false,
  playsInline = true,
  "aria-label": ariaLabel,
  ...props
}: LazyVideoProps) {
  const videoRef = useRef<HTMLVideoElement | null>(null);
  const [loaded, setLoaded] = useState(false);

  useEffect(() => {
    const el = videoRef.current;
    if (!el) return;

    const prefersReducedMotion =
      window.matchMedia?.("(prefers-reduced-motion: reduce)")?.matches ?? false;
    const saveData =
      (navigator as unknown as { connection?: { saveData?: boolean } })
        ?.connection?.saveData === true;
    const shouldAutoplay = autoPlay && !prefersReducedMotion && !saveData;

    let observer: IntersectionObserver | null = null;

    const onIntersect: IntersectionObserverCallback = (entries) => {
      entries.forEach(async (entry) => {
        if (entry.isIntersecting && !loaded) {
          el.src = src;
          el.load();

          if (shouldAutoplay) {
            const playVideo = async () => {
              try {
                await el.play();
              } catch (_error) {
                // Autoplay might be blocked
                // console.log("[v0] Autoplay blocked:", error)
              }
            };
            if (el.readyState >= 3) {
              playVideo();
            } else {
              el.addEventListener("canplay", playVideo, { once: true });
            }
          }

          setLoaded(true);
        } else if (!entry.isIntersecting && loaded && shouldAutoplay) {
          try {
            el.pause();
          } catch {}
        } else if (entry.isIntersecting && loaded && shouldAutoplay) {
          try {
            await el.play();
          } catch {}
        }
      });
    };

    observer = new IntersectionObserver(onIntersect, {
      rootMargin: "80px",
      threshold: 0.15,
    });
    observer.observe(el);

    const onVisibility = () => {
      if (!el) return;
      const hidden = document.visibilityState === "hidden";
      if (hidden) {
        try {
          el.pause();
        } catch {}
      } else if (shouldAutoplay && loaded) {
        // resume only if we were auto-playing
        el.play().catch(() => {});
      }
    };
    document.addEventListener("visibilitychange", onVisibility);

    return () => {
      document.removeEventListener("visibilitychange", onVisibility);
      observer?.disconnect();
    };
  }, [src, loaded, autoPlay]);

  return (
    <video
      ref={videoRef}
      className={className}
      muted={muted}
      loop={loop}
      playsInline={playsInline}
      controls={controls}
      preload="none"
      poster={poster}
      aria-label={ariaLabel}
      disableRemotePlayback
      {...props}
    >
      Your browser does not support the video tag.
    </video>
  );
}
```
---

## Liquid Metal Button <a name="liquid-metal-button"></a>

WebGL shader-powered liquid metal button effect for React. Create stunning 3D metallic animations with customizable colors and reflections.

- **Registry Key**: `liquid-metal-button`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/liquid-metal-button.json
```

#### Dependencies
- **NPM Packages**:
  - `lucide-react`
  - `@paper-design/shaders`

#### How to Use
```tsx
"use client";

import { LiquidMetalButton } from "@/registry/default/ui/liquid-metal-button";

export default function LiquidMetalButtonDemo() {
  return (
    <div className="flex flex-col items-center justify-center gap-8 p-8">
      <div className="flex items-center gap-8">
        <LiquidMetalButton label="Get Started" />
        <LiquidMetalButton viewMode="icon" />
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/liquid-metal-button.tsx`
```tsx
"use client";

import { Sparkles } from "lucide-react";
import type React from "react";
import { useEffect, useMemo, useRef, useState } from "react";

interface LiquidMetalButtonProps {
  label?: string;
  onClick?: () => void;
  viewMode?: "text" | "icon";
}

export function LiquidMetalButton({
  label = "Get Started",
  onClick,
  viewMode = "text",
}: LiquidMetalButtonProps) {
  const [isHovered, setIsHovered] = useState(false);
  const [isPressed, setIsPressed] = useState(false);
  const [ripples, setRipples] = useState<
    Array<{ x: number; y: number; id: number }>
  >([]);
  const shaderRef = useRef<HTMLDivElement>(null);
  // biome-ignore lint/suspicious/noExplicitAny: External library without types
  const shaderMount = useRef<any>(null);
  const buttonRef = useRef<HTMLButtonElement>(null);
  const rippleId = useRef(0);

  const dimensions = useMemo(() => {
    if (viewMode === "icon") {
      return {
        width: 46,
        height: 46,
        innerWidth: 42,
        innerHeight: 42,
        shaderWidth: 46,
        shaderHeight: 46,
      };
    } else {
      return {
        width: 142,
        height: 46,
        innerWidth: 138,
        innerHeight: 42,
        shaderWidth: 142,
        shaderHeight: 46,
      };
    }
  }, [viewMode]);

  useEffect(() => {
    const styleId = "shader-canvas-style-exploded";
    if (!document.getElementById(styleId)) {
      const style = document.createElement("style");
      style.id = styleId;
      style.textContent = `
        .shader-container-exploded canvas {
          width: 100% !important;
          height: 100% !important;
          display: block !important;
          position: absolute !important;
          top: 0 !important;
          left: 0 !important;
          border-radius: 100px !important;
        }
        @keyframes ripple-animation {
          0% {
            transform: translate(-50%, -50%) scale(0);
            opacity: 0.6;
          }
          100% {
            transform: translate(-50%, -50%) scale(4);
            opacity: 0;
          }
        }
      `;
      document.head.appendChild(style);
    }

    const loadShader = async () => {
      try {
        const { liquidMetalFragmentShader, ShaderMount } = await import(
          "@paper-design/shaders"
        );

        if (shaderRef.current) {
          if (shaderMount.current?.destroy) {
            shaderMount.current.destroy();
          }

          shaderMount.current = new ShaderMount(
            shaderRef.current,
            liquidMetalFragmentShader,
            {
              u_repetition: 4,
              u_softness: 0.5,
              u_shiftRed: 0.3,
              u_shiftBlue: 0.3,
              u_distortion: 0,
              u_contour: 0,
              u_angle: 45,
              u_scale: 8,
              u_shape: 1,
              u_offsetX: 0.1,
              u_offsetY: -0.1,
            },
            undefined,
            0.6,
          );
        }
      } catch (error) {
        console.error("[v0] Failed to load shader:", error);
      }
    };

    loadShader();

    return () => {
      if (shaderMount.current?.destroy) {
        shaderMount.current.destroy();
        shaderMount.current = null;
      }
    };
  }, []);

  const handleMouseEnter = () => {
    setIsHovered(true);
    shaderMount.current?.setSpeed?.(1);
  };

  const handleMouseLeave = () => {
    setIsHovered(false);
    setIsPressed(false);
    shaderMount.current?.setSpeed?.(0.6);
  };

  const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
    if (shaderMount.current?.setSpeed) {
      shaderMount.current.setSpeed(2.4);
      setTimeout(() => {
        if (isHovered) {
          shaderMount.current?.setSpeed?.(1);
        } else {
          shaderMount.current?.setSpeed?.(0.6);
        }
      }, 300);
    }

    if (buttonRef.current) {
      const rect = buttonRef.current.getBoundingClientRect();
      const x = e.clientX - rect.left;
      const y = e.clientY - rect.top;
      const ripple = { x, y, id: rippleId.current++ };

      setRipples((prev) => [...prev, ripple]);
      setTimeout(() => {
        setRipples((prev) => prev.filter((r) => r.id !== ripple.id));
      }, 600);
    }

    onClick?.();
  };

  return (
    <div className="relative inline-block">
      <div
        style={{
          perspective: "1000px",
          perspectiveOrigin: "50% 50%",
        }}
      >
        <div
          style={{
            position: "relative",
            width: `${dimensions.width}px`,
            height: `${dimensions.height}px`,
            transformStyle: "preserve-3d",
            transition:
              "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), width 0.4s ease, height 0.4s ease",
            transform: "none",
          }}
        >
          <div
            style={{
              position: "absolute",
              top: 0,
              left: 0,
              width: `${dimensions.width}px`,
              height: `${dimensions.height}px`,
              display: "flex",
              alignItems: "center",
              justifyContent: "center",
              gap: "6px",
              transformStyle: "preserve-3d",
              transition:
                "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), width 0.4s ease, height 0.4s ease, gap 0.4s ease",
              transform: "translateZ(20px)",
              zIndex: 30,
              pointerEvents: "none",
            }}
          >
            {viewMode === "icon" && (
              <Sparkles
                size={16}
                style={{
                  color: "#666666",
                  filter: "drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.5))",
                  transition: "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1)",
                  transform: "scale(1)",
                }}
              />
            )}
            {viewMode === "text" && (
              <span
                style={{
                  fontSize: "14px",
                  color: "#666666",
                  fontWeight: 400,
                  textShadow: "0px 1px 2px rgba(0, 0, 0, 0.5)",
                  transition: "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1)",
                  transform: "scale(1)",
                  whiteSpace: "nowrap",
                }}
              >
                {label}
              </span>
            )}
          </div>

          <div
            style={{
              position: "absolute",
              top: 0,
              left: 0,
              width: `${dimensions.width}px`,
              height: `${dimensions.height}px`,
              transformStyle: "preserve-3d",
              transition:
                "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), width 0.4s ease, height 0.4s ease",
              transform: `translateZ(10px) ${isPressed ? "translateY(1px) scale(0.98)" : "translateY(0) scale(1)"}`,
              zIndex: 20,
            }}
          >
            <div
              style={{
                width: `${dimensions.innerWidth}px`,
                height: `${dimensions.innerHeight}px`,
                margin: "2px",
                borderRadius: "100px",
                background: "linear-gradient(180deg, #202020 0%, #000000 100%)",
                boxShadow: isPressed
                  ? "inset 0px 2px 4px rgba(0, 0, 0, 0.4), inset 0px 1px 2px rgba(0, 0, 0, 0.3)"
                  : "none",
                transition:
                  "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), width 0.4s ease, height 0.4s ease, box-shadow 0.15s cubic-bezier(0.4, 0, 0.2, 1)",
              }}
            />
          </div>

          <div
            style={{
              position: "absolute",
              top: 0,
              left: 0,
              width: `${dimensions.width}px`,
              height: `${dimensions.height}px`,
              transformStyle: "preserve-3d",
              transition:
                "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), width 0.4s ease, height 0.4s ease",
              transform: `translateZ(0px) ${isPressed ? "translateY(1px) scale(0.98)" : "translateY(0) scale(1)"}`,
              zIndex: 10,
            }}
          >
            <div
              style={{
                height: `${dimensions.height}px`,
                width: `${dimensions.width}px`,
                borderRadius: "100px",
                boxShadow: isPressed
                  ? "0px 0px 0px 1px rgba(0, 0, 0, 0.5), 0px 1px 2px 0px rgba(0, 0, 0, 0.3)"
                  : isHovered
                    ? "0px 0px 0px 1px rgba(0, 0, 0, 0.4), 0px 12px 6px 0px rgba(0, 0, 0, 0.05), 0px 8px 5px 0px rgba(0, 0, 0, 0.1), 0px 4px 4px 0px rgba(0, 0, 0, 0.15), 0px 1px 2px 0px rgba(0, 0, 0, 0.2)"
                    : "0px 0px 0px 1px rgba(0, 0, 0, 0.3), 0px 36px 14px 0px rgba(0, 0, 0, 0.02), 0px 20px 12px 0px rgba(0, 0, 0, 0.08), 0px 9px 9px 0px rgba(0, 0, 0, 0.12), 0px 2px 5px 0px rgba(0, 0, 0, 0.15)",
                transition:
                  "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), width 0.4s ease, height 0.4s ease, box-shadow 0.15s cubic-bezier(0.4, 0, 0.2, 1)",
                background: "rgb(0 0 0 / 0)",
              }}
            >
              <div
                ref={shaderRef}
                className="shader-container-exploded"
                style={{
                  borderRadius: "100px",
                  overflow: "hidden",
                  position: "relative",
                  width: `${dimensions.shaderWidth}px`,
                  maxWidth: `${dimensions.shaderWidth}px`,
                  height: `${dimensions.shaderHeight}px`,
                  transition: "width 0.4s ease, height 0.4s ease",
                }}
              />
            </div>
          </div>

          <button
            ref={buttonRef}
            onClick={handleClick}
            onMouseEnter={handleMouseEnter}
            onMouseLeave={handleMouseLeave}
            onMouseDown={() => setIsPressed(true)}
            onMouseUp={() => setIsPressed(false)}
            style={{
              position: "absolute",
              top: 0,
              left: 0,
              width: `${dimensions.width}px`,
              height: `${dimensions.height}px`,
              background: "transparent",
              border: "none",
              cursor: "pointer",
              outline: "none",
              zIndex: 40,
              transformStyle: "preserve-3d",
              transform: "translateZ(25px)",
              transition:
                "all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), width 0.4s ease, height 0.4s ease",
              overflow: "hidden",
              borderRadius: "100px",
            }}
            aria-label={label}
          >
            {ripples.map((ripple) => (
              <span
                key={ripple.id}
                style={{
                  position: "absolute",
                  left: `${ripple.x}px`,
                  top: `${ripple.y}px`,
                  width: "20px",
                  height: "20px",
                  borderRadius: "50%",
                  background:
                    "radial-gradient(circle, rgba(255, 255, 255, 0.4) 0%, rgba(255, 255, 255, 0) 70%)",
                  pointerEvents: "none",
                  animation: "ripple-animation 0.6s ease-out",
                }}
              />
            ))}
          </button>
        </div>
      </div>
    </div>
  );
}
```
---

## Magnetic <a name="magnetic"></a>

Magnetic hover effect component for React. Elements smoothly follow cursor with spring physics. Perfect for buttons, cards, and interactive UI.

- **Registry Key**: `magnetic`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/magnetic.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
import { Magnetic } from "@/registry/default/ui/magnetic";

export default function MagneticDemo() {
  return (
    <div className="flex min-h-[300px] items-center justify-center">
      <Magnetic>
        <button className="rounded-lg bg-primary px-6 py-3 font-medium text-primary-foreground text-sm shadow-lg transition-colors hover:bg-primary/90">
          Hover Me
        </button>
      </Magnetic>
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/magnetic.tsx`
```tsx
"use client";

import {
  motion,
  type SpringOptions,
  useMotionValue,
  useSpring,
} from "motion/react";
import type React from "react";
import { useEffect, useRef, useState } from "react";

const SPRING_CONFIG = { stiffness: 26.7, damping: 4.1, mass: 0.2 };

export type MagneticProps = {
  children: React.ReactNode;
  intensity?: number;
  range?: number;
  actionArea?: "self" | "parent" | "global";
  springOptions?: SpringOptions;
};

export function Magnetic({
  children,
  intensity = 0.6,
  range = 100,
  actionArea = "self",
  springOptions = SPRING_CONFIG,
}: MagneticProps) {
  const [isHovered, setIsHovered] = useState(false);
  const ref = useRef<HTMLDivElement>(null);

  const x = useMotionValue(0);
  const y = useMotionValue(0);

  const springX = useSpring(x, springOptions);
  const springY = useSpring(y, springOptions);

  useEffect(() => {
    const calculateDistance = (e: MouseEvent) => {
      if (ref.current) {
        const rect = ref.current.getBoundingClientRect();
        const centerX = rect.left + rect.width / 2;
        const centerY = rect.top + rect.height / 2;
        const distanceX = e.clientX - centerX;
        const distanceY = e.clientY - centerY;

        const absoluteDistance = Math.sqrt(distanceX ** 2 + distanceY ** 2);

        if (isHovered && absoluteDistance <= range) {
          const scale = 1 - absoluteDistance / range;
          x.set(distanceX * intensity * scale);
          y.set(distanceY * intensity * scale);
        } else {
          x.set(0);
          y.set(0);
        }
      }
    };

    document.addEventListener("mousemove", calculateDistance);

    return () => {
      document.removeEventListener("mousemove", calculateDistance);
    };
  }, [isHovered, intensity, range, x.set, y.set, y, x]);

  useEffect(() => {
    if (actionArea === "parent" && ref.current?.parentElement) {
      const parent = ref.current.parentElement;

      const handleParentEnter = () => setIsHovered(true);
      const handleParentLeave = () => setIsHovered(false);

      parent.addEventListener("mouseenter", handleParentEnter);
      parent.addEventListener("mouseleave", handleParentLeave);

      return () => {
        parent.removeEventListener("mouseenter", handleParentEnter);
        parent.removeEventListener("mouseleave", handleParentLeave);
      };
    } else if (actionArea === "global") {
      setIsHovered(true);
    }
  }, [actionArea]);

  const handleMouseEnter = () => {
    if (actionArea === "self") {
      setIsHovered(true);
    }
  };

  const handleMouseLeave = () => {
    if (actionArea === "self") {
      setIsHovered(false);
      x.set(0);
      y.set(0);
    }
  };

  return (
    <motion.div
      ref={ref}
      onMouseEnter={actionArea === "self" ? handleMouseEnter : undefined}
      onMouseLeave={actionArea === "self" ? handleMouseLeave : undefined}
      style={{
        x: springX,
        y: springY,
      }}
    >
      {children}
    </motion.div>
  );
}
```
---

## Morphing Cursor <a name="morphing-cursor"></a>

A magnetic text effect component with a morphing cursor that reveals alternate text on hover. Creates an engaging interactive experience with smooth animations.

- **Registry Key**: `morphing-cursor`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/morphing-cursor.json
```

#### How to Use
```tsx
import { MagneticText } from "@/registry/default/ui/morphing-cursor";

export default function MorphingCursorDemo() {
  return (
    <div className="flex min-h-[300px] items-center justify-center">
      <MagneticText text="CREATIVE" hoverText="EXPLORE" />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/morphing-cursor.tsx`
```tsx
"use client";

import type React from "react";
import { useCallback, useEffect, useRef, useState } from "react";
import { cn } from "@/lib/utils";

interface MagneticTextProps {
  text: string;
  hoverText?: string;
  className?: string;
}

export function MagneticText({
  text = "CREATIVE",
  hoverText = "EXPLORE",
  className,
}: MagneticTextProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const circleRef = useRef<HTMLDivElement>(null);
  const innerTextRef = useRef<HTMLDivElement>(null);
  const [isHovered, setIsHovered] = useState(false);
  const [containerSize, setContainerSize] = useState({ width: 0, height: 0 });

  const mousePos = useRef({ x: 0, y: 0 });
  const currentPos = useRef({ x: 0, y: 0 });
  const animationFrameRef = useRef<number | undefined>(undefined);

  useEffect(() => {
    const updateSize = () => {
      if (containerRef.current) {
        setContainerSize({
          width: containerRef.current.offsetWidth,
          height: containerRef.current.offsetHeight,
        });
      }
    };
    updateSize();
    window.addEventListener("resize", updateSize);
    return () => window.removeEventListener("resize", updateSize);
  }, []);

  useEffect(() => {
    const lerp = (start: number, end: number, factor: number) =>
      start + (end - start) * factor;

    const animate = () => {
      currentPos.current.x = lerp(
        currentPos.current.x,
        mousePos.current.x,
        0.15,
      );
      currentPos.current.y = lerp(
        currentPos.current.y,
        mousePos.current.y,
        0.15,
      );

      if (circleRef.current) {
        circleRef.current.style.transform = `translate(${currentPos.current.x}px, ${currentPos.current.y}px) translate(-50%, -50%)`;
      }

      if (innerTextRef.current) {
        innerTextRef.current.style.transform = `translate(${-currentPos.current.x}px, ${-currentPos.current.y}px)`;
      }

      animationFrameRef.current = requestAnimationFrame(animate);
    };

    animationFrameRef.current = requestAnimationFrame(animate);
    return () => {
      if (animationFrameRef.current)
        cancelAnimationFrame(animationFrameRef.current);
    };
  }, []);

  const handleMouseMove = useCallback((e: React.MouseEvent<HTMLDivElement>) => {
    if (!containerRef.current) return;
    const rect = containerRef.current.getBoundingClientRect();
    mousePos.current = {
      x: e.clientX - rect.left,
      y: e.clientY - rect.top,
    };
  }, []);

  const handleMouseEnter = useCallback(
    (e: React.MouseEvent<HTMLDivElement>) => {
      if (!containerRef.current) return;
      const rect = containerRef.current.getBoundingClientRect();
      const x = e.clientX - rect.left;
      const y = e.clientY - rect.top;
      mousePos.current = { x, y };
      currentPos.current = { x, y };
      setIsHovered(true);
    },
    [],
  );

  const handleMouseLeave = useCallback(() => {
    setIsHovered(false);
  }, []);

  return (
    // biome-ignore lint/a11y/noStaticElementInteractions: This is a decorative visual effect that tracks mouse position for aesthetic purposes only
    <div
      ref={containerRef}
      onMouseMove={handleMouseMove}
      onMouseEnter={handleMouseEnter}
      onMouseLeave={handleMouseLeave}
      className={cn(
        "relative inline-flex cursor-none select-none items-center justify-center",
        className,
      )}
    >
      {/* Base text layer - original text */}
      <span className="font-bold text-5xl text-foreground tracking-tighter tracking-wide">
        {text}
      </span>

      <div
        ref={circleRef}
        className="pointer-events-none absolute top-0 left-0 overflow-hidden rounded-full bg-foreground"
        style={{
          width: isHovered ? 150 : 0,
          height: isHovered ? 150 : 0,
          transition:
            "width 0.5s cubic-bezier(0.33, 1, 0.68, 1), height 0.5s cubic-bezier(0.33, 1, 0.68, 1)",
          willChange: "transform, width, height",
        }}
      >
        <div
          ref={innerTextRef}
          className="absolute flex items-center justify-center"
          style={{
            width: containerSize.width,
            height: containerSize.height,
            top: "50%",
            left: "50%",
            willChange: "transform",
          }}
        >
          <span className="whitespace-nowrap font-bold text-5xl text-background tracking-tighter tracking-wide">
            {hoverText}
          </span>
        </div>
      </div>
    </div>
  );
}
```
---

## Number Counter <a name="number-counter"></a>

Animated number counter for React. Rolling digits, circular progress, and stats counters. Perfect for dashboards, pricing pages, and landing pages.

- **Registry Key**: `number-counter`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/number-counter.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
import { NumberCounter } from "@/registry/default/ui/number-counter";

export default function NumberCounterDemo() {
  return (
    <div className="flex w-full flex-col items-center justify-center gap-8 py-20">
      <div className="text-center">
        <h3 className="mb-2 font-medium text-muted-foreground text-sm">
          Basic Counter
        </h3>
        <NumberCounter
          value={1234.56}
          decimals={2}
          prefix="$"
          className="font-bold text-4xl"
        />
      </div>

      <div className="text-center">
        <h3 className="mb-2 font-medium text-muted-foreground text-sm">
          Large Number
        </h3>
        <NumberCounter
          value={1000000}
          prefix="+"
          className="font-bold text-4xl text-primary"
        />
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/number-counter.tsx`
```tsx
import { motion, useInView, useSpring, useTransform } from "motion/react";
import * as React from "react";
import { cn } from "@/lib/utils";

type EasingType =
  | "linear"
  | "easeOut"
  | "easeIn"
  | "easeInOut"
  | "spring"
  | "bounce";

interface NumberCounterProps {
  value: number;
  from?: number;
  duration?: number;
  delay?: number;
  decimals?: number;
  separator?: string;
  decimalSeparator?: string;
  prefix?: string;
  suffix?: string;
  easing?: EasingType;
  className?: string;
  once?: boolean;
  formatFn?: (value: number) => string;
}

// Easing functions for CSS-based animation
const easingFunctions: Record<EasingType, number[]> = {
  linear: [0, 0, 1, 1],
  easeOut: [0.16, 1, 0.3, 1],
  easeIn: [0.7, 0, 0.84, 0],
  easeInOut: [0.65, 0, 0.35, 1],
  spring: [0.34, 1.56, 0.64, 1],
  bounce: [0.68, -0.55, 0.27, 1.55],
};

// Format number with separators
const formatNumber = (
  value: number,
  decimals: number,
  separator: string,
  decimalSeparator: string,
): string => {
  const fixed = value.toFixed(decimals);
  const [intPart, decPart] = fixed.split(".");

  const formattedInt = (intPart || "").replace(
    /\B(?=(\d{3})+(?!\d))/g,
    separator,
  );

  return decPart
    ? `${formattedInt}${decimalSeparator}${decPart}`
    : formattedInt;
};

// Animated digit component for rolling effect
const AnimatedDigit = ({
  digit,
  delay = 0,
}: {
  digit: string;
  delay?: number;
}) => {
  const isNumber = /\d/.test(digit);

  if (!isNumber) {
    return <span className="inline-block">{digit}</span>;
  }

  const num = parseInt(digit, 10);

  return (
    <span className="relative inline-block h-[1em] w-[0.6em] overflow-hidden">
      <motion.span
        className="absolute top-0 left-0 flex w-full flex-col items-center"
        initial={{ y: "-100%" }}
        animate={{ y: `${-num * 10}%` }}
        transition={{
          duration: 0.5,
          delay,
          ease: [0.16, 1, 0.3, 1],
        }}
      >
        {[0, 1, 2, 3, 4, 5, 6, 7, 8, 9].map((n) => (
          <span key={n} className="h-[1em] leading-none">
            {n}
          </span>
        ))}
      </motion.span>
    </span>
  );
};

// Spring-based counter using useSpring
const SpringCounter = ({
  value,
  from = 0,
  decimals = 0,
  separator = ",",
  decimalSeparator = ".",
  prefix = "",
  suffix = "",
  className,
  once = true,
  formatFn,
  duration = 2,
  delay = 0,
}: NumberCounterProps) => {
  const ref = React.useRef(null);
  const isInView = useInView(ref, { once });
  const [displayValue, setDisplayValue] = React.useState(from);

  const springValue = useSpring(from, {
    stiffness: 100,
    damping: 30,
    duration: duration * 1000,
  });

  React.useEffect(() => {
    if (isInView) {
      const timer = setTimeout(() => {
        springValue.set(value);
      }, delay * 1000);
      return () => clearTimeout(timer);
    }
  }, [isInView, value, springValue, delay]);

  React.useEffect(() => {
    const unsubscribe = springValue.on("change", (latest) => {
      setDisplayValue(latest);
    });
    return unsubscribe;
  }, [springValue]);

  const formatted = formatFn
    ? formatFn(displayValue)
    : formatNumber(displayValue, decimals, separator, decimalSeparator);

  return (
    <span ref={ref} className={className}>
      {prefix}
      {formatted}
      {suffix}
    </span>
  );
};

// Main NumberCounter component
const NumberCounter = React.forwardRef<HTMLSpanElement, NumberCounterProps>(
  (
    {
      value,
      from = 0,
      duration = 2,
      delay = 0,
      decimals = 0,
      separator = ",",
      decimalSeparator = ".",
      prefix = "",
      suffix = "",
      easing = "easeOut",
      className,
      once = true,
      formatFn,
    },
    ref,
  ) => {
    const containerRef = React.useRef<HTMLSpanElement>(null);
    const isInView = useInView(containerRef, { once });
    const [count, setCount] = React.useState(from);

    React.useEffect(() => {
      if (!isInView) return;

      const startTime = Date.now() + delay * 1000;
      const endTime = startTime + duration * 1000;

      // Cubic bezier easing function
      const bezier = easingFunctions[easing] || easingFunctions.linear;
      const cubicBezier = (t: number): number => {
        const [x1 = 0, y1 = 0, x2 = 1, y2 = 1] = bezier;
        // Simplified cubic bezier approximation
        const cx = 3 * x1;
        const bx = 3 * (x2 - x1) - cx;
        const ax = 1 - cx - bx;
        const cy = 3 * y1;
        const by = 3 * (y2 - y1) - cy;
        const ay = 1 - cy - by;

        const sampleCurveX = (t: number) => ((ax * t + bx) * t + cx) * t;
        const sampleCurveY = (t: number) => ((ay * t + by) * t + cy) * t;

        // Newton-Raphson iteration for finding t given x
        let x = t;
        for (let i = 0; i < 8; i++) {
          const currentX = sampleCurveX(x) - t;
          if (Math.abs(currentX) < 0.0001) break;
          const dx = (3 * ax * x + 2 * bx) * x + cx;
          if (Math.abs(dx) < 0.0001) break;
          x -= currentX / dx;
        }

        return sampleCurveY(x);
      };

      let animationFrame: number;

      const animate = () => {
        const now = Date.now();

        if (now < startTime) {
          animationFrame = requestAnimationFrame(animate);
          return;
        }

        if (now >= endTime) {
          setCount(value);
          return;
        }

        const elapsed = now - startTime;
        const total = endTime - startTime;
        const progress = elapsed / total;
        const easedProgress = cubicBezier(progress);

        const currentValue = from + (value - from) * easedProgress;
        setCount(currentValue);

        animationFrame = requestAnimationFrame(animate);
      };

      animationFrame = requestAnimationFrame(animate);

      return () => cancelAnimationFrame(animationFrame);
    }, [isInView, value, from, duration, delay, easing]);

    const formatted = formatFn
      ? formatFn(count)
      : formatNumber(count, decimals, separator, decimalSeparator);

    return (
      <span ref={containerRef} className={cn("tabular-nums", className)}>
        {prefix}
        <span ref={ref}>{formatted}</span>
        {suffix}
      </span>
    );
  },
);

NumberCounter.displayName = "NumberCounter";

// Rolling digits counter
interface RollingCounterProps {
  value: number;
  className?: string;
  prefix?: string;
  suffix?: string;
  separator?: string;
}

const RollingCounter = React.forwardRef<HTMLSpanElement, RollingCounterProps>(
  ({ value, className, prefix = "", suffix = "", separator = "," }, ref) => {
    const containerRef = React.useRef<HTMLSpanElement>(null);
    const isInView = useInView(containerRef, { once: true });

    const formattedValue = formatNumber(value, 0, separator, ".");
    const digits = formattedValue.split("");

    return (
      <span
        ref={containerRef}
        className={cn("inline-flex tabular-nums", className)}
      >
        {prefix && <span>{prefix}</span>}
        <span ref={ref} className="inline-flex">
          {isInView &&
            digits.map((digit, index) => (
              <AnimatedDigit
                key={`${index}-${digit}`}
                digit={digit}
                delay={index * 0.05}
              />
            ))}
        </span>
        {suffix && <span>{suffix}</span>}
      </span>
    );
  },
);

RollingCounter.displayName = "RollingCounter";

// Percentage counter with circular progress
interface CircularCounterProps {
  value: number;
  size?: number;
  strokeWidth?: number;
  className?: string;
  duration?: number;
  color?: string;
  trackColor?: string;
}

const CircularCounter = React.forwardRef<HTMLDivElement, CircularCounterProps>(
  (
    {
      value,
      size = 120,
      strokeWidth = 8,
      className,
      duration = 2,
      color = "hsl(var(--primary))",
      trackColor = "hsl(var(--muted))",
    },
    ref,
  ) => {
    const containerRef = React.useRef<HTMLDivElement>(null);
    const isInView = useInView(containerRef, { once: true });

    const springValue = useSpring(0, {
      duration: duration * 1000,
      bounce: 0,
    });

    const radius = (size - strokeWidth) / 2;
    const circumference = radius * 2 * Math.PI;

    const offset = useTransform(springValue, [0, 100], [circumference, 0]);
    const rounded = useTransform(springValue, (latest) => Math.round(latest));

    React.useEffect(() => {
      if (isInView) {
        springValue.set(value);
      }
    }, [isInView, value, springValue]);

    return (
      <div
        ref={containerRef}
        className={cn(
          "relative inline-flex items-center justify-center",
          className,
        )}
        style={{ width: size, height: size }}
      >
        <svg width={size} height={size} className="-rotate-90">
          <circle
            cx={size / 2}
            cy={size / 2}
            r={radius}
            fill="none"
            stroke={trackColor}
            strokeWidth={strokeWidth}
          />
          <motion.circle
            cx={size / 2}
            cy={size / 2}
            r={radius}
            fill="none"
            stroke={color}
            strokeWidth={strokeWidth}
            strokeLinecap="round"
            strokeDasharray={circumference}
            style={{ strokeDashoffset: offset }}
          />
        </svg>
        <div
          ref={ref}
          className="absolute inset-0 flex items-center justify-center"
        >
          <span
            className="font-bold tabular-nums leading-none"
            style={{ fontSize: size * 0.22 }}
          >
            <motion.span>{rounded}</motion.span>%
          </span>
        </div>
      </div>
    );
  },
);

CircularCounter.displayName = "CircularCounter";

// Compact counter for stats
interface StatCounterProps {
  value: number;
  label: string;
  prefix?: string;
  suffix?: string;
  decimals?: number;
  className?: string;
}

const StatCounter = React.forwardRef<HTMLDivElement, StatCounterProps>(
  (
    { value, label, prefix = "", suffix = "", decimals = 0, className },
    ref,
  ) => {
    return (
      <div ref={ref} className={cn("text-center", className)}>
        <NumberCounter
          value={value}
          prefix={prefix}
          suffix={suffix}
          decimals={decimals}
          duration={2.5}
          easing="easeOut"
          className="font-bold text-4xl text-foreground"
        />
        <p className="mt-2 text-muted-foreground text-sm">{label}</p>
      </div>
    );
  },
);

StatCounter.displayName = "StatCounter";

export {
  CircularCounter,
  formatNumber,
  NumberCounter,
  RollingCounter,
  SpringCounter,
  StatCounter,
};
export type {
  CircularCounterProps,
  EasingType,
  NumberCounterProps,
  RollingCounterProps,
  StatCounterProps,
};
```
---

## Phone Card <a name="phone-card"></a>

iPhone mockup component for React. Realistic device frame with lazy-loaded video support. Perfect for app showcases and product demos.

- **Registry Key**: `phone-card`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/phone-card.json
```

#### How to Use
```tsx
import { PhoneCard } from "@/registry/default/ui/phone-card";

export default function PhoneCardDemo() {
  return (
    <div className="flex min-h-screen items-center justify-center p-8">
      <PhoneCard
        title="8°"
        sub="Clear night. Great for render farm runs."
        tone="calm"
        gradient="from-[#0f172a] via-[#14532d] to-[#052e16]"
        videoSrc="https://hebbkx1anhila5yf.public.blob.vercel-storage.com/A%20new%20chapter%20in%20the%20story%20of%20success.__Introducing%20the%20new%20TAG%20Heuer%20Carrera%20Day-Date%20collection%2C%20reimagined%20with%20bold%20colors%2C%20refined%20finishes%2C%20and%20upgraded%20functionality%20to%20keep%20you%20focused%20on%20your%20goals.%20__Six%20-nDNoRQyFaZ8oaaoty4XaQz8W8E5bqA.mp4"
        mediaType="video"
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/phone-card.tsx`
```tsx
"use client";
import { LazyVideo } from "./lazy-video";

export interface PhoneCardProps {
  title?: string;
  sub?: string;
  tone?: string;
  gradient?: string;
  videoSrc?: string;
  imageSrc?: string;
  mediaType?: "video" | "image";
}

export function PhoneCard({
  title = "8°",
  sub = "Clear night. Great for render farm runs.",
  tone = "calm",
  gradient = "from-[#0f172a] via-[#14532d] to-[#052e16]",
  videoSrc = "https://hebbkx1anhila5yf.public.blob.vercel-storage.com/A%20new%20chapter%20in%20the%20story%20of%20success.__Introducing%20the%20new%20TAG%20Heuer%20Carrera%20Day-Date%20collection%2C%20reimagined%20with%20bold%20colors%2C%20refined%20finishes%2C%20and%20upgraded%20functionality%20to%20keep%20you%20focused%20on%20your%20goals.%20__Six%20-nDNoRQyFaZ8oaaoty4XaQz8W8E5bqA.mp4",
  imageSrc = "https://www.jakala.com/hs-fs/hubfs/Vercel%20header-1.jpg?width=800&height=800&name=Vercel%20header-1.jpg",
  mediaType = "video",
}: PhoneCardProps) {
  return (
    <div className="glass-border relative rounded-[28px] bg-neutral-900 p-2">
      <div className="relative aspect-[9/19] w-full overflow-hidden rounded-2xl bg-black">
        {mediaType === "video" ? (
          <LazyVideo
            src={videoSrc}
            className="absolute inset-0 h-full w-full object-cover"
            autoPlay={true}
            loop={true}
            muted={true}
            playsInline={true}
            aria-label={`${title} - ${sub}`}
          />
        ) : (
          // biome-ignore lint/performance/noImgElement: next/image causes ESM issues with fumadocs-mdx
          <img
            src={imageSrc}
            alt={`${title} - ${sub}`}
            className="absolute inset-0 h-full w-full object-cover"
          />
        )}

        {/* Gradient overlay */}
        <div
          className={`absolute inset-0 bg-gradient-to-b ${gradient} opacity-60 mix-blend-overlay`}
        />

        <div className="relative z-10 p-3">
          <div className="mx-auto mb-3 h-1.5 w-16 rounded-full bg-white/20" />
          <div className="space-y-1 px-1">
            <div className="font-bold text-3xl text-white/90 leading-snug">
              {title}
            </div>
            <p className="text-white/70 text-xs">{sub}</p>
            <div className="mt-3 inline-flex items-center rounded-full bg-black/40 px-2 py-0.5 text-[10px] text-lime-300 uppercase tracking-wider">
              {tone === "calm" ? "Joly UI" : tone}
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
```
---

## Pricing 01 <a name="pricing-01"></a>

- **Registry Key**: `pricing-01`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/pricing-01.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`
  - `lucide-react`

#### How to Use
```tsx
import { Pricing } from "@/registry/default/ui/pricing-01"
import type { PricingPlan } from "@/registry/default/ui/pricing-01"

const plans: PricingPlan[] = [
  {
    name: "Starter",
    description: "Perfect for side projects and small teams",
    price: { monthly: 0, yearly: 0 },
    features: [
      "3 team members",
      "10 projects",
      "Basic analytics",
      "Community support",
      "1GB storage",
    ],
    cta: "Get Started",
    highlighted: false,
  },
  {
    name: "Pro",
    description: "For growing teams that need more power",
    price: { monthly: 29, yearly: 24 },
    features: [
      "Unlimited team members",
      "Unlimited projects",
      "Advanced analytics",
      "Priority support",
      "100GB storage",
      "Custom domains",
      "API access",
    ],
    cta: "Start Free Trial",
    highlighted: true,
  },
  {
    name: "Enterprise",
    description: "For organizations with advanced needs",
    price: { monthly: 99, yearly: 79 },
    features: [
      "Everything in Pro",
      "SSO & SAML",
      "Dedicated support",
      "SLA guarantee",
      "Unlimited storage",
      "Custom integrations",
      "Audit logs",
    ],
    cta: "Contact Sales",
    highlighted: false,
  },
]

export default function PricingDemo() {
  return (
    <Pricing
      plans={plans}
      title="Simple, transparent pricing"
      subtitle="Start free, scale as you grow. No hidden fees, no surprises."
      onPlanSelect={(plan) => console.log("Selected:", plan.name)}
    />
  )
}
```
#### Component Source Code
##### File: `ui/pricing-01.tsx`
```tsx
"use client"

import { motion, useInView } from "framer-motion"
import { Check } from "lucide-react"
import { useRef, useState } from "react"

// ─── Types ───────────────────────────────────────────────────────────────────

export interface PricingPlan {
  /** Plan name displayed as the card title */
  name: string
  /** Short description of the plan */
  description: string
  /** Price per billing cycle */
  price: { monthly: number; yearly: number }
  /** List of features included in the plan */
  features: string[]
  /** Call-to-action button label */
  cta: string
  /** Whether this plan card is visually highlighted */
  highlighted?: boolean
}

export interface PricingProps {
  /** Array of pricing plans to display */
  plans: PricingPlan[]
  /** Section heading */
  title?: string
  /** Section subtitle */
  subtitle?: string
  /** Default billing cycle */
  defaultBillingCycle?: "monthly" | "yearly"
  /** Label for the yearly discount badge */
  yearlyDiscountLabel?: string
  /** Label shown on the highlighted plan badge */
  highlightedBadgeLabel?: string
  /** Callback when a plan CTA button is clicked */
  onPlanSelect?: (plan: PricingPlan) => void
  /** Additional CSS classes for the root section element */
  className?: string
}

// ─── Sub-components ──────────────────────────────────────────────────────────

function BorderBeam() {
  return (
    <div className="absolute inset-0 rounded-2xl overflow-hidden pointer-events-none">
      <div
        className="absolute w-24 h-24 bg-white/20 blur-xl border-beam"
        style={{
          offsetPath: "rect(0 100% 100% 0 round 16px)",
        }}
      />
    </div>
  )
}

function BillingToggle({
  billingCycle,
  onToggle,
  yearlyDiscountLabel,
}: {
  billingCycle: "monthly" | "yearly"
  onToggle: (cycle: "monthly" | "yearly") => void
  yearlyDiscountLabel: string
}) {
  return (
    <div className="inline-flex items-center p-1 rounded-full bg-zinc-900 border border-zinc-800">
      <button
        onClick={() => onToggle("monthly")}
        className={`relative px-4 py-2 text-sm font-medium rounded-full transition-colors ${
          billingCycle === "monthly" ? "text-white" : "text-zinc-400"
        }`}
      >
        {billingCycle === "monthly" && (
          <motion.div
            layoutId="billing-toggle"
            className="absolute inset-0 bg-zinc-800 rounded-full"
            transition={{ type: "spring", stiffness: 500, damping: 30 }}
          />
        )}
        <span className="relative z-10">Monthly</span>
      </button>
      <button
        onClick={() => onToggle("yearly")}
        className={`relative px-4 py-2 text-sm font-medium rounded-full transition-colors ${
          billingCycle === "yearly" ? "text-white" : "text-zinc-400"
        }`}
      >
        {billingCycle === "yearly" && (
          <motion.div
            layoutId="billing-toggle"
            className="absolute inset-0 bg-zinc-800 rounded-full"
            transition={{ type: "spring", stiffness: 500, damping: 30 }}
          />
        )}
        <span className="relative z-10">Yearly</span>
        <span className="relative z-10 ml-2 px-2 py-0.5 text-xs bg-emerald-500/20 text-emerald-400 rounded-full">
          {yearlyDiscountLabel}
        </span>
      </button>
    </div>
  )
}

function PricingCard({
  plan,
  billingCycle,
  highlightedBadgeLabel,
  onSelect,
  index,
  isInView,
}: {
  plan: PricingPlan
  billingCycle: "monthly" | "yearly"
  highlightedBadgeLabel: string
  onSelect?: (plan: PricingPlan) => void
  index: number
  isInView: boolean
}) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={isInView ? { opacity: 1, y: 0 } : {}}
      transition={{ duration: 0.6, delay: 0.3 + index * 0.1 }}
      className={`relative p-6 rounded-2xl border transition-all duration-300 hover:scale-[1.02] ${
        plan.highlighted
          ? "bg-zinc-900 border-zinc-700"
          : "bg-zinc-900/50 border-zinc-800 hover:border-zinc-600"
      }`}
    >
      {plan.highlighted && <BorderBeam />}

      {plan.highlighted && (
        <div className="absolute -top-3 left-1/2 -translate-x-1/2 px-3 py-1 bg-white text-zinc-950 text-xs font-medium rounded-full">
          {highlightedBadgeLabel}
        </div>
      )}

      <div className="mb-6">
        <h3 className="text-xl font-semibold text-white mb-2">{plan.name}</h3>
        <p className="text-zinc-400 text-sm">{plan.description}</p>
      </div>

      <div className="mb-6">
        <div className="flex items-baseline gap-1">
          <span className="text-4xl font-bold text-white">${plan.price[billingCycle]}</span>
          {plan.price.monthly > 0 && <span className="text-zinc-400 text-sm">/month</span>}
        </div>
        {billingCycle === "yearly" && plan.price.yearly > 0 && (
          <p className="text-xs text-zinc-500 mt-1">Billed annually (${plan.price.yearly * 12}/year)</p>
        )}
      </div>

      <ul className="space-y-3 mb-8">
        {plan.features.map((feature) => (
          <li key={feature} className="flex items-center gap-3 text-sm text-zinc-300">
            <Check className="w-4 h-4 text-emerald-500 shrink-0" strokeWidth={1.5} />
            {feature}
          </li>
        ))}
      </ul>

      <button
        onClick={() => onSelect?.(plan)}
        className={`w-full rounded-full py-2 px-4 text-sm font-medium transition-colors cursor-pointer ${
          plan.highlighted
            ? "shimmer-btn bg-white text-zinc-950 hover:bg-zinc-200"
            : "bg-zinc-800 text-white hover:bg-zinc-700 border border-zinc-700"
        }`}
      >
        {plan.cta}
      </button>
    </motion.div>
  )
}

// ─── Main Component ──────────────────────────────────────────────────────────

export function Pricing({
  plans,
  title = "Simple, transparent pricing",
  subtitle = "Start free, scale as you grow. No hidden fees, no surprises.",
  defaultBillingCycle = "monthly",
  yearlyDiscountLabel = "-20%",
  highlightedBadgeLabel = "Most Popular",
  onPlanSelect,
  className,
}: PricingProps) {
  const ref = useRef(null)
  const isInView = useInView(ref, { once: true, margin: "-100px" })
  const [billingCycle, setBillingCycle] = useState<"monthly" | "yearly">(defaultBillingCycle)

  return (
    <section id="pricing" className={`py-24 px-4 ${className ?? ""}`}>
      <div className="max-w-6xl mx-auto">
        <motion.div
          initial={{ opacity: 0, y: 20 }}
          animate={isInView ? { opacity: 1, y: 0 } : {}}
          transition={{ duration: 0.6 }}
          className="text-center mb-12"
        >
          <h2 className="text-3xl sm:text-4xl font-bold text-white mb-4">
            {title}
          </h2>
          <p className="text-zinc-400 max-w-2xl mx-auto mb-8">
            {subtitle}
          </p>

          <BillingToggle
            billingCycle={billingCycle}
            onToggle={setBillingCycle}
            yearlyDiscountLabel={yearlyDiscountLabel}
          />
        </motion.div>

        <motion.div
          ref={ref}
          initial={{ opacity: 0, y: 40 }}
          animate={isInView ? { opacity: 1, y: 0 } : {}}
          transition={{ duration: 0.6, delay: 0.2 }}
          className="grid grid-cols-1 md:grid-cols-3 gap-6"
        >
          {plans.map((plan, index) => (
            <PricingCard
              key={plan.name}
              plan={plan}
              billingCycle={billingCycle}
              highlightedBadgeLabel={highlightedBadgeLabel}
              onSelect={onPlanSelect}
              index={index}
              isInView={isInView}
            />
          ))}
        </motion.div>
      </div>
    </section>
  )
}
```
---

## Rainbow Button <a name="rainbow-button"></a>

Animated rainbow gradient border button for React. Eye-catching CTA component with smooth color transitions and hover effects.

- **Registry Key**: `rainbow-button`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/rainbow-button.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
"use client";

import { ArrowRight } from "lucide-react";
import { RainbowButton } from "@/registry/default/ui/rainbow-button";

export default function RainbowButtonDemo() {
  return (
    <div className="flex items-center justify-center p-8">
      <RainbowButton className="group">
        Get Started
        <ArrowRight className="ml-2 h-4 w-4 transition-transform group-hover:translate-x-1" />
      </RainbowButton>
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/rainbow-button.tsx`
```tsx
import { motion } from "motion/react";
import * as React from "react";
import { cn } from "@/lib/utils";

interface RainbowButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  children: React.ReactNode;
  colors?: string[];
  duration?: number;
  borderWidth?: number;
  animated?: boolean;
}

const RainbowButton = React.forwardRef<HTMLButtonElement, RainbowButtonProps>(
  (
    {
      children,
      colors = ["#f43f5e", "#8b5cf6", "#3b82f6", "#22c55e", "#f43f5e"],
      duration = 2,
      borderWidth = 2,
      animated = true,
      className,
      onClick,
      disabled,
      type = "button",
      ...props
    },
    ref,
  ) => {
    const gradientColors = colors.join(", ");

    return (
      <button
        ref={ref}
        type={type}
        onClick={onClick}
        disabled={disabled}
        className={cn(
          "relative inline-flex items-center justify-center overflow-hidden rounded-lg font-medium transition-transform hover:scale-105 active:scale-95 disabled:pointer-events-none disabled:opacity-50",
          className,
        )}
        style={{ padding: borderWidth }}
        {...props}
      >
        {/* Animated gradient background */}
        <motion.div
          className="absolute inset-0"
          style={{
            background: `linear-gradient(var(--gradient-angle, 0deg), ${gradientColors})`,
          }}
          animate={
            animated
              ? {
                  "--gradient-angle": ["0deg", "360deg"],
                }
              : undefined
          }
          transition={
            animated
              ? {
                  duration,
                  repeat: Infinity,
                  ease: "linear",
                }
              : undefined
          }
        />
        {/* Button content */}
        <span
          className="relative z-10 flex items-center gap-2 rounded-md bg-background px-6 py-2.5 font-medium text-sm transition-colors hover:bg-background/90"
          style={{
            borderRadius: `calc(0.5rem - ${borderWidth}px)`,
          }}
        >
          {children}
        </span>
      </button>
    );
  },
);
RainbowButton.displayName = "RainbowButton";

export { RainbowButton };
```
---

## Registry <a name="registry"></a>

- **Registry Key**: `registry`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/registry.json
```

---

## Rotate Text <a name="rotate-text"></a>

Word rotation animation for React. Smooth vertical flip transitions with spring physics. Great for hero headlines and dynamic taglines.

- **Registry Key**: `rotate-text`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/rotate-text.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
"use client";

import { RotatingText } from "@/registry/default/ui/rotate-text";

export default function RotateTextDemo() {
  return (
    <div className="flex items-center justify-center">
      <RotatingText
        words={["Hello", "World", "Rotate", "Text"]}
        className="font-bold text-4xl text-foreground"
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/rotate-text.tsx`
```tsx
import { AnimatePresence, motion } from "motion/react";
import * as React from "react";
import { cn } from "@/lib/utils";

interface RotatingTextProps {
  words: string[];
  className?: string;
  interval?: number;
}

const RotatingText = React.forwardRef<HTMLSpanElement, RotatingTextProps>(
  ({ words, className, interval = 2000 }, ref) => {
    const [currentIndex, setCurrentIndex] = React.useState(0);

    React.useEffect(() => {
      const timer = setInterval(() => {
        setCurrentIndex((prev) => (prev + 1) % words.length);
      }, interval);
      return () => clearInterval(timer);
    }, [words.length, interval]);

    return (
      <span
        ref={ref}
        className={cn("relative inline-flex overflow-hidden", className)}
      >
        <AnimatePresence mode="popLayout">
          <motion.span
            key={currentIndex}
            initial={{ y: "100%", opacity: 0 }}
            animate={{ y: 0, opacity: 1 }}
            exit={{ y: "-100%", opacity: 0 }}
            transition={{
              type: "spring",
              stiffness: 300,
              damping: 30,
            }}
            className="inline-block"
          >
            {words[currentIndex]}
          </motion.span>
        </AnimatePresence>
      </span>
    );
  },
);
RotatingText.displayName = "RotatingText";

export { RotatingText };
```
---

## Scroll Text <a name="scroll-text"></a>

Scroll-triggered text animations for React. Includes fade, blur, scale, parallax, and sticky reveal effects. Built with Framer Motion.

- **Registry Key**: `scroll-text`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/scroll-text.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
import { ScrollText } from "@/registry/default/ui/scroll-text";

export default function ScrollTextDemo() {
  return (
    <div className="flex w-full flex-col items-center justify-center gap-32 py-20">
      <ScrollText effect="fadeUp" className="font-bold text-4xl">
        Fade Up Animation
      </ScrollText>

      <ScrollText effect="blur" className="font-bold text-4xl">
        Blur Animation
      </ScrollText>

      <ScrollText effect="scale" className="font-bold text-4xl">
        Scale Animation
      </ScrollText>

      <ScrollText effect="rotate" className="font-bold text-4xl">
        Rotate Animation
      </ScrollText>

      <ScrollText effect="slideLeft" className="font-bold text-4xl">
        Slide Left Animation
      </ScrollText>
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/scroll-text.tsx`
```tsx
import {
  type MotionValue,
  motion,
  useScroll,
  useSpring,
  useTransform,
} from "motion/react";
import * as React from "react";
import { cn } from "@/lib/utils";

// ============================================================================
// TYPES
// ============================================================================

type ScrollEffect =
  | "fadeIn"
  | "fadeUp"
  | "fadeDown"
  | "parallax"
  | "scale"
  | "scaleUp"
  | "scaleDown"
  | "rotate"
  | "blur"
  | "slideLeft"
  | "slideRight"
  | "skew"
  | "flip"
  | "reveal";

interface ScrollTextProps {
  children: React.ReactNode;
  effect?: ScrollEffect;
  className?: string;
  offset?: [string, string];
  speed?: number;
  spring?: boolean;
  springConfig?: { stiffness?: number; damping?: number; mass?: number };
  threshold?: [number, number];
}

interface ParallaxTextProps {
  children: React.ReactNode;
  className?: string;
  speed?: number;
  direction?: "up" | "down" | "left" | "right";
  spring?: boolean;
}

interface ScrollRevealProps {
  children: React.ReactNode;
  className?: string;
  direction?: "up" | "down" | "left" | "right";
  delay?: number;
  duration?: number;
  distance?: number;
  once?: boolean;
}

interface ScrollFadeProps {
  children: React.ReactNode;
  className?: string;
  fadeIn?: boolean;
  fadeOut?: boolean;
  threshold?: [number, number];
}

interface ScrollScaleProps {
  children: React.ReactNode;
  className?: string;
  from?: number;
  to?: number;
  threshold?: [number, number];
}

interface HorizontalScrollTextProps {
  children: React.ReactNode;
  className?: string;
  speed?: number;
  direction?: "left" | "right";
  repeat?: number;
}

interface ScrollProgressTextProps {
  text: string;
  className?: string;
  charClassName?: string;
  stagger?: number;
}

interface StickyScrollTextProps {
  children: React.ReactNode;
  className?: string;
  height?: string;
  effect?: "fade" | "scale" | "blur" | "color";
}

// ============================================================================
// SCROLL TEXT COMPONENT
// ============================================================================

const ScrollText = React.forwardRef<HTMLDivElement, ScrollTextProps>(
  (
    {
      children,
      effect = "fadeIn",
      className,
      offset = ["start end", "end start"],
      speed = 1,
      spring = false,
      springConfig,
      threshold = [0, 1],
    },
    forwardedRef,
  ) => {
    const internalRef = React.useRef<HTMLDivElement>(null);
    const ref =
      (forwardedRef as React.RefObject<HTMLDivElement>) || internalRef;

    const { scrollYProgress } = useScroll({
      target: ref,
      offset: offset as ["start end", "end start"],
    });

    const [start, end] = threshold;
    const springOpts = {
      stiffness: springConfig?.stiffness ?? 100,
      damping: springConfig?.damping ?? 30,
      mass: springConfig?.mass ?? 1,
    };

    // Create transforms
    const rawOpacity = useTransform(scrollYProgress, [start, end], [0, 1]);
    const rawY = useTransform(scrollYProgress, [start, end], [50 * speed, 0]);
    const rawYDown = useTransform(
      scrollYProgress,
      [start, end],
      [-50 * speed, 0],
    );
    const rawYParallax = useTransform(
      scrollYProgress,
      [0, 1],
      [100 * speed, -100 * speed],
    );
    const rawScale = useTransform(scrollYProgress, [start, end], [0.5, 1]);
    const rawScaleUp = useTransform(
      scrollYProgress,
      [0, 0.5, 1],
      [0.8, 1, 1.2],
    );
    const rawScaleDown = useTransform(
      scrollYProgress,
      [0, 0.5, 1],
      [1.2, 1, 0.8],
    );
    const rawRotate = useTransform(scrollYProgress, [0, 1], [0, 360 * speed]);
    const rawBlurOpacity = useTransform(
      scrollYProgress,
      [start, end],
      [0.5, 1],
    );
    const rawX = useTransform(scrollYProgress, [start, end], [200 * speed, 0]);
    const rawXRight = useTransform(
      scrollYProgress,
      [start, end],
      [-200 * speed, 0],
    );
    const rawSkew = useTransform(
      scrollYProgress,
      [start, end],
      [20 * speed, 0],
    );
    const rawRotateX = useTransform(scrollYProgress, [start, end], [90, 0]);

    // Apply spring if needed
    const opacitySpring = useSpring(rawOpacity, springOpts);
    const ySpring = useSpring(rawY, springOpts);
    const yDownSpring = useSpring(rawYDown, springOpts);
    const yParallaxSpring = useSpring(rawYParallax, springOpts);
    const scaleSpring = useSpring(rawScale, springOpts);
    const scaleUpSpring = useSpring(rawScaleUp, springOpts);
    const scaleDownSpring = useSpring(rawScaleDown, springOpts);
    const rotateSpring = useSpring(rawRotate, springOpts);
    const blurOpacitySpring = useSpring(rawBlurOpacity, springOpts);
    const rotateXSpring = useSpring(rawRotateX, springOpts);

    const opacity = spring ? opacitySpring : rawOpacity;
    const y = spring ? ySpring : rawY;
    const yDown = spring ? yDownSpring : rawYDown;
    const yParallax = spring ? yParallaxSpring : rawYParallax;
    const scale = spring ? scaleSpring : rawScale;
    const scaleUp = spring ? scaleUpSpring : rawScaleUp;
    const scaleDown = spring ? scaleDownSpring : rawScaleDown;
    const rotate = spring ? rotateSpring : rawRotate;
    const blurOpacity = spring ? blurOpacitySpring : rawBlurOpacity;
    const rotateX = spring ? rotateXSpring : rawRotateX;
    const xSpring = useSpring(rawX, springOpts);
    const xRightSpring = useSpring(rawXRight, springOpts);
    const skewSpring = useSpring(rawSkew, springOpts);

    const x = spring ? xSpring : rawX;
    const xRight = spring ? xRightSpring : rawXRight;
    const skew = spring ? skewSpring : rawSkew;

    // Non-springable transforms
    const blur = useTransform(scrollYProgress, [start, end], [10 * speed, 0]);
    const clipPath = useTransform(scrollYProgress, [start, end], [100, 0]);

    const effectStyles: Record<
      ScrollEffect,
      Record<string, MotionValue<number>>
    > = {
      fadeIn: { opacity },
      fadeUp: { opacity, y },
      fadeDown: { opacity, y: yDown },
      parallax: { y: yParallax },
      scale: { scale, opacity },
      scaleUp: { scale: scaleUp },
      scaleDown: { scale: scaleDown },
      rotate: { rotate },
      blur: { opacity: blurOpacity },
      slideLeft: { x, opacity },
      slideRight: { x: xRight, opacity },
      skew: { skewX: skew, opacity },
      flip: { rotateX, opacity },
      reveal: {},
    };

    const filterBlur = useTransform(blur, (v) => `blur(${v}px)`);
    const clipPathValue = useTransform(clipPath, (v) => `inset(0 ${v}% 0 0)`);

    return (
      <motion.div
        ref={ref}
        className={cn("will-change-transform", className)}
        style={{
          ...effectStyles[effect],
          ...(effect === "blur" && { filter: filterBlur }),
          ...(effect === "reveal" && { clipPath: clipPathValue }),
          ...(effect === "flip" && { perspective: 1000 }),
        }}
      >
        {children}
      </motion.div>
    );
  },
);

ScrollText.displayName = "ScrollText";

// ============================================================================
// PARALLAX TEXT COMPONENT
// ============================================================================

const ParallaxText = React.forwardRef<HTMLDivElement, ParallaxTextProps>(
  (
    { children, className, speed = 1, direction = "up", spring = true },
    ref,
  ) => {
    const containerRef = React.useRef<HTMLDivElement>(null);
    const { scrollYProgress } = useScroll({
      target: containerRef,
      offset: ["start end", "end start"],
    });

    const distance = 100 * speed;
    const springOpts = spring
      ? { stiffness: 100, damping: 30 }
      : { stiffness: 1000, damping: 1000 };

    const yUp = useSpring(
      useTransform(scrollYProgress, [0, 1], [distance, -distance]),
      springOpts,
    );
    const yDown = useSpring(
      useTransform(scrollYProgress, [0, 1], [-distance, distance]),
      springOpts,
    );
    const xLeft = useSpring(
      useTransform(scrollYProgress, [0, 1], [distance, -distance]),
      springOpts,
    );
    const xRight = useSpring(
      useTransform(scrollYProgress, [0, 1], [-distance, distance]),
      springOpts,
    );

    const isHorizontal = direction === "left" || direction === "right";
    const transform = {
      up: yUp,
      down: yDown,
      left: xLeft,
      right: xRight,
    }[direction];

    return (
      <div ref={containerRef} className={cn("overflow-hidden", className)}>
        <motion.div
          ref={ref}
          style={{ [isHorizontal ? "x" : "y"]: transform }}
          className="will-change-transform"
        >
          {children}
        </motion.div>
      </div>
    );
  },
);

ParallaxText.displayName = "ParallaxText";

// ============================================================================
// SCROLL REVEAL COMPONENT
// ============================================================================

const ScrollReveal = React.forwardRef<HTMLDivElement, ScrollRevealProps>(
  (
    {
      children,
      className,
      direction = "up",
      delay = 0,
      duration = 0.6,
      distance = 60,
      once = true,
    },
    ref,
  ) => {
    const containerRef = React.useRef<HTMLDivElement>(null);
    const [isInView, setIsInView] = React.useState(false);

    React.useEffect(() => {
      const observer = new IntersectionObserver(
        ([entry]) => {
          if (entry?.isIntersecting) {
            setIsInView(true);
            if (once && containerRef.current) {
              observer.unobserve(containerRef.current);
            }
          } else if (!once) {
            setIsInView(false);
          }
        },
        { threshold: 0.1 },
      );

      if (containerRef.current) {
        observer.observe(containerRef.current);
      }

      return () => observer.disconnect();
    }, [once]);

    const getInitialPosition = () => {
      switch (direction) {
        case "up":
          return { y: distance, x: 0 };
        case "down":
          return { y: -distance, x: 0 };
        case "left":
          return { x: distance, y: 0 };
        case "right":
          return { x: -distance, y: 0 };
      }
    };

    const initial = getInitialPosition();

    return (
      <div ref={containerRef} className={className}>
        <motion.div
          ref={ref}
          initial={{ opacity: 0, ...initial }}
          animate={
            isInView ? { opacity: 1, x: 0, y: 0 } : { opacity: 0, ...initial }
          }
          transition={{ duration, delay, ease: [0.25, 0.46, 0.45, 0.94] }}
        >
          {children}
        </motion.div>
      </div>
    );
  },
);

ScrollReveal.displayName = "ScrollReveal";

// ============================================================================
// SCROLL FADE COMPONENT
// ============================================================================

const ScrollFade = React.forwardRef<HTMLDivElement, ScrollFadeProps>(
  (
    {
      children,
      className,
      fadeIn = true,
      fadeOut = true,
      threshold = [0.2, 0.8],
    },
    ref,
  ) => {
    const containerRef = React.useRef<HTMLDivElement>(null);
    const { scrollYProgress } = useScroll({
      target: containerRef,
      offset: ["start end", "end start"],
    });

    const opacity = useTransform(scrollYProgress, (value) => {
      if (fadeIn && fadeOut) {
        if (value < threshold[0]) return value / threshold[0];
        if (value > threshold[1])
          return 1 - (value - threshold[1]) / (1 - threshold[1]);
        return 1;
      }
      if (fadeIn) return Math.min(value / threshold[0], 1);
      if (fadeOut)
        return value > threshold[1]
          ? 1 - (value - threshold[1]) / (1 - threshold[1])
          : 1;
      return 1;
    });

    return (
      <motion.div ref={containerRef} className={className} style={{ opacity }}>
        <div ref={ref}>{children}</div>
      </motion.div>
    );
  },
);

ScrollFade.displayName = "ScrollFade";

// ============================================================================
// SCROLL SCALE COMPONENT
// ============================================================================

const ScrollScale = React.forwardRef<HTMLDivElement, ScrollScaleProps>(
  ({ children, className, from = 0.8, to = 1, threshold = [0, 0.5] }, ref) => {
    const containerRef = React.useRef<HTMLDivElement>(null);
    const { scrollYProgress } = useScroll({
      target: containerRef,
      offset: ["start end", "end start"],
    });

    const scale = useSpring(
      useTransform(scrollYProgress, threshold, [from, to]),
      { stiffness: 100, damping: 30 },
    );
    const opacity = useTransform(scrollYProgress, threshold, [0, 1]);

    return (
      <motion.div
        ref={containerRef}
        className={cn("will-change-transform", className)}
        style={{ scale, opacity }}
      >
        <div ref={ref}>{children}</div>
      </motion.div>
    );
  },
);

ScrollScale.displayName = "ScrollScale";

// ============================================================================
// HORIZONTAL SCROLL TEXT COMPONENT
// ============================================================================

const HorizontalScrollText = React.forwardRef<
  HTMLDivElement,
  HorizontalScrollTextProps
>(({ children, className, speed = 1, direction = "left", repeat = 3 }, ref) => {
  const containerRef = React.useRef<HTMLDivElement>(null);
  const { scrollYProgress } = useScroll({
    target: containerRef,
    offset: ["start end", "end start"],
  });

  const x = useTransform(
    scrollYProgress,
    [0, 1],
    direction === "left"
      ? ["0%", `-${50 * speed}%`]
      : [`-${50 * speed}%`, "0%"],
  );

  return (
    <div ref={containerRef} className={cn("overflow-hidden", className)}>
      <motion.div
        ref={ref}
        className="flex whitespace-nowrap will-change-transform"
        style={{ x }}
      >
        {Array.from({ length: repeat }).map((_, i) => (
          <span key={i} className="flex-shrink-0 px-4">
            {children}
          </span>
        ))}
      </motion.div>
    </div>
  );
});

HorizontalScrollText.displayName = "HorizontalScrollText";

// ============================================================================
// SCROLL PROGRESS TEXT COMPONENT
// ============================================================================

interface ScrollProgressCharProps {
  char: string;
  progress: MotionValue<number>;
  range: [number, number];
  className?: string;
}

const ScrollProgressChar: React.FC<ScrollProgressCharProps> = ({
  char,
  progress,
  range,
  className,
}) => {
  const opacity = useTransform(progress, range, [0.2, 1]);
  const color = useTransform(progress, range, [
    "hsl(var(--muted-foreground))",
    "hsl(var(--foreground))",
  ]);

  return (
    <motion.span
      style={{ opacity, color }}
      className={cn("transition-colors", className)}
    >
      {char === " " ? "\u00A0" : char}
    </motion.span>
  );
};

const ScrollProgressText = React.forwardRef<
  HTMLDivElement,
  ScrollProgressTextProps
>(({ text, className, charClassName }, ref) => {
  const containerRef = React.useRef<HTMLDivElement>(null);
  const { scrollYProgress } = useScroll({
    target: containerRef,
    offset: ["start 0.9", "start 0.25"],
  });

  const characters = text.split("");
  const step = 1 / characters.length;

  return (
    <div ref={containerRef} className={className}>
      <span ref={ref} className="flex flex-wrap">
        {characters.map((char, index) => {
          const start = index * step;
          const end = start + step;
          return (
            <ScrollProgressChar
              key={index}
              char={char}
              progress={scrollYProgress}
              range={[start, end]}
              className={charClassName}
            />
          );
        })}
      </span>
    </div>
  );
});

ScrollProgressText.displayName = "ScrollProgressText";

// ============================================================================
// STICKY SCROLL TEXT COMPONENT
// ============================================================================

const StickyScrollText = React.forwardRef<
  HTMLDivElement,
  StickyScrollTextProps
>(({ children, className, height = "200vh", effect = "fade" }, ref) => {
  const containerRef = React.useRef<HTMLDivElement>(null);
  const { scrollYProgress } = useScroll({
    target: containerRef,
    offset: ["start start", "end start"],
  });

  const opacityFade = useTransform(
    scrollYProgress,
    [0, 0.3, 0.7, 1],
    [0, 1, 1, 0],
  );
  const scaleValue = useTransform(
    scrollYProgress,
    [0, 0.3, 0.7, 1],
    [0.5, 1, 1, 0.5],
  );
  const blurValue = useTransform(
    scrollYProgress,
    [0, 0.3, 0.7, 1],
    [10, 0, 0, 10],
  );
  const filterBlur = useTransform(blurValue, (v) => `blur(${v}px)`);
  const color = useTransform(
    scrollYProgress,
    [0, 0.5, 1],
    [
      "hsl(var(--muted-foreground))",
      "hsl(var(--primary))",
      "hsl(var(--muted-foreground))",
    ],
  );

  const effectStyles = {
    fade: { opacity: opacityFade },
    scale: { scale: scaleValue, opacity: opacityFade },
    blur: { filter: filterBlur, opacity: opacityFade },
    color: { opacity: opacityFade, color },
  };

  return (
    <div ref={containerRef} className="relative" style={{ height }}>
      <motion.div
        ref={ref}
        className={cn(
          "sticky top-1/2 -translate-y-1/2 will-change-transform",
          className,
        )}
        style={effectStyles[effect]}
      >
        {children}
      </motion.div>
    </div>
  );
});

StickyScrollText.displayName = "StickyScrollText";

// ============================================================================
// EXPORTS
// ============================================================================

export {
  HorizontalScrollText,
  ParallaxText,
  ScrollFade,
  ScrollProgressText,
  ScrollReveal,
  ScrollScale,
  ScrollText,
  StickyScrollText,
};

export type {
  HorizontalScrollTextProps,
  ParallaxTextProps,
  ScrollEffect,
  ScrollFadeProps,
  ScrollProgressTextProps,
  ScrollRevealProps,
  ScrollScaleProps,
  ScrollTextProps,
  StickyScrollTextProps,
};
```
---

## Segmented Button <a name="segmented-button"></a>

iOS-style segmented control for React. Animated selection indicator with smooth spring transitions. Perfect for filters, tabs, and toggle groups.

- **Registry Key**: `segmented-button`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/segmented-button.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
"use client";

import SegmentedButton from "@/registry/default/ui/segmented-button";

export default function SegmentedButtonBasicDemo() {
  const navigationTabs = [
    { id: "home", label: "Home" },
    { id: "about", label: "About" },
    { id: "contact", label: "Contact" },
  ];

  return (
    <SegmentedButton
      buttons={navigationTabs}
      defaultActive="home"
      onChange={(activeId) => console.log("Active tab:", activeId)}
    />
  );
}
```
#### Component Source Code
##### File: `ui/segmented-button.tsx`
```tsx
"use client";

import { motion } from "motion/react";
import { useEffect, useRef, useState } from "react";

interface SegmentedButtonItem {
  id: string;
  label?: string | null;
  isLogo?: boolean;
}

interface SegmentedButtonProps {
  buttons: SegmentedButtonItem[];
  defaultActive?: string;
  onChange?: (activeId: string) => void;
  className?: string;
}

export default function SegmentedButton({
  buttons,
  defaultActive,
  onChange,
  className = "",
}: SegmentedButtonProps) {
  const [activeButton, setActiveButton] = useState(
    defaultActive || buttons[0]?.id || "",
  );
  const [indicatorStyle, setIndicatorStyle] = useState({ left: 0, width: 0 });
  const [hoveredIndex, setHoveredIndex] = useState<number | null>(null);
  const [hoverStyle, setHoverStyle] = useState({ left: 0, width: 0 });
  const buttonRefs = useRef<(HTMLButtonElement | null)[]>([]);
  const containerRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const activeIndex = buttons.findIndex((btn) => btn.id === activeButton);
    const activeElement = buttonRefs.current[activeIndex];

    if (activeElement) {
      setIndicatorStyle({
        left: activeElement.offsetLeft,
        width: activeElement.offsetWidth,
      });
    }
  }, [activeButton, buttons]);

  useEffect(() => {
    if (hoveredIndex !== null) {
      const hoveredElement = buttonRefs.current[hoveredIndex];
      if (hoveredElement) {
        setHoverStyle({
          left: hoveredElement.offsetLeft,
          width: hoveredElement.offsetWidth,
        });
      }
    } else {
      if (containerRef.current) {
        setHoverStyle({
          left: 0,
          width: containerRef.current.offsetWidth,
        });
      }
    }
  }, [hoveredIndex]);

  useEffect(() => {
    if (containerRef.current) {
      setHoverStyle({
        left: 0,
        width: containerRef.current.offsetWidth,
      });
    }
  }, []);

  const handleButtonClick = (buttonId: string) => {
    setActiveButton(buttonId);
    onChange?.(buttonId);
  };

  return (
    <div
      ref={containerRef}
      className={`relative inline-flex items-center justify-start rounded-full ${className}`}
      onMouseLeave={() => setHoveredIndex(null)}
      role="group"
    >
      <motion.div
        className="absolute top-0 h-7 rounded-full bg-black/10 dark:bg-white/15"
        animate={{
          left: hoverStyle.left,
          width: hoverStyle.width,
        }}
        transition={{
          type: "spring",
          stiffness: 400,
          damping: 30,
        }}
      />

      <motion.div
        className="absolute top-0 h-7 rounded-[999px] bg-gradient-to-b from-[#A8A8A8] to-[#D3D3D3] px-2.5 py-1 shadow-[inset_0_1px_0_0_rgba(0,0,0,0.15),inset_0_-1px_0_0_rgba(255,255,255,0.30),inset_0_0_0_1px_rgba(0,0,0,0.10),inset_0_-6px_10.5px_0_rgba(0,0,0,0.08)] dark:from-[#D3D3D3] dark:to-[#A8A8A8] dark:shadow-[inset_0_1px_0_0_rgba(255,255,255,0.30),inset_0_-1px_0_0_rgba(255,255,255,0.60),inset_0_0_0_1px_rgba(255,255,255,0.30),inset_0_-6px_10.5px_0_rgba(255,255,255,0.13)]"
        animate={{
          left: indicatorStyle.left,
          width: indicatorStyle.width,
        }}
        transition={{
          type: "spring",
          stiffness: 400,
          damping: 30,
        }}
      />

      {buttons.map((button, index) => (
        <button
          key={button.id}
          ref={(el) => {
            buttonRefs.current[index] = el;
          }}
          onClick={() => handleButtonClick(button.id)}
          onMouseEnter={() => setHoveredIndex(index)}
          className="relative z-10 flex items-center justify-center gap-2 rounded-full px-2 py-1 transition-colors"
        >
          {button.isLogo ? (
            <div className="flex h-5 items-center justify-center">
              <svg
                width="19"
                height="20"
                viewBox="0 0 19 20"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
                className={`transition-all ${
                  activeButton === button.id
                    ? "[filter:drop-shadow(0px_1px_0px_rgba(255,255,255,0.65))] [&_path]:fill-black dark:[&_path]:fill-black/80"
                    : "[&_path]:fill-neutral-50/80 dark:[&_path]:fill-neutral-50/80"
                }`}
              >
                <path
                  fillRule="evenodd"
                  clipRule="evenodd"
                  d="M11.2851 7.03125H15.7382C15.8084 7.03125 15.8774 7.03612 15.9449 7.04554L11.296 11.6945C11.2863 11.6258 11.2812 11.5557 11.2812 11.4844V7.03125H9.5V11.4844C9.5 13.2879 10.9621 14.75 12.7656 14.75H17.2188V12.9688H12.7656C12.6943 12.9688 12.6242 12.9638 12.5556 12.954L17.2073 8.30221C17.2173 8.3719 17.2225 8.44315 17.2225 8.51562V12.9688H19.0038V8.51562C19.0038 6.71207 17.5417 5.25 15.7382 5.25H11.2851V7.03125ZM0 6.4375V6.44231L6.08623 14.1927C6.81766 15.1242 8.31376 14.6069 8.31376 13.4226V6.4375H6.53251V11.8769L2.26105 6.4375H0Z"
                />
                <path
                  fillRule="evenodd"
                  clipRule="evenodd"
                  d="M11.2851 7.03125H15.7382C15.8084 7.03125 15.8774 7.03612 15.9449 7.04554L11.296 11.6945C11.2863 11.6258 11.2812 11.5557 11.2812 11.4844V7.03125H9.5V11.4844C9.5 13.2879 10.9621 14.75 12.7656 14.75H17.2188V12.9688H12.7656C12.6943 12.9688 12.6242 12.9638 12.5556 12.954L17.2073 8.30221C17.2173 8.3719 17.2225 8.44315 17.2225 8.51562V12.9688H19.0038V8.51562C19.0038 6.71207 17.5417 5.25 15.7382 5.25H11.2851V7.03125ZM0 6.4375V6.44231L6.08623 14.1927C6.81766 15.1242 8.31376 14.6069 8.31376 13.4226V6.4375H6.53251V11.8769L2.26105 6.4375H0Z"
                />
              </svg>
            </div>
          ) : (
            <span
              className={`text-center font-normal font-sans text-sm leading-tight transition-colors ${
                activeButton === button.id
                  ? "text-black [text-shadow:_0px_1px_0px_rgb(255_255_255_/_0.65)] dark:text-black/80"
                  : "text-black-50/80 dark:text-neutral-50/80"
              }`}
            >
              {button.label}
            </span>
          )}
        </button>
      ))}
    </div>
  );
}
```
---

## Shutter Text <a name="shutter-text"></a>

A cinematic shutter-style text reveal animation with layered color slices that sweep across each character. Supports auto, scroll, click, and hover triggers.

- **Registry Key**: `shutter-text`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/shutter-text.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
import ShutterText from "@/registry/default/ui/shutter-text";

export default function ShutterTextDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center">
      <ShutterText text="IMMERSE" className="text-7xl" />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/shutter-text.tsx`
```tsx
"use client";

import { AnimatePresence, motion, useInView } from "framer-motion";
import {
  type HTMLAttributes,
  useCallback,
  useEffect,
  useRef,
  useState,
} from "react";

interface ShutterTextProps extends HTMLAttributes<HTMLDivElement> {
  text?: string;
  trigger?: "auto" | "scroll" | "click" | "hover";
}

export default function ShutterText({
  text = "IMMERSE",
  trigger = "auto",
  className = "",
  ...props
}: ShutterTextProps) {
  const [count, setCount] = useState(0);
  const [active, setActive] = useState(
    trigger === "auto" || trigger === "click" || trigger === "hover",
  );
  const [animating, setAnimating] = useState(trigger === "auto");
  const ref = useRef<HTMLDivElement>(null);
  const isInView = useInView(ref, { once: false, amount: 0.5 });
  const characters = text.split("");

  // Handle scroll trigger
  useEffect(() => {
    if (trigger === "scroll" && isInView) {
      setActive(true);
      setAnimating(true);
      setCount((c) => c + 1);
    }
    if (trigger === "scroll" && !isInView) {
      setActive(false);
      setAnimating(false);
    }
  }, [trigger, isInView]);

  // Handle auto trigger – animate once on mount
  useEffect(() => {
    if (trigger === "auto") {
      setActive(true);
      setAnimating(true);
      setCount((c) => c + 1);
    }
  }, [trigger]);

  const handleClick = useCallback(() => {
    if (trigger === "click") {
      setAnimating(true);
      setCount((c) => c + 1);
    }
  }, [trigger]);

  const handleMouseEnter = useCallback(() => {
    if (trigger === "hover") {
      setAnimating(true);
      setCount((c) => c + 1);
    }
  }, [trigger]);

  const handleMouseLeave = useCallback(() => {
    if (trigger === "hover") {
      setAnimating(false);
    }
  }, [trigger]);

  return (
    <div
      ref={ref}
      role="button"
      tabIndex={0}
      onClick={handleClick}
      onMouseEnter={handleMouseEnter}
      onMouseLeave={handleMouseLeave}
      className={`relative inline-flex flex-wrap items-center justify-center ${className}`}
      {...props}
    >
      <AnimatePresence mode="wait">
        {animating ? (
          <motion.span
            key={count}
            className="flex flex-wrap items-center justify-center"
          >
            {characters.map((char, i) => (
              <span
                key={i}
                className="relative inline-block overflow-hidden px-[0.1vw]"
              >
                {/* Main Character */}
                <motion.span
                  initial={{ opacity: 0, filter: "blur(10px)" }}
                  animate={{ opacity: 1, filter: "blur(0px)" }}
                  transition={{ delay: i * 0.04 + 0.3, duration: 0.8 }}
                  className="inline-block font-black text-zinc-900 leading-none tracking-tighter dark:text-white"
                >
                  {char === " " ? "\u00A0" : char}
                </motion.span>

                {/* Top Slice Layer */}
                <motion.span
                  initial={{ x: "-100%", opacity: 0 }}
                  animate={{ x: "100%", opacity: [0, 1, 0] }}
                  transition={{
                    duration: 0.7,
                    delay: i * 0.04,
                    ease: "easeInOut",
                  }}
                  className="pointer-events-none absolute inset-0 z-10 inline-block font-black text-indigo-600 leading-none dark:text-emerald-400"
                  style={{ clipPath: "polygon(0 0, 100% 0, 100% 35%, 0 35%)" }}
                >
                  {char}
                </motion.span>

                {/* Middle Slice Layer */}
                <motion.span
                  initial={{ x: "100%", opacity: 0 }}
                  animate={{ x: "-100%", opacity: [0, 1, 0] }}
                  transition={{
                    duration: 0.7,
                    delay: i * 0.04 + 0.1,
                    ease: "easeInOut",
                  }}
                  className="pointer-events-none absolute inset-0 z-10 inline-block font-black text-zinc-800 leading-none dark:text-zinc-200"
                  style={{
                    clipPath: "polygon(0 35%, 100% 35%, 100% 65%, 0 65%)",
                  }}
                >
                  {char}
                </motion.span>

                {/* Bottom Slice Layer */}
                <motion.span
                  initial={{ x: "-100%", opacity: 0 }}
                  animate={{ x: "100%", opacity: [0, 1, 0] }}
                  transition={{
                    duration: 0.7,
                    delay: i * 0.04 + 0.2,
                    ease: "easeInOut",
                  }}
                  className="pointer-events-none absolute inset-0 z-10 inline-block font-black text-indigo-600 leading-none dark:text-emerald-400"
                  style={{
                    clipPath: "polygon(0 65%, 100% 65%, 100% 100%, 0 100%)",
                  }}
                >
                  {char}
                </motion.span>
              </span>
            ))}
          </motion.span>
        ) : (
          <span className="flex flex-wrap items-center justify-center">
            {characters.map((char, i) => (
              <span
                key={i}
                className="relative inline-block overflow-hidden px-[0.1vw]"
              >
                <span className="inline-block font-black text-zinc-900 leading-none tracking-tighter dark:text-white">
                  {char === " " ? "\u00A0" : char}
                </span>
              </span>
            ))}
          </span>
        )}
      </AnimatePresence>
    </div>
  );
}
```
---

## Spline Scene <a name="spline-scene"></a>

- **Registry Key**: `spline-scene`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/spline-scene.json
```

#### Dependencies
- **NPM Packages**:
  - `@splinetool/react-spline`

#### How to Use
```tsx
import { SplineScene } from "@/registry/default/ui/spline-scene";

export default function SplineSceneDemo() {
  return (
    <div className="w-full h-[400px] rounded-lg overflow-hidden border">
      <SplineScene
        scene="https://prod.spline.design/UbM7F-HZcyTbZ4y3/scene.splinecode"
        className="w-full h-full"
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/spline-scene.tsx`
```tsx
"use client"

import { Suspense, lazy } from "react"
const Spline = lazy(() => import("@splinetool/react-spline"))

interface SplineSceneProps {
  scene: string
  className?: string
}

export function SplineScene({ scene, className }: SplineSceneProps) {
  return (
    <Suspense
      fallback={
        <div className="w-full h-full flex items-center justify-center bg-black/5 rounded-lg">
          <div className="animate-spin rounded-full h-8 w-8 border-b-2 border-primary"></div>
        </div>
      }
    >
      <Spline scene={scene} className={className} />
    </Suspense>
  )
}
```
---

## Sticky Navbar <a name="sticky-navbar"></a>

- **Registry Key**: `sticky-navbar`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/sticky-navbar.json
```

#### Dependencies
- **NPM Packages**:
  - `lucide-react`
  - `motion`

#### How to Use
```tsx
import { Button } from "@/components/ui/button"
import {
    MobileNav,
    MobileNavHeader,
    MobileNavMenu,
    MobileNavToggle,
    NavActions,
    Navbar,
    NavBody,
    NavItems,
    NavLogo,
} from "@/registry/default/ui/sticky-navbar"
import { useState } from "react"

export default function StickyNavbarDemo() {
  const [mobileMenuOpen, setMobileMenuOpen] = useState(false)

  const navItems = [
    { name: "Features", link: "#features" },
    { name: "Pricing", link: "#pricing" },
    { name: "Docs", link: "#docs" },
    { name: "Blog", link: "#blog" },
  ]

  return (
    <div className="relative h-[600px] w-full overflow-auto">
      <Navbar>
        {/* Desktop Nav */}
        <NavBody>
          <NavLogo href="#">
            <div className="flex h-8 w-8 items-center justify-center rounded-lg bg-zinc-950 dark:bg-white">
              <span className="text-sm font-bold text-white dark:text-zinc-950">
                A
              </span>
            </div>
            <span className="hidden font-semibold sm:block">Apex</span>
          </NavLogo>

          <NavItems items={navItems} />

          <NavActions>
            <Button variant="ghost" size="sm">
              Sign In
            </Button>
            <Button size="sm" className="rounded-full">
              Get Started
            </Button>
          </NavActions>
        </NavBody>

        {/* Mobile Nav */}
        <MobileNav>
          <MobileNavHeader>
            <NavLogo href="#">
              <div className="flex h-8 w-8 items-center justify-center rounded-lg bg-zinc-950 dark:bg-white">
                <span className="text-sm font-bold text-white dark:text-zinc-950">
                  A
                </span>
              </div>
              <span className="font-semibold">Apex</span>
            </NavLogo>
            <MobileNavToggle
              isOpen={mobileMenuOpen}
              onClick={() => setMobileMenuOpen(!mobileMenuOpen)}
            />
          </MobileNavHeader>

          <MobileNavMenu
            isOpen={mobileMenuOpen}
            onClose={() => setMobileMenuOpen(false)}
          >
            {navItems.map((item) => (
              <a
                key={item.name}
                href={item.link}
                className="rounded-lg px-4 py-3 text-sm transition-colors hover:bg-zinc-100 dark:hover:bg-zinc-800"
                onClick={() => setMobileMenuOpen(false)}
              >
                {item.name}
              </a>
            ))}
            <hr className="my-2 border-zinc-200 dark:border-zinc-800" />
            <Button
              variant="ghost"
              className="w-full justify-start"
              onClick={() => setMobileMenuOpen(false)}
            >
              Sign In
            </Button>
            <Button
              className="w-full rounded-full"
              onClick={() => setMobileMenuOpen(false)}
            >
              Get Started
            </Button>
          </MobileNavMenu>
        </MobileNav>
      </Navbar>

      {/* Demo Content */}
      <div className="space-y-4 px-4 pt-32">
        <h1 className="text-4xl font-bold">Scroll to see the navbar effect</h1>
        {Array.from({ length: 20 }).map((_, i) => (
          <p key={i} className="text-muted-foreground">
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do
            eiusmod tempor incididunt ut labore et dolore magna aliqua.
          </p>
        ))}
      </div>
    </div>
  )
}
```
#### Component Source Code
##### File: `ui/sticky-navbar.tsx`
```tsx
"use client"

import { cn } from "@/lib/utils"
import { AnimatePresence, motion, useMotionValueEvent, useScroll } from "framer-motion"
import { Menu, X } from "lucide-react"
import React, { useRef, useState } from "react"

interface NavbarProps {
  children: React.ReactNode
  className?: string
  position?: "fixed" | "sticky"
}

interface NavBodyProps {
  children: React.ReactNode
  className?: string
  visible?: boolean
}

interface NavItemsProps {
  items: {
    name: string
    link: string
    icon?: React.ReactNode
  }[]
  className?: string
  onItemClick?: () => void
}

interface MobileNavProps {
  children: React.ReactNode
  className?: string
  visible?: boolean
}

interface MobileNavHeaderProps {
  children: React.ReactNode
  className?: string
}

interface MobileNavMenuProps {
  children: React.ReactNode
  className?: string
  isOpen: boolean
  onClose: () => void
}

interface MobileNavToggleProps {
  isOpen: boolean
  onClick: () => void
  className?: string
}

interface NavLogoProps {
  href?: string
  children: React.ReactNode
  className?: string
}

interface NavActionsProps {
  children: React.ReactNode
  className?: string
}

export const Navbar = ({ children, className, position = "fixed" }: NavbarProps) => {
  const ref = useRef<HTMLDivElement>(null)
  const { scrollY } = useScroll({
    target: ref,
    offset: ["start start", "end start"],
  })
  const [visible, setVisible] = useState<boolean>(false)

  useMotionValueEvent(scrollY, "change", (latest) => {
    if (latest > 100) {
      setVisible(true)
    } else {
      setVisible(false)
    }
  })

  return (
    <motion.div
      ref={ref}
      className={cn(
        position === "fixed" ? "fixed" : "sticky",
        "inset-x-0 top-4 z-50 mx-auto w-[calc(100%-2rem)] max-w-7xl",
        className
      )}
    >
      {React.Children.map(children, (child) =>
        React.isValidElement(child)
          ? React.cloneElement(child as React.ReactElement<{ visible?: boolean }>, { visible })
          : child
      )}
    </motion.div>
  )
}

export const NavBody = ({ children, className, visible }: NavBodyProps) => {
  return (
    <motion.nav
      initial={{ y: -100, opacity: 0 }}
      animate={{
        y: 0,
        opacity: 1,
        backdropFilter: visible ? "blur(16px)" : "blur(16px)",
      }}
      transition={{
        duration: 0.6,
        ease: [0.22, 1, 0.36, 1],
      }}
      className={cn(
        "relative z-[60] mx-auto hidden w-full flex-row items-center justify-between rounded-full bg-white/80 px-4 py-3 backdrop-blur-md md:flex dark:bg-zinc-900/80",
        "border border-zinc-200 dark:border-zinc-800",
        className
      )}
    >
      {children}
    </motion.nav>
  )
}

export const NavItems = ({ items, className, onItemClick }: NavItemsProps) => {
  const [hovered, setHovered] = useState<number | null>(null)

  return (
    <motion.div
      onMouseLeave={() => setHovered(null)}
      className={cn("flex flex-1 flex-row items-center justify-center gap-1", className)}
    >
      {items.map((item, idx) => (
        <a
          key={`link-${idx}`}
          href={item.link}
          onMouseEnter={() => setHovered(idx)}
          onClick={onItemClick}
          className="relative px-4 py-2 text-sm text-zinc-600 transition-colors hover:text-zinc-900 dark:text-zinc-400 dark:hover:text-white"
        >
          {hovered === idx && (
            <motion.div
              layoutId="navbar-hover"
              className="absolute inset-0 rounded-full bg-zinc-100 dark:bg-zinc-800"
              initial={false}
              transition={{ type: "spring", stiffness: 500, damping: 30 }}
            />
          )}
          <span className="relative z-10 flex items-center gap-2">
            {item.icon}
            {item.name}
          </span>
        </a>
      ))}
    </motion.div>
  )
}

export const NavLogo = ({ href = "#", children, className }: NavLogoProps) => {
  return (
    <a href={href} className={cn("relative z-20 flex items-center gap-2", className)}>
      {children}
    </a>
  )
}

export const NavActions = ({ children, className }: NavActionsProps) => {
  return <div className={cn("hidden md:flex items-center gap-3", className)}>{children}</div>
}

export const MobileNav = ({ children, className, visible }: MobileNavProps) => {
  return (
    <motion.nav
      initial={{ y: -100, opacity: 0 }}
      animate={{
        y: 0,
        opacity: 1,
        backdropFilter: visible ? "blur(16px)" : "blur(16px)",
      }}
      transition={{
        duration: 0.6,
        ease: [0.22, 1, 0.36, 1],
      }}
      className={cn(
        "relative z-50 mx-auto flex w-full flex-col items-center justify-between rounded-full bg-white/80 px-4 py-3 backdrop-blur-md md:hidden dark:bg-zinc-900/80",
        "border border-zinc-200 dark:border-zinc-800",
        className
      )}
    >
      {children}
    </motion.nav>
  )
}

export const MobileNavHeader = ({ children, className }: MobileNavHeaderProps) => {
  return <div className={cn("flex w-full flex-row items-center justify-between", className)}>{children}</div>
}

export const MobileNavMenu = ({ children, className, isOpen, onClose }: MobileNavMenuProps) => {
  return (
    <AnimatePresence>
      {isOpen && (
        <motion.div
          initial={{ opacity: 0, y: -10 }}
          animate={{ opacity: 1, y: 0 }}
          exit={{ opacity: 0, y: -10 }}
          transition={{ duration: 0.2 }}
          className={cn(
            "absolute left-0 right-0 top-full z-50 mt-2 flex w-full flex-col gap-2 rounded-2xl bg-white/95 p-4 backdrop-blur-md dark:bg-zinc-900/95",
            "border border-zinc-200 dark:border-zinc-800",
            "shadow-lg",
            className
          )}
        >
          {children}
        </motion.div>
      )}
    </AnimatePresence>
  )
}

export const MobileNavToggle = ({ isOpen, onClick, className }: MobileNavToggleProps) => {
  return (
    <button
      onClick={onClick}
      aria-label="Toggle menu"
      className={cn("p-2 text-zinc-600 hover:text-zinc-900 dark:text-zinc-400 dark:hover:text-white", className)}
    >
      {isOpen ? <X size={20} /> : <Menu size={20} />}
    </button>
  )
}
```
---

## Style <a name="style"></a>

- **Registry Key**: `style`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/style.json
```

#### Dependencies
- **NPM Packages**:
  - `tailwindcss-animate`
  - `class-variance-authority`
  - `lucide-react`

---

## Text Morphing <a name="text-morphing"></a>

Smooth text morphing animation for React. Words transition with blur and scale effects. Perfect for dynamic hero sections and attention-grabbing headlines.

- **Registry Key**: `text-morphing`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/text-morphing.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
"use client";

import { MorphingText } from "@/registry/default/ui/text-morphing";

export default function TextMorphingDemo() {
  return (
    <div className="flex items-center justify-center">
      <div className="text-center">
        <h1 className="mb-4 font-bold text-4xl">
          Build{" "}
          <MorphingText
            words={["beautiful", "amazing", "stunning", "incredible"]}
            className="text-blue-600"
          />{" "}
          interfaces
        </h1>
        <p className="text-lg text-muted-foreground">
          Create{" "}
          <MorphingText
            words={["responsive", "accessible", "performant", "modern"]}
            interval={2000}
            className="text-green-600"
          />{" "}
          web experiences
        </p>
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/text-morphing.tsx`
```tsx
import { AnimatePresence, motion } from "motion/react";
import * as React from "react";
import { cn } from "@/lib/utils";

interface MorphingTextProps {
  words: string[];
  className?: string;
  interval?: number;
  animationDuration?: number;
}

const MorphingText = React.forwardRef<HTMLSpanElement, MorphingTextProps>(
  ({ words, className, interval = 3000, animationDuration = 0.5 }, ref) => {
    const [currentIndex, setCurrentIndex] = React.useState(0);

    React.useEffect(() => {
      const timer = setInterval(() => {
        setCurrentIndex((prev) => (prev + 1) % words.length);
      }, interval);
      return () => clearInterval(timer);
    }, [words.length, interval]);

    return (
      <span ref={ref} className={cn("relative inline-block", className)}>
        <AnimatePresence mode="wait">
          <motion.span
            key={currentIndex}
            initial={{ opacity: 0, y: 20, filter: "blur(8px)" }}
            animate={{ opacity: 1, y: 0, filter: "blur(0px)" }}
            exit={{ opacity: 0, y: -20, filter: "blur(8px)" }}
            transition={{ duration: animationDuration, ease: "easeInOut" }}
            className="inline-block"
          >
            {words[currentIndex]}
          </motion.span>
        </AnimatePresence>
      </span>
    );
  },
);
MorphingText.displayName = "MorphingText";
export { MorphingText };
```
---

## Theme Daylight <a name="theme-daylight"></a>

- **Registry Key**: `theme-daylight`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/theme-daylight.json
```

---

## Theme Emerald <a name="theme-emerald"></a>

- **Registry Key**: `theme-emerald`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/theme-emerald.json
```

---

## Theme Midnight <a name="theme-midnight"></a>

- **Registry Key**: `theme-midnight`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/theme-midnight.json
```

---

## Typewriter Text <a name="typewritter-text"></a>

React typewriter effect with customizable speed, cursor styles, and word deletion. Perfect for hero sections and dynamic text displays.

- **Registry Key**: `typewritter-text`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/typewritter-text.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
"use client";

import { TypewriterText } from "@/registry/default/ui/typewritter-text";

export default function TypewritterTextDemo() {
  return (
    <div className="flex items-center justify-center">
      <TypewriterText
        words={["Hello", "World", "Typewriter", "Effect"]}
        className="font-bold text-4xl text-foreground"
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/typewritter-text.tsx`
```tsx
import { motion } from "motion/react";
import * as React from "react";
import { cn } from "@/lib/utils";

interface TypewriterTextProps {
  words: string[];
  className?: string;
  typingSpeed?: number;
  deletingSpeed?: number;
  pauseDuration?: number;
  cursorClassName?: string;
}

const TypewriterText = React.forwardRef<HTMLSpanElement, TypewriterTextProps>(
  (
    {
      words,
      className,
      typingSpeed = 100,
      deletingSpeed = 50,
      pauseDuration = 1500,
      cursorClassName,
    },
    ref,
  ) => {
    const [currentWordIndex, setCurrentWordIndex] = React.useState(0);
    const [currentText, setCurrentText] = React.useState("");
    const [isDeleting, setIsDeleting] = React.useState(false);

    React.useEffect(() => {
      const currentWord = words[currentWordIndex] || "";

      const timeout = setTimeout(
        () => {
          if (!isDeleting) {
            if (currentText.length < currentWord.length) {
              setCurrentText(currentWord.slice(0, currentText.length + 1));
            } else {
              setTimeout(() => setIsDeleting(true), pauseDuration);
            }
          } else {
            if (currentText.length > 0) {
              setCurrentText(currentText.slice(0, -1));
            } else {
              setIsDeleting(false);
              setCurrentWordIndex((prev) => (prev + 1) % words.length);
            }
          }
        },
        isDeleting ? deletingSpeed : typingSpeed,
      );

      return () => clearTimeout(timeout);
    }, [
      currentText,
      isDeleting,
      currentWordIndex,
      words,
      typingSpeed,
      deletingSpeed,
      pauseDuration,
    ]);

    return (
      <span ref={ref} className={cn("inline-block", className)}>
        {currentText}
        <motion.span
          animate={{ opacity: [1, 0] }}
          transition={{
            duration: 0.5,
            repeat: Infinity,
            repeatType: "reverse",
          }}
          className={cn(
            "ml-0.5 inline-block h-[1em] w-[2px] bg-current align-middle",
            cursorClassName,
          )}
        />
      </span>
    );
  },
);
TypewriterText.displayName = "TypewriterText";

export { TypewriterText };
```
---

## Use Mobile <a name="use-mobile"></a>

- **Registry Key**: `use-mobile`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/use-mobile.json
```

#### Component Source Code
##### File: `hooks/use-mobile.ts`
```ts
import * as React from "react";

const MOBILE_BREAKPOINT = 768;

export function useIsMobile({
  breakpoint = MOBILE_BREAKPOINT,
}: {
  breakpoint?: number;
}) {
  const [isMobile, setIsMobile] = React.useState<boolean | undefined>(
    undefined,
  );

  React.useEffect(() => {
    const mql = window.matchMedia(`(max-width: ${breakpoint - 1}px)`);
    const onChange = () => {
      setIsMobile(window.innerWidth < breakpoint);
    };
    mql.addEventListener("change", onChange);
    setIsMobile(window.innerWidth < breakpoint);
    return () => mql.removeEventListener("change", onChange);
  }, [breakpoint]);

  return !!isMobile;
}
```
---

## Utils <a name="utils"></a>

- **Registry Key**: `utils`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/utils.json
```

#### Dependencies
- **NPM Packages**:
  - `clsx`
  - `tailwind-merge`

#### Component Source Code
##### File: `lib/utils.ts`
```ts
import { type ClassValue, clsx } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```
---

## Velocity Morph <a name="velocity-morph"></a>

High-speed text morph animation for React. Blur, scale, and slide effects with velocity-based easing. Perfect for dynamic and energetic UI.

- **Registry Key**: `velocity-morph`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/velocity-morph.json
```

#### Dependencies
- **NPM Packages**:
  - `motion`

#### How to Use
```tsx
"use client";

import { VelocityMorph } from "@/registry/default/ui/velocity-morph";

export default function VelocityMorphDemo() {
  return (
    <div className="flex items-center justify-center">
      <VelocityMorph
        texts={["Hello", "World", "Velocity", "Morph"]}
        className="font-bold text-4xl text-foreground"
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/velocity-morph.tsx`
```tsx
import { AnimatePresence, motion } from "motion/react";
import * as React from "react";
import { cn } from "@/lib/utils";

interface VelocityMorphProps {
  texts: string[];
  className?: string;
  interval?: number;
}

const VelocityMorph = React.forwardRef<HTMLDivElement, VelocityMorphProps>(
  ({ texts, className, interval = 3000 }, ref) => {
    const [currentIndex, setCurrentIndex] = React.useState(0);

    React.useEffect(() => {
      const timer = setInterval(() => {
        setCurrentIndex((prev) => (prev + 1) % texts.length);
      }, interval);

      return () => clearInterval(timer);
    }, [interval, texts.length]);

    return (
      <div
        ref={ref}
        className={cn(
          "relative overflow-hidden whitespace-nowrap p-2",
          className,
        )}
      >
        <AnimatePresence mode="popLayout">
          <motion.div
            key={currentIndex}
            initial={{
              y: 40,
              opacity: 0,
              filter: "blur(10px)",
              scale: 0.8,
            }}
            animate={{
              y: 0,
              opacity: 1,
              filter: "blur(0px)",
              scale: 1,
            }}
            exit={{
              y: -40,
              opacity: 0,
              filter: "blur(10px)",
              scale: 0.8,
            }}
            transition={{
              duration: 0.4,
              ease: [0.16, 1, 0.3, 1],
            }}
          >
            {texts[currentIndex]}
          </motion.div>
        </AnimatePresence>
      </div>
    );
  },
);

VelocityMorph.displayName = "VelocityMorph";

export { VelocityMorph };
```
---

## Vercel Tabs <a name="vercel-tabs"></a>

Animated tabs component inspired by Vercel's dashboard. Smooth active indicator animation with hover effects. Perfect for navigation and settings UI.

- **Registry Key**: `vercel-tabs`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/vercel-tabs.json
```

#### How to Use
```tsx
import { VercelTabs } from "@/registry/default/ui/vercel-tabs";

const tabsData = [
  {
    label: "Overview",
    value: "Overview",
    content: (
      <div className="rounded-lg border border-gray-200 bg-white p-6 text-black dark:border-[#333] dark:bg-[#1c1c1c] dark:text-white">
        <h2 className="mb-2 font-bold text-2xl">Overview Content</h2>
        <p className="text-muted-foreground">
          This is the content area for the Overview tab. You can pass any React
          components or content here.
        </p>
      </div>
    ),
  },
  {
    label: "Integrations",
    value: "Integrations",
    content: (
      <div className="rounded-lg border border-gray-200 bg-white p-6 text-black dark:border-[#333] dark:bg-[#1c1c1c] dark:text-white">
        <h2 className="mb-2 font-bold text-2xl">Integrations</h2>
        <p className="text-muted-foreground">
          Connect your favorite tools and services.
        </p>
      </div>
    ),
  },
  {
    label: "Activity",
    value: "Activity",
    content: (
      <div className="rounded-lg border border-gray-200 bg-white p-6 text-black dark:border-[#333] dark:bg-[#1c1c1c] dark:text-white">
        <h2 className="mb-2 font-bold text-2xl">Activity Log</h2>
        <p className="text-muted-foreground">
          View the latest activity on your account.
        </p>
      </div>
    ),
  },
  {
    label: "Domains",
    value: "Domains",
    content: (
      <div className="rounded-lg border border-gray-200 bg-white p-6 text-black dark:border-[#333] dark:bg-[#1c1c1c] dark:text-white">
        <h2 className="mb-2 font-bold text-2xl">Domains</h2>
        <p className="text-muted-foreground">
          Manage your custom domains and DNS settings.
        </p>
      </div>
    ),
  },
  {
    label: "Usage",
    value: "Usage",
    content: (
      <div className="rounded-lg border border-gray-200 bg-white p-6 text-black dark:border-[#333] dark:bg-[#1c1c1c] dark:text-white">
        <h2 className="mb-2 font-bold text-2xl">Usage Statistics</h2>
        <p className="text-muted-foreground">
          Check your resource usage and limits.
        </p>
      </div>
    ),
  },
  {
    label: "Monitoring",
    value: "Monitoring",
    content: (
      <div className="rounded-lg border border-gray-200 bg-white p-6 text-black dark:border-[#333] dark:bg-[#1c1c1c] dark:text-white">
        <h2 className="mb-2 font-bold text-2xl">Monitoring</h2>
        <p className="text-muted-foreground">
          Real-time performance monitoring.
        </p>
      </div>
    ),
  },
];

export default function VercelTabsDemo() {
  return (
    <div className="flex flex-col gap-8">
      <div className="flex flex-col gap-3">
        <h3 className="font-medium text-sm">Vercel Tabs</h3>
        <VercelTabs tabs={tabsData} defaultTab="Overview" />
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/vercel-tabs.tsx`
```tsx
"use client";

import type React from "react";
import { useEffect, useRef, useState } from "react";
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs";

interface TabData {
  label: string;
  value: string;
  content: React.ReactNode;
}

interface VercelTabsProps {
  tabs: TabData[];
  defaultTab?: string;
  className?: string;
}

export function VercelTabs({ tabs, defaultTab, className }: VercelTabsProps) {
  const [hoveredIndex, setHoveredIndex] = useState<number | null>(null);
  const [activeTab, setActiveTab] = useState(defaultTab || tabs[0]?.value);
  const [hoverStyle, setHoverStyle] = useState({});
  const [activeStyle, setActiveStyle] = useState({ left: "0px", width: "0px" });
  const tabRefs = useRef<(HTMLButtonElement | null)[]>([]);

  const activeIndex = tabs.findIndex((tab) => tab.value === activeTab);

  useEffect(() => {
    if (hoveredIndex !== null) {
      const hoveredElement = tabRefs.current[hoveredIndex];
      if (hoveredElement) {
        const { offsetLeft, offsetWidth } = hoveredElement;
        setHoverStyle({
          left: `${offsetLeft}px`,
          width: `${offsetWidth}px`,
        });
      }
    }
  }, [hoveredIndex]);

  useEffect(() => {
    const activeElement = tabRefs.current[activeIndex];
    if (activeElement) {
      const { offsetLeft, offsetWidth } = activeElement;
      setActiveStyle({
        left: `${offsetLeft}px`,
        width: `${offsetWidth}px`,
      });
    }
  }, [activeIndex]);

  useEffect(() => {
    requestAnimationFrame(() => {
      const activeElement = tabRefs.current[activeIndex];
      if (activeElement) {
        const { offsetLeft, offsetWidth } = activeElement;
        setActiveStyle({
          left: `${offsetLeft}px`,
          width: `${offsetWidth}px`,
        });
      }
    });
  }, [activeIndex]);

  return (
    <Tabs
      defaultValue={activeTab}
      onValueChange={setActiveTab}
      className={`flex w-full flex-col items-center ${className}`}
    >
      <TabsList className="relative h-auto select-none gap-[6px] bg-transparent p-0">
        {/* Hover Highlight */}
        <div
          className="absolute top-0 left-0 flex h-[30px] items-center rounded-[6px] bg-[#0e0f1114] transition-all duration-300 ease-out dark:bg-[#ffffff1a]"
          style={{
            ...hoverStyle,
            opacity: hoveredIndex !== null ? 1 : 0,
          }}
        />

        {/* Active Indicator */}
        <div
          className="absolute bottom-[-6px] h-[2px] bg-[#0e0f11] transition-all duration-300 ease-out dark:bg-white"
          style={activeStyle}
        />

        {tabs.map((tab, index) => (
          <TabsTrigger
            key={tab.value}
            value={tab.value}
            ref={(el) => {
              tabRefs.current[index] = el;
            }}
            className={`z-10 h-[30px] cursor-pointer rounded-md border-0 bg-transparent px-3 py-2 outline-none transition-colors duration-300 focus-visible:outline-none focus-visible:ring-0 focus-visible:ring-offset-0 data-[state=active]:bg-transparent data-[state=active]:shadow-none ${
              activeTab === tab.value
                ? "text-[#0e0e10] dark:text-white"
                : "text-[#0e0f1199] dark:text-[#ffffff99]"
            }`}
            onMouseEnter={() => setHoveredIndex(index)}
            onMouseLeave={() => setHoveredIndex(null)}
          >
            <span className="whitespace-nowrap font-medium text-sm leading-5">
              {tab.label}
            </span>
          </TabsTrigger>
        ))}
      </TabsList>

      {/* Content Area */}
      <div className="mt-8 w-full px-4">
        {tabs.map((tab) => (
          <TabsContent
            key={tab.value}
            value={tab.value}
            className="fade-in-50 w-full animate-in duration-500"
          >
            {tab.content}
          </TabsContent>
        ))}
      </div>
    </Tabs>
  );
}
```
---

## Video Player <a name="video-player"></a>

Custom React video player with playback controls, volume slider, fullscreen mode, keyboard shortcuts, and progress bar. Fully accessible and styleable.

- **Registry Key**: `video-player`
#### Installation
```bash
npx shadcn@latest add https://www.jolyui.dev/r/video-player.json
```

#### Dependencies
- **NPM Packages**:
  - `lucide-react`
  - `motion`

#### How to Use
```tsx
import { VideoPlayer } from "@/registry/default/ui/video-player";

export default function VideoPlayerDemo() {
  return (
    <div className="mx-auto w-full max-w-4xl">
      <VideoPlayer
        src="https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4"
        poster="https://peach.blender.org/wp-content/uploads/title_anouncement.jpg?x11217"
        title="Big Buck Bunny"
        description="A large and lovable rabbit deals with three tiny bullies, led by a flying squirrel, who are determined to squelch his happiness."
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `ui/video-player.tsx`
```tsx
"use client";

import {
  ChevronLeft,
  ChevronRight,
  ChevronsLeft,
  ChevronsRight,
  Loader2,
  Maximize,
  Maximize2,
  MessageCircle,
  Minimize,
  MoreVertical,
  Pause,
  PictureInPicture2,
  Play,
  Repeat,
  RotateCcw,
  RotateCw,
  Settings,
  Volume2,
  VolumeX,
} from "lucide-react";
import { AnimatePresence, motion } from "motion/react";
import type React from "react";
import {
  forwardRef,
  useEffect,
  useImperativeHandle,
  useRef,
  useState,
} from "react";

interface QualitySource {
  quality: string;
  src: string;
}

interface CaptionTrack {
  src: string;
  label: string;
  srcLang: string;
  default?: boolean;
}

interface Chapter {
  title: string;
  startTime: number;
  endTime: number;
}

export interface VideoPlayerProps {
  src: string | QualitySource[];
  tracks?: CaptionTrack[];
  poster?: string;
  title?: string;
  description?: string;
  compact?: boolean;
  chapters?: Chapter[];
  onTimeUpdate?: (time: number) => void;
  onNextVideo?: () => void;
  onPrevVideo?: () => void;
  currentVideoIndex?: number;
  totalVideos?: number;
}

export interface VideoPlayerRef {
  seek: (time: number) => void;
  play: () => void;
  pause: () => void;
}

const Tooltip = ({
  children,
  label,
}: {
  children: React.ReactNode;
  label: string;
}) => {
  const [isHovered, setIsHovered] = useState(false);

  return (
    <div
      className="relative inline-flex"
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
      role="button"
      tabIndex={0}
    >
      {children}
      <motion.div
        initial={{ opacity: 0 }}
        animate={{ opacity: isHovered ? 1 : 0 }}
        transition={{ duration: 0.2 }}
        className="pointer-events-none absolute bottom-full left-1/2 z-50 mb-2 -translate-x-1/2 whitespace-nowrap rounded bg-black/90 px-2 py-1 text-white text-xs"
      >
        {label}
      </motion.div>
    </div>
  );
};

export const VideoPlayer = forwardRef<VideoPlayerRef, VideoPlayerProps>(
  (
    {
      src,
      tracks = [],
      poster,
      title: _title,
      description: _description,
      compact: _compact = false,
      chapters = [],
      onTimeUpdate,
      onNextVideo,
      onPrevVideo,
      currentVideoIndex = 0,
      totalVideos = 1,
    },
    ref,
  ) => {
    const videoRef = useRef<HTMLVideoElement>(null);
    const containerRef = useRef<HTMLDivElement>(null);

    const [isPlaying, setIsPlaying] = useState(false);
    const [isMuted, setIsMuted] = useState(false);
    const [isFullscreen, setIsFullscreen] = useState(false);
    const [isLoading, setIsLoading] = useState(false);
    const [showControls, setShowControls] = useState(true);
    const [activeDialog, setActiveDialog] = useState<
      "settings" | "options" | "captions" | null
    >(null);
    const [volume, setVolume] = useState(1);
    const [showVolumeSlider, setShowVolumeSlider] = useState(false);
    const [quality, setQuality] = useState("auto");
    const [availableQualities, setAvailableQualities] = useState<string[]>([]);
    const [currentSrc, setCurrentSrc] = useState("");
    const [speed, setSpeed] = useState(1);
    const [isPictureInPicture, setIsPictureInPicture] = useState(false);
    const [currentCaption, setCurrentCaption] = useState<string | null>(null);
    const [isTheaterMode, setIsTheaterMode] = useState(false);
    const [isLooping, setIsLooping] = useState(false);

    const [currentTime, setCurrentTime] = useState(0);
    const [duration, setDuration] = useState(0);
    const [buffered, setBuffered] = useState(0);

    // Hover state for timeline
    const [hoverTime, setHoverTime] = useState<number | null>(null);
    const [hoverPosition, setHoverPosition] = useState<number | null>(null);

    // Double tap state
    const [doubleTapAction, setDoubleTapAction] = useState<{
      side: "left" | "right";
      id: number;
    } | null>(null);
    const lastTapRef = useRef<{ time: number; x: number } | null>(null);
    const tapTimeoutRef = useRef<NodeJS.Timeout>(undefined);

    const controlsTimeoutRef = useRef<NodeJS.Timeout>(undefined);

    const handleTap = (e: React.MouseEvent<HTMLDivElement>) => {
      const time = Date.now();
      const clientX = e.clientX;
      const rect = containerRef.current?.getBoundingClientRect();
      if (!rect) return;

      const x = clientX - rect.left;
      const width = rect.width;
      const isLeft = x < width * 0.3;
      const isRight = x > width * 0.7;

      if (!isLeft && !isRight) {
        togglePlay();
        return;
      }

      if (lastTapRef.current && time - lastTapRef.current.time < 300) {
        // Double tap detected
        if (tapTimeoutRef.current) {
          clearTimeout(tapTimeoutRef.current);
        }

        if (isLeft) {
          handleSkip(-10);
          setDoubleTapAction({ side: "left", id: time });
        } else {
          handleSkip(10);
          setDoubleTapAction({ side: "right", id: time });
        }
        lastTapRef.current = null;
      } else {
        // First tap
        lastTapRef.current = { time, x };
        tapTimeoutRef.current = setTimeout(() => {
          togglePlay();
          lastTapRef.current = null;
        }, 300);
      }
    };

    // Clear double tap action after animation
    useEffect(() => {
      if (doubleTapAction) {
        const timeout = setTimeout(() => {
          setDoubleTapAction(null);
        }, 1000);
        return () => clearTimeout(timeout);
      }
    }, [doubleTapAction]);

    useImperativeHandle(ref, () => ({
      seek: (time: number) => {
        if (videoRef.current) {
          videoRef.current.currentTime = time;
          setCurrentTime(time);
        }
      },
      play: () => {
        videoRef.current?.play();
      },
      pause: () => {
        videoRef.current?.pause();
      },
    }));

    // Initialize quality sources
    useEffect(() => {
      if (Array.isArray(src)) {
        const qualities = src.map((s) => s.quality);
        setAvailableQualities(["auto", ...qualities]);
        // Default to the first quality source if auto
        setCurrentSrc(src[0]?.src || "");
      } else {
        setAvailableQualities(["auto"]);
        setCurrentSrc(src);
      }
    }, [src]);

    // Initialize captions
    useEffect(() => {
      if (tracks.length > 0) {
        const defaultTrack = tracks.find((t) => t.default);
        if (defaultTrack) {
          setCurrentCaption(defaultTrack.srcLang);
        }
      }
    }, [tracks]);

    // Handle caption change
    const handleCaptionChange = (lang: string | null) => {
      setCurrentCaption(lang);
      if (videoRef.current) {
        const textTracks = videoRef.current.textTracks;
        for (let i = 0; i < textTracks.length; i++) {
          const track = textTracks[i];
          if (track) {
            if (lang && track.language === lang) {
              track.mode = "showing";
            } else {
              track.mode = "hidden";
            }
          }
        }
      }
    };

    // Format time display
    const formatTime = (time: number) => {
      if (!time || Number.isNaN(time)) return "0:00";
      const hours = Math.floor(time / 3600);
      const minutes = Math.floor((time % 3600) / 60);
      const seconds = Math.floor(time % 60);

      if (hours > 0) {
        return `${hours}:${minutes.toString().padStart(2, "0")}:${seconds.toString().padStart(2, "0")}`;
      }
      return `${minutes}:${seconds.toString().padStart(2, "0")}`;
    };

    // Handle play/pause
    const togglePlay = () => {
      if (videoRef.current) {
        if (isPlaying) {
          videoRef.current.pause();
        } else {
          videoRef.current.play();
        }
        setIsPlaying(!isPlaying);
      }
    };

    // Handle volume change
    const handleVolumeChange = (value: number[]) => {
      const newVolume = value[0] ?? 1;
      setVolume(newVolume);
      if (videoRef.current) {
        videoRef.current.volume = newVolume;
      }
      if (newVolume === 0) {
        setIsMuted(true);
      } else if (isMuted) {
        setIsMuted(false);
      }
    };

    // Toggle mute
    const toggleMute = () => {
      if (videoRef.current) {
        if (isMuted) {
          videoRef.current.volume = volume || 0.5;
          setIsMuted(false);
        } else {
          videoRef.current.volume = 0;
          setIsMuted(true);
        }
      }
    };

    // Handle progress bar click
    const handleProgressClick = (e: React.MouseEvent<HTMLDivElement>) => {
      if (!videoRef.current || !duration) return;
      const rect = e.currentTarget.getBoundingClientRect();
      const percent = (e.clientX - rect.left) / rect.width;
      const newTime = percent * duration;
      videoRef.current.currentTime = newTime;
      setCurrentTime(newTime);
    };

    // Handle progress bar hover
    const handleProgressHover = (e: React.MouseEvent<HTMLDivElement>) => {
      if (!duration) return;
      const rect = e.currentTarget.getBoundingClientRect();
      const percent = (e.clientX - rect.left) / rect.width;
      const time = Math.max(0, Math.min(percent * duration, duration));
      setHoverTime(time);
      setHoverPosition(percent * 100);
    };

    const handleProgressLeave = () => {
      setHoverTime(null);
      setHoverPosition(null);
    };

    // Toggle fullscreen
    const toggleFullscreen = () => {
      if (!containerRef.current) return;

      if (!isFullscreen) {
        if (containerRef.current.requestFullscreen) {
          containerRef.current.requestFullscreen();
        } else if (
          (
            containerRef.current as HTMLElement & {
              webkitRequestFullscreen?: () => void;
            }
          ).webkitRequestFullscreen
        ) {
          (
            containerRef.current as HTMLElement & {
              webkitRequestFullscreen?: () => void;
            }
          ).webkitRequestFullscreen?.();
        }
        setIsFullscreen(true);
      } else {
        if (document.fullscreenElement) {
          document.exitFullscreen();
        }
        setIsFullscreen(false);
      }
    };

    const toggleTheaterMode = () => {
      const newTheaterMode = !isTheaterMode;
      setIsTheaterMode(newTheaterMode);

      // Lock/unlock body scroll
      if (newTheaterMode) {
        document.body.style.overflow = "hidden";
      } else {
        document.body.style.overflow = "";
      }
    };

    // Handle Picture-in-Picture
    const togglePictureInPicture = async () => {
      try {
        if (document.pictureInPictureElement) {
          await document.exitPictureInPicture();
          setIsPictureInPicture(false);
        } else if (videoRef.current && document.pictureInPictureEnabled) {
          await videoRef.current.requestPictureInPicture();
          setIsPictureInPicture(true);
        }
      } catch (error) {
        console.error("PiP error:", error);
      }
    };

    // Handle quality change
    const handleQualityChange = (newQuality: string) => {
      if (!videoRef.current) return;

      const currentTime = videoRef.current.currentTime;
      const wasPlaying = !videoRef.current.paused;

      setQuality(newQuality);

      if (Array.isArray(src)) {
        let newSrc = "";
        if (newQuality === "auto") {
          newSrc = src[0]?.src || "";
        } else {
          const source = src.find((s) => s.quality === newQuality);
          if (source) newSrc = source.src;
        }

        if (newSrc && newSrc !== currentSrc) {
          setCurrentSrc(newSrc);
          // Restore playback position after source change
          const handleCanPlay = () => {
            if (videoRef.current) {
              videoRef.current.currentTime = currentTime;
              if (wasPlaying) videoRef.current.play();
              videoRef.current.removeEventListener(
                "loadedmetadata",
                handleCanPlay,
              );
            }
          };
          videoRef.current.addEventListener("loadedmetadata", handleCanPlay);
        }
      }
    };

    // Handle speed change
    const handleSpeedChange = (newSpeed: number) => {
      setSpeed(newSpeed);
      if (videoRef.current) {
        videoRef.current.playbackRate = newSpeed;
      }
    };

    // Handle skip
    const handleSkip = (seconds: number) => {
      if (videoRef.current) {
        videoRef.current.currentTime += seconds;
      }
    };

    const handleToggleLoop = () => {
      if (videoRef.current) {
        videoRef.current.loop = !videoRef.current.loop;
        setIsLooping(!isLooping);
      }
    };

    // Handle video metadata loaded
    const handleLoadedMetadata = () => {
      setDuration(videoRef.current?.duration || 0);
      setIsLoading(false);
    };

    // Handle time update
    const handleTimeUpdate = () => {
      setCurrentTime(videoRef.current?.currentTime || 0);
      if (onTimeUpdate) {
        onTimeUpdate(videoRef.current?.currentTime || 0);
      }

      // Update buffered amount
      if (videoRef.current && videoRef.current.buffered.length > 0) {
        const bufferedEnd = videoRef.current.buffered.end(
          videoRef.current.buffered.length - 1,
        );
        setBuffered((bufferedEnd / duration) * 100);
      }
    };

    // Handle fullscreen change
    useEffect(() => {
      const handleFullscreenChange = () => {
        setIsFullscreen(!!document.fullscreenElement);
      };
      document.addEventListener("fullscreenchange", handleFullscreenChange);
      return () =>
        document.removeEventListener(
          "fullscreenchange",
          handleFullscreenChange,
        );
    }, []);

    // Handle mouse movement for controls
    useEffect(() => {
      const container = containerRef.current;
      if (!container) return;

      const handleMouseMove = () => {
        setShowControls(true);
        if (controlsTimeoutRef.current) {
          clearTimeout(controlsTimeoutRef.current);
        }

        if (isPlaying) {
          controlsTimeoutRef.current = setTimeout(() => {
            setShowControls(false);
          }, 3000);
        }
      };

      container.addEventListener("mousemove", handleMouseMove);
      return () => container.removeEventListener("mousemove", handleMouseMove);
    }, [isPlaying]);

    return (
      <div
        ref={containerRef}
        className={`group relative w-full overflow-hidden rounded-lg bg-black ${
          isTheaterMode
            ? "fixed inset-0 z-50 h-screen w-screen rounded-none"
            : ""
        }`}
      >
        <div
          className={`relative w-full transition-all duration-300 ${isTheaterMode ? "h-screen" : "aspect-video"}`}
        >
          {/* Video Element */}
          {/* biome-ignore lint/a11y/useMediaCaption: Video player component may not have captions */}
          <video
            ref={videoRef}
            src={currentSrc}
            poster={poster}
            onLoadedMetadata={handleLoadedMetadata}
            onTimeUpdate={handleTimeUpdate}
            onPlay={() => setIsPlaying(true)}
            onPause={() => setIsPlaying(false)}
            onLoadStart={() => setIsLoading(true)}
            onEnded={() => setIsPlaying(false)}
            className="absolute inset-0 h-full w-full"
          >
            {tracks.map((track, index) => (
              <track
                key={index}
                kind="subtitles"
                src={track.src}
                srcLang={track.srcLang}
                label={track.label}
                default={track.default}
              />
            ))}
          </video>

          {/* Loading Spinner */}
          <AnimatePresence>
            {isLoading && (
              <motion.div
                initial={{ opacity: 0 }}
                animate={{ opacity: 1 }}
                exit={{ opacity: 0 }}
                className="absolute inset-0 flex items-center justify-center bg-black/20 backdrop-blur-sm"
              >
                <motion.div
                  animate={{ rotate: 360 }}
                  transition={{ duration: 1, repeat: Number.POSITIVE_INFINITY }}
                >
                  <Loader2 className="h-12 w-12 text-white" />
                </motion.div>
              </motion.div>
            )}
          </AnimatePresence>

          {/* Controls Background Gradient */}
          <AnimatePresence>
            {showControls && (
              <motion.div
                initial={{ opacity: 0 }}
                animate={{ opacity: 1 }}
                exit={{ opacity: 0 }}
                className="absolute right-0 bottom-0 left-0 h-32 bg-gradient-to-t from-black via-black/50 to-transparent"
              />
            )}
          </AnimatePresence>

          {/* Click Overlay */}
          <div
            className="absolute inset-0 z-10"
            onClick={handleTap}
            role="button"
            aria-label="Play/Pause"
            tabIndex={0}
          />

          {/* Double Tap Animation */}
          <AnimatePresence>
            {doubleTapAction && (
              <div
                key={doubleTapAction.id}
                className={`absolute inset-y-0 ${
                  doubleTapAction.side === "left"
                    ? "left-0 justify-start pl-12"
                    : "right-0 justify-end pr-12"
                } pointer-events-none z-20 flex w-1/2 items-center`}
              >
                <motion.div
                  initial={{ opacity: 0, scale: 0.5 }}
                  animate={{ opacity: 1, scale: 1 }}
                  exit={{ opacity: 0, scale: 1.5 }}
                  transition={{ duration: 0.5 }}
                  className="flex flex-col items-center justify-center rounded-full shadow-lg"
                >
                  {doubleTapAction.side === "left" ? (
                    <>
                      <ChevronsLeft className="h-8 w-8 text-white" />
                      <span className="mt-1 select-none font-bold text-white text-xs shadow-lg">
                        10s
                      </span>
                    </>
                  ) : (
                    <>
                      <ChevronsRight className="h-8 w-8 text-white" />
                      <span className="mt-1 select-none font-bold text-white text-xs shadow-lg">
                        10s
                      </span>
                    </>
                  )}
                </motion.div>
              </div>
            )}
          </AnimatePresence>

          {/* Center Play Button */}
          <AnimatePresence>
            {showControls && !isPlaying && (
              <motion.button
                initial={{ opacity: 0, scale: 0.8 }}
                animate={{ opacity: 1, scale: 1 }}
                exit={{ opacity: 0, scale: 0.8 }}
                className="pointer-events-none absolute inset-0 z-20 flex items-center justify-center"
              >
                <motion.div
                  whileHover={{ scale: 1.1 }}
                  whileTap={{ scale: 0.95 }}
                  className="pointer-events-auto rounded-full bg-white/20 p-4 backdrop-blur-sm transition-colors hover:bg-white/30"
                  onClick={togglePlay}
                >
                  <Play className="h-12 w-12 fill-white text-white" />
                </motion.div>
              </motion.button>
            )}
          </AnimatePresence>

          {/* Controls Bar */}
          <AnimatePresence>
            {showControls && (
              <motion.div
                initial={{ opacity: 0, y: 20 }}
                animate={{ opacity: 1, y: 0 }}
                exit={{ opacity: 0, y: 20 }}
                className="absolute right-0 bottom-0 left-0 z-40 flex flex-col gap-3 px-4 py-3"
              >
                {/* Progress Bar */}
                <div
                  onClick={handleProgressClick}
                  onMouseMove={handleProgressHover}
                  onMouseLeave={handleProgressLeave}
                  className="group/progress relative h-1.5 w-full cursor-pointer rounded-full bg-white/20 transition-all hover:h-2"
                  role="button"
                  aria-label="Seek"
                  tabIndex={0}
                >
                  {/* Buffered indicator */}
                  <div
                    className="absolute inset-y-0 left-0 rounded-full bg-white/40"
                    style={{ width: `${buffered}%` }}
                  />

                  {/* Progress indicator */}
                  <div
                    className="absolute inset-y-0 left-0 rounded-full bg-gradient-to-r from-blue-500 to-cyan-400 transition-all"
                    style={{ width: `${(currentTime / duration) * 100}%` }}
                  />

                  {/* Chapter markers */}
                  {chapters.length > 0 && duration > 0 && (
                    <div className="pointer-events-none absolute inset-0 h-full w-full">
                      {chapters.map((chapter, index) => {
                        if (index === 0) return null;
                        const left = (chapter.startTime / duration) * 100;
                        return (
                          <div
                            key={index}
                            className="absolute top-0 bottom-0 z-10 w-0.5 bg-black/50"
                            style={{ left: `${left}%` }}
                          />
                        );
                      })}
                    </div>
                  )}

                  {/* Scrubber */}
                  <motion.div
                    className="absolute top-1/2 z-20 h-4 w-4 -translate-x-1/2 -translate-y-1/2 rounded-full bg-white opacity-0 shadow-lg transition-opacity group-hover/progress:opacity-100"
                    style={{ left: `${(currentTime / duration) * 100}%` }}
                  />

                  {/* Hover Time Tooltip */}
                  <AnimatePresence>
                    {hoverTime !== null && hoverPosition !== null && (
                      <motion.div
                        initial={{ opacity: 0, y: 10, scale: 0.8 }}
                        animate={{ opacity: 1, y: 0, scale: 1 }}
                        exit={{ opacity: 0, y: 10, scale: 0.8 }}
                        className="pointer-events-none absolute bottom-full z-50 mb-4 flex -translate-x-1/2 flex-col items-center gap-0.5 whitespace-nowrap rounded-lg border border-white/10 bg-black/90 px-2 py-1 text-white text-xs"
                        style={{ left: `${hoverPosition}%` }}
                      >
                        {chapters.length > 0 && (
                          <span className="font-medium text-white/90">
                            {
                              chapters.find((c, i) => {
                                const nextChapter = chapters[i + 1];
                                return (
                                  hoverTime >= c.startTime &&
                                  (!nextChapter ||
                                    hoverTime < nextChapter.startTime)
                                );
                              })?.title
                            }
                          </span>
                        )}
                        <span
                          className={chapters.length > 0 ? "text-white/70" : ""}
                        >
                          {formatTime(hoverTime)}
                        </span>
                      </motion.div>
                    )}
                  </AnimatePresence>
                </div>

                {/* Time Display and Controls */}
                <div className="flex items-center justify-between gap-2">
                  {/* Left Controls */}
                  <div className="flex items-center gap-1">
                    {/* Play/Pause */}
                    <Tooltip
                      label={isPlaying ? "Pause (Space)" : "Play (Space)"}
                    >
                      <motion.button
                        whileHover={{ scale: 1.1 }}
                        whileTap={{ scale: 0.95 }}
                        onClick={togglePlay}
                        className="flex items-center justify-center rounded-lg p-2 transition-colors hover:bg-white/20"
                        aria-label="Play/Pause"
                      >
                        {isPlaying ? (
                          <Pause className="h-5 w-5 fill-white text-white" />
                        ) : (
                          <Play className="h-5 w-5 fill-white text-white" />
                        )}
                      </motion.button>
                    </Tooltip>

                    {/* Skip Back 10s */}
                    <Tooltip label="Previous 10 seconds (J)">
                      <motion.button
                        whileHover={{ scale: 1.1 }}
                        whileTap={{ scale: 0.95 }}
                        onClick={() => handleSkip(-10)}
                        className="flex items-center justify-center rounded-lg p-2 transition-colors hover:bg-white/20"
                        aria-label="Skip back 10 seconds"
                      >
                        <RotateCcw className="h-5 w-5 text-white" />
                      </motion.button>
                    </Tooltip>

                    {/* Skip Forward 10s */}
                    <Tooltip label="Next 10 seconds (L)">
                      <motion.button
                        whileHover={{ scale: 1.1 }}
                        whileTap={{ scale: 0.95 }}
                        onClick={() => handleSkip(10)}
                        className="flex items-center justify-center rounded-lg p-2 transition-colors hover:bg-white/20"
                        aria-label="Skip forward 10 seconds"
                      >
                        <RotateCw className="h-5 w-5 text-white" />
                      </motion.button>
                    </Tooltip>

                    {/* Previous Video */}
                    {currentVideoIndex > 0 && (
                      <Tooltip label="Previous Video">
                        <motion.button
                          whileHover={{ scale: 1.1 }}
                          whileTap={{ scale: 0.95 }}
                          onClick={onPrevVideo}
                          className="flex items-center justify-center rounded-lg p-2 transition-colors hover:bg-white/20"
                          aria-label="Previous video"
                        >
                          <ChevronLeft className="h-5 w-5 text-white" />
                        </motion.button>
                      </Tooltip>
                    )}

                    {/* Next Video */}
                    {currentVideoIndex < totalVideos - 1 && (
                      <Tooltip label="Next Video">
                        <motion.button
                          whileHover={{ scale: 1.1 }}
                          whileTap={{ scale: 0.95 }}
                          onClick={onNextVideo}
                          className="flex items-center justify-center rounded-lg p-2 transition-colors hover:bg-white/20"
                          aria-label="Next video"
                        >
                          <ChevronRight className="h-5 w-5 text-white" />
                        </motion.button>
                      </Tooltip>
                    )}

                    {/* Volume Control */}
                    <div className="flex items-center gap-1">
                      <Tooltip label={isMuted ? "Unmute (M)" : "Mute (M)"}>
                        <motion.button
                          whileHover={{ scale: 1.1 }}
                          whileTap={{ scale: 0.95 }}
                          onMouseEnter={() => setShowVolumeSlider(true)}
                          onMouseLeave={() => setShowVolumeSlider(false)}
                          onClick={toggleMute}
                          className="flex items-center justify-center rounded-lg p-2 transition-colors hover:bg-white/20"
                          aria-label="Mute/Unmute"
                        >
                          {isMuted ? (
                            <VolumeX className="h-5 w-5 text-white" />
                          ) : (
                            <Volume2 className="h-5 w-5 text-white" />
                          )}
                        </motion.button>
                      </Tooltip>

                      {/* Volume Slider */}
                      <AnimatePresence>
                        {showVolumeSlider && (
                          <motion.div
                            initial={{ opacity: 0, width: 0 }}
                            animate={{ opacity: 1, width: 80 }}
                            exit={{ opacity: 0, width: 0 }}
                            className="flex items-center overflow-hidden pl-2"
                            onMouseEnter={() => setShowVolumeSlider(true)}
                            onMouseLeave={() => setShowVolumeSlider(false)}
                          >
                            <input
                              type="range"
                              min="0"
                              max="1"
                              step="0.01"
                              value={isMuted ? 0 : volume}
                              onChange={(e) =>
                                handleVolumeChange([
                                  Number.parseFloat(e.target.value),
                                ])
                              }
                              className="h-1 w-full cursor-pointer appearance-none rounded-full focus:outline-none [&::-moz-range-thumb]:h-3 [&::-moz-range-thumb]:w-3 [&::-moz-range-thumb]:rounded-full [&::-moz-range-thumb]:border-none [&::-moz-range-thumb]:bg-white [&::-webkit-slider-thumb]:h-3 [&::-webkit-slider-thumb]:w-3 [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:rounded-full [&::-webkit-slider-thumb]:bg-white"
                              style={{
                                background: `linear-gradient(to right, white ${isMuted ? 0 : volume * 100}%, rgba(255, 255, 255, 0.2) ${isMuted ? 0 : volume * 100}%)`,
                              }}
                            />
                          </motion.div>
                        )}
                      </AnimatePresence>
                    </div>

                    {/* Time Display */}
                    <span className="ml-2 flex min-w-24 items-center text-sm text-white">
                      {formatTime(currentTime)} / {formatTime(duration)}
                    </span>
                  </div>

                  {/* Right Controls */}
                  <div className="flex items-center gap-1">
                    {/* Captions */}
                    {tracks.length > 0 && (
                      <div className="relative flex items-center">
                        <Tooltip label="Captions (C)">
                          <motion.button
                            whileHover={{ scale: 1.1 }}
                            whileTap={{ scale: 0.95 }}
                            onClick={() => {
                              if (tracks.length === 1 && tracks[0]) {
                                handleCaptionChange(
                                  currentCaption ? null : tracks[0].srcLang,
                                );
                              } else {
                                setActiveDialog(
                                  activeDialog === "captions"
                                    ? null
                                    : "captions",
                                );
                              }
                            }}
                            className={`flex items-center justify-center rounded-lg p-2 transition-colors ${
                              currentCaption
                                ? "bg-cyan-500/30 hover:bg-cyan-500/40"
                                : "hover:bg-white/20"
                            }`}
                            aria-label="Captions"
                          >
                            <MessageCircle className="h-5 w-5 text-white" />
                          </motion.button>
                        </Tooltip>

                        <AnimatePresence>
                          {activeDialog === "captions" && tracks.length > 1 && (
                            <motion.div
                              initial={{ opacity: 0, y: 10 }}
                              animate={{ opacity: 1, y: 0 }}
                              exit={{ opacity: 0, y: 10 }}
                              className="absolute right-0 bottom-full z-50 mb-2 min-w-40 rounded-lg border border-white/10 bg-black/95 p-3 backdrop-blur-sm"
                            >
                              <p className="mb-2 font-semibold text-white text-xs uppercase opacity-70">
                                Captions
                              </p>
                              <div className="space-y-1">
                                <button
                                  onClick={() => {
                                    handleCaptionChange(null);
                                    setActiveDialog(null);
                                  }}
                                  className={`w-full rounded px-2 py-1 text-left text-sm transition-colors ${
                                    !currentCaption
                                      ? "bg-cyan-500 text-white"
                                      : "text-white/70 hover:bg-white/10"
                                  }`}
                                >
                                  Off
                                </button>
                                {tracks.map((track) => (
                                  <button
                                    key={track.srcLang}
                                    onClick={() => {
                                      handleCaptionChange(track.srcLang);
                                      setActiveDialog(null);
                                    }}
                                    className={`w-full rounded px-2 py-1 text-left text-sm transition-colors ${
                                      currentCaption === track.srcLang
                                        ? "bg-cyan-500 text-white"
                                        : "text-white/70 hover:bg-white/10"
                                    }`}
                                  >
                                    {track.label}
                                  </button>
                                ))}
                              </div>
                            </motion.div>
                          )}
                        </AnimatePresence>
                      </div>
                    )}

                    {/* Picture-in-Picture */}
                    <Tooltip label="Picture in Picture (P)">
                      <motion.button
                        whileHover={{ scale: 1.1 }}
                        whileTap={{ scale: 0.95 }}
                        onClick={togglePictureInPicture}
                        className={`flex items-center justify-center rounded-lg p-2 transition-colors ${
                          isPictureInPicture
                            ? "bg-cyan-500/30 hover:bg-cyan-500/40"
                            : "hover:bg-white/20"
                        }`}
                        aria-label="Picture in Picture"
                      >
                        <PictureInPicture2 className="h-5 w-5 text-white" />
                      </motion.button>
                    </Tooltip>

                    {/* Settings */}
                    <div className="relative flex items-center">
                      <Tooltip label="Settings">
                        <motion.button
                          whileHover={{ scale: 1.1 }}
                          whileTap={{ scale: 0.95 }}
                          onClick={() =>
                            setActiveDialog(
                              activeDialog === "settings" ? null : "settings",
                            )
                          }
                          className={`flex items-center justify-center rounded-lg p-2 transition-colors ${
                            activeDialog === "settings"
                              ? "bg-cyan-500/30"
                              : "hover:bg-white/20"
                          }`}
                          aria-label="Settings"
                        >
                          <Settings className="h-5 w-5 text-white" />
                        </motion.button>
                      </Tooltip>

                      <AnimatePresence>
                        {activeDialog === "settings" && (
                          <motion.div
                            initial={{ opacity: 0, y: 10 }}
                            animate={{ opacity: 1, y: 0 }}
                            exit={{ opacity: 0, y: 10 }}
                            className="absolute right-0 bottom-full z-50 mb-2 min-w-40 rounded-lg border border-white/10 bg-black/95 p-3 backdrop-blur-sm"
                          >
                            {/* Quality Selection */}
                            {availableQualities.length > 1 && (
                              <div className="mb-3">
                                <p className="mb-2 font-semibold text-white text-xs uppercase opacity-70">
                                  Quality
                                </p>
                                <div className="space-y-1">
                                  {availableQualities.map((q) => (
                                    <button
                                      key={q}
                                      onClick={() => handleQualityChange(q)}
                                      className={`w-full rounded px-2 py-1 text-left text-sm transition-colors ${
                                        quality === q
                                          ? "bg-cyan-500 text-white"
                                          : "text-white/70 hover:bg-white/10"
                                      }`}
                                    >
                                      {q}
                                    </button>
                                  ))}
                                </div>
                              </div>
                            )}

                            {/* Speed Selection */}
                            <div>
                              <p className="mb-2 font-semibold text-white text-xs uppercase opacity-70">
                                Speed
                              </p>
                              <div className="space-y-1">
                                {[0.5, 0.75, 1, 1.25, 1.5, 2].map((s) => (
                                  <button
                                    key={s}
                                    onClick={() => handleSpeedChange(s)}
                                    className={`w-full rounded px-2 py-1 text-left text-sm transition-colors ${
                                      speed === s
                                        ? "bg-cyan-500 text-white"
                                        : "text-white/70 hover:bg-white/10"
                                    }`}
                                  >
                                    {s}x
                                  </button>
                                ))}
                              </div>
                            </div>
                          </motion.div>
                        )}
                      </AnimatePresence>
                    </div>

                    {/* More Options */}
                    <div className="relative flex items-center">
                      <Tooltip label="More Options">
                        <motion.button
                          whileHover={{ scale: 1.1 }}
                          whileTap={{ scale: 0.95 }}
                          onClick={() =>
                            setActiveDialog(
                              activeDialog === "options" ? null : "options",
                            )
                          }
                          className={`flex items-center justify-center rounded-lg p-2 transition-colors ${
                            activeDialog === "options"
                              ? "bg-cyan-500/30"
                              : "hover:bg-white/20"
                          }`}
                          aria-label="More options"
                        >
                          <MoreVertical className="h-5 w-5 text-white" />
                        </motion.button>
                      </Tooltip>

                      <AnimatePresence>
                        {activeDialog === "options" && (
                          <motion.div
                            initial={{ opacity: 0, y: 10 }}
                            animate={{ opacity: 1, y: 0 }}
                            exit={{ opacity: 0, y: 10 }}
                            className="absolute right-0 bottom-full z-50 mb-2 min-w-48 rounded-lg border border-white/10 bg-black/95 p-2 backdrop-blur-sm"
                          >
                            {/* Theater Mode */}
                            <button
                              onClick={() => {
                                toggleTheaterMode();
                                setActiveDialog(null);
                              }}
                              className="flex w-full items-center gap-2 rounded px-3 py-2 text-left text-sm text-white transition-colors hover:bg-white/10"
                            >
                              <Maximize2 className="h-4 w-4" />
                              {isTheaterMode
                                ? "Exit Theater Mode"
                                : "Theater Mode"}
                            </button>

                            {/* Loop */}
                            <button
                              onClick={() => handleToggleLoop()}
                              className={`flex w-full items-center gap-2 rounded px-3 py-2 text-left text-sm transition-colors ${
                                isLooping
                                  ? "bg-cyan-500/30 text-white"
                                  : "text-white hover:bg-white/10"
                              }`}
                            >
                              <Repeat className="h-4 w-4" />
                              Loop
                            </button>
                          </motion.div>
                        )}
                      </AnimatePresence>
                    </div>

                    {/* Fullscreen */}
                    <Tooltip
                      label={
                        isFullscreen ? "Exit Fullscreen (F)" : "Fullscreen (F)"
                      }
                    >
                      <motion.button
                        whileHover={{ scale: 1.1 }}
                        whileTap={{ scale: 0.95 }}
                        onClick={toggleFullscreen}
                        className="flex items-center justify-center rounded-lg p-2 transition-colors hover:bg-white/20"
                        aria-label="Fullscreen"
                      >
                        {isFullscreen ? (
                          <Minimize className="h-5 w-5 text-white" />
                        ) : (
                          <Maximize className="h-5 w-5 text-white" />
                        )}
                      </motion.button>
                    </Tooltip>
                  </div>
                </div>
              </motion.div>
            )}
          </AnimatePresence>
        </div>
      </div>
    );
  },
);
VideoPlayer.displayName = "VideoPlayer";
```
---
