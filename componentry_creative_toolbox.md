# Componentry Creative Developer Reference Guide

A complete, structured reference guide for **Componentry** (componentry.fun) — an animation-centric react component library packed with physics effects, matrix animations, split flaps, and webgl canvases, built with Framer Motion, Canvas, and Tailwind CSS.

## Table of Contents
- [Animated Gradient](#animated-gradient)
- [Auth Modal](#auth-modal)
- [Border Beam](#border-beam)
- [Circuit Board](#circuit-board)
- [Closing Plasma](#closing-plasma)
- [Collection Surfer](#collection-surfer)
- [Command Menu](#command-menu)
- [Cursor Driven Particle Typography](#cursor-driven-particle-typography)
- [Dither Gradient](#dither-gradient)
- [Dither Prism Hero](#dither-prism-hero)
- [Eye Tracking](#eye-tracking)
- [Flight Status Card](#flight-status-card)
- [Github Calendar](#github-calendar)
- [Hero Geometric](#hero-geometric)
- [Hyper Text](#hyper-text)
- [Image Ripple Effect](#image-ripple-effect)
- [Image Trail](#image-trail)
- [Infinite Image Field](#infinite-image-field)
- [Interactive Hover Button](#interactive-hover-button)
- [Layered Stack](#layered-stack)
- [Letter Cascade](#letter-cascade)
- [Liquid Blob](#liquid-blob)
- [Mac Keyboard](#mac-keyboard)
- [Magnet Lines](#magnet-lines)
- [Magnetic Dock](#magnetic-dock)
- [Matrix Rain](#matrix-rain)
- [Music Player](#music-player)
- [Noise Texture](#noise-texture)
- [Particle Galaxy](#particle-galaxy)
- [Pixel Canvas](#pixel-canvas)
- [Pulsating Button](#pulsating-button)
- [Scroll Based Velocity](#scroll-based-velocity)
- [Scroll Choreography](#scroll-choreography)
- [Scroll Split Card](#scroll-split-card)
- [Scrub Input](#scrub-input)
- [Shimmer Button](#shimmer-button)
- [Showcase Card](#showcase-card)
- [Signature](#signature)
- [Split Flap Display](#split-flap-display)
- [Spotlight Card](#spotlight-card)
- [Sticky Scroll Cards](#sticky-scroll-cards)
- [Testimonial Marquee](#testimonial-marquee)
- [Text Animate](#text-animate)
- [Text Repel](#text-repel)
- [Webgl Liquid](#webgl-liquid)

---

## Animated Gradient <a name="animated-gradient"></a>

Component for animated-gradient

- **Registry Key**: `animated-gradient`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/animated-gradient.json
```

#### How to Use
```tsx
"use client";

import { AnimatedGradient } from "@workspace/ui/components/animated-gradient";
import type { GradientConfig } from "@workspace/ui/components/animated-gradient";

interface AnimatedGradientPreviewProps {
    config?: GradientConfig;
}

export function AnimatedGradientPreview({
    config = { preset: "Aurora" }
}: AnimatedGradientPreviewProps) {
    return (
        <div className="relative h-[55vh] lg:h-full w-full bg-[#f3f4f6] dark:bg-[#080808]">
            <div className="relative h-full w-full overflow-hidden rounded-none border-none">
                <AnimatedGradient config={config} noise={{ opacity: 0.2, scale: 1 }} className="!min-h-full" style={{ minHeight: "100%" }} />
                <div className="absolute inset-0 flex flex-col items-center justify-center pointer-events-none z-10 px-4 text-center">
                    <h2 className="text-5xl md:text-7xl font-extrabold tracking-tight text-white drop-shadow-[0_4px_24px_rgba(0,0,0,0.6)]">
                        Animated
                    </h2>
                    <h2 className="text-5xl md:text-7xl font-serif italic tracking-tight text-transparent bg-clip-text bg-gradient-to-br from-white via-white/90 to-white/40 drop-shadow-[0_4px_24px_rgba(0,0,0,0.4)] mt-2 pb-2 pr-4">
                        Gradient
                    </h2>
                </div>
            </div>
        </div>
    );
}
```
#### Component Source Code
##### File: `components/ui/animated-gradient.tsx`
```tsx
"use client";

import { useRef, useEffect, useMemo, useState, CSSProperties } from "react";
import { cn } from "@/lib/utils";
import { WebGLErrorBoundary, WebGLFallback } from "./webgl-error-boundary";

type PatternShape = "Checks" | "Stripes" | "Edge";

const PatternShapes: Record<PatternShape, number> = {
    Checks: 0,
    Stripes: 1,
    Edge: 2,
};

interface PresetParams {
    color1: string;
    color2: string;
    color3: string;
    rotation: number;
    proportion: number;
    scale: number;
    speed: number;
    distortion: number;
    swirl: number;
    swirlIterations: number;
    softness: number;
    offset: number;
    shape: PatternShape;
    shapeSize: number;
}

const presets: Record<PresetName, PresetParams> = {
    Aurora: {
        color1: "#0a001a",
        color2: "#1a0b2e",
        color3: "#f20089",
        rotation: -45,
        proportion: 60,
        scale: 0.6,
        speed: 15,
        distortion: 40,
        swirl: 80,
        swirlIterations: 10,
        softness: 100,
        offset: 200,
        shape: "Edge",
        shapeSize: 50,
    },
    Oceanic: {
        color1: "#000814",
        color2: "#001d3d",
        color3: "#00b4d8",
        rotation: 0,
        proportion: 70,
        scale: 0.4,
        speed: 10,
        distortion: 15,
        swirl: 50,
        swirlIterations: 12,
        softness: 80,
        offset: 150,
        shape: "Checks",
        shapeSize: 30,
    },
    Amber: {
        color1: "#140c00",
        color2: "#4a2500",
        color3: "#f57c00",
        rotation: 120,
        proportion: 80,
        scale: 0.8,
        speed: 20,
        distortion: 25,
        swirl: 60,
        swirlIterations: 8,
        softness: 90,
        offset: 500,
        shape: "Stripes",
        shapeSize: 40,
    },
    Toxic: {
        color1: "#050d05",
        color2: "#0a240a",
        color3: "#39ff14",
        rotation: -90,
        proportion: 55,
        scale: 0.5,
        speed: 25,
        distortion: 60,
        swirl: 100,
        swirlIterations: 15,
        softness: 70,
        offset: -100,
        shape: "Edge",
        shapeSize: 20,
    },
    Ghost: {
        color1: "#0a0a0a",
        color2: "#1c1c1c",
        color3: "#a3a3a3",
        rotation: 45,
        proportion: 50,
        scale: 0.3,
        speed: 8,
        distortion: 10,
        swirl: 30,
        swirlIterations: 5,
        softness: 100,
        offset: 0,
        shape: "Checks",
        shapeSize: 60,
    },
};

type PresetName = "Aurora" | "Oceanic" | "Amber" | "Toxic" | "Ghost";

export interface CustomConfig {
    preset: "custom";
    color1: string;
    color2: string;
    color3: string;
    rotation?: number;
    proportion?: number;
    scale?: number;
    speed?: number;
    distortion?: number;
    swirl?: number;
    swirlIterations?: number;
    softness?: number;
    offset?: number;
    shape?: PatternShape;
    shapeSize?: number;
}

export interface PresetConfig {
    preset: PresetName;
    speed?: number;
}

export type GradientConfig = CustomConfig | PresetConfig;

export interface NoiseConfig {
    opacity: number;
    scale?: number;
}

export interface AnimatedGradientProps {
    config?: GradientConfig;
    noise?: NoiseConfig;
    radius?: string;
    style?: CSSProperties;
    className?: string;
}

export function AnimatedGradient({
    config = { preset: "Aurora" },
    noise,
    radius = "0px",
    style,
    className,
}: AnimatedGradientProps) {
    const canvasRef = useRef<HTMLCanvasElement>(null);
    const containerRef = useRef<HTMLDivElement>(null);
    const frameIdRef = useRef<number | undefined>(undefined);
    const startTimeRef = useRef<number>(0);

    const [isMounted, setIsMounted] = useState(false);
    const [hasWebGLError, setHasWebGLError] = useState(false);

    useEffect(() => {
        setIsMounted(true);
        return () => setIsMounted(false);
    }, []);

    const params = useMemo((): PresetParams => {
        if (config.preset === "custom") {
            return {
                color1: config.color1,
                color2: config.color2,
                color3: config.color3,
                rotation: config.rotation ?? 0,
                proportion: config.proportion ?? 35,
                scale: config.scale ?? 1,
                speed: config.speed ?? 25,
                distortion: config.distortion ?? 12,
                swirl: config.swirl ?? 80,
                swirlIterations: config.swirlIterations ?? 10,
                softness: config.softness ?? 100,
                offset: config.offset ?? 0,
                shape: config.shape ?? "Checks",
                shapeSize: config.shapeSize ?? 10,
            };
        }
        const preset = presets[config.preset] || presets.Aurora;
        return {
            ...preset,
            speed: config.speed ?? preset.speed,
        };
    }, [config]);

    useEffect(() => {
        if (hasWebGLError) return;

        const canvas = canvasRef.current;
        const container = containerRef.current;
        if (!canvas || !container || !isMounted) return;

        try {
            const gl = canvas.getContext("webgl2", {
                premultipliedAlpha: true,
                alpha: true,
                antialias: true,
            });
            if (!gl) {
                setHasWebGLError(true);
                return;
            }

            const vertexShaderSource = `#version 300 es
    in vec4 a_position;
    void main() {
      gl_Position = a_position;
    }`;

            const vertexShader = gl.createShader(gl.VERTEX_SHADER)!;
            gl.shaderSource(vertexShader, vertexShaderSource);
            gl.compileShader(vertexShader);
            if (!gl.getShaderParameter(vertexShader, gl.COMPILE_STATUS)) {
                gl.deleteShader(vertexShader);
                setHasWebGLError(true);
                return;
            }

            const fragmentShader = gl.createShader(gl.FRAGMENT_SHADER)!;
            gl.shaderSource(fragmentShader, FRAGMENT_SHADER);
            gl.compileShader(fragmentShader);
            if (!gl.getShaderParameter(fragmentShader, gl.COMPILE_STATUS)) {
                gl.deleteShader(vertexShader);
                gl.deleteShader(fragmentShader);
                setHasWebGLError(true);
                return;
            }

            const program = gl.createProgram()!;
            gl.attachShader(program, vertexShader);
            gl.attachShader(program, fragmentShader);
            gl.linkProgram(program);
            if (!gl.getProgramParameter(program, gl.LINK_STATUS)) {
                gl.deleteProgram(program);
                gl.deleteShader(vertexShader);
                gl.deleteShader(fragmentShader);
                setHasWebGLError(true);
                return;
            }
            gl.useProgram(program);

            const positionBuffer = gl.createBuffer();
            gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer);
            gl.bufferData(
                gl.ARRAY_BUFFER,
                new Float32Array([-1, -1, 1, -1, -1, 1, -1, 1, 1, -1, 1, 1]),
                gl.STATIC_DRAW
            );

            const positionLocation = gl.getAttribLocation(program, "a_position");
            gl.enableVertexAttribArray(positionLocation);
            gl.vertexAttribPointer(positionLocation, 2, gl.FLOAT, false, 0, 0);

            const uniforms = {
                u_time: gl.getUniformLocation(program, "u_time"),
                u_resolution: gl.getUniformLocation(program, "u_resolution"),
                u_pixelRatio: gl.getUniformLocation(program, "u_pixelRatio"),
                u_scale: gl.getUniformLocation(program, "u_scale"),
                u_rotation: gl.getUniformLocation(program, "u_rotation"),
                u_color1: gl.getUniformLocation(program, "u_color1"),
                u_color2: gl.getUniformLocation(program, "u_color2"),
                u_color3: gl.getUniformLocation(program, "u_color3"),
                u_proportion: gl.getUniformLocation(program, "u_proportion"),
                u_softness: gl.getUniformLocation(program, "u_softness"),
                u_shape: gl.getUniformLocation(program, "u_shape"),
                u_shapeScale: gl.getUniformLocation(program, "u_shapeScale"),
                u_distortion: gl.getUniformLocation(program, "u_distortion"),
                u_swirl: gl.getUniformLocation(program, "u_swirl"),
                u_swirlIterations: gl.getUniformLocation(program, "u_swirlIterations"),
            };

            const resize = () => {
                const width = container.clientWidth;
                const height = container.clientHeight;
                const pixelRatio = window.devicePixelRatio || 1;
                canvas.width = width * pixelRatio;
                canvas.height = height * pixelRatio;
                canvas.style.width = `${width}px`;
                canvas.style.height = `${height}px`;
                gl.viewport(0, 0, canvas.width, canvas.height);
            };

            resize();
            const resizeObserver = new ResizeObserver(resize);
            resizeObserver.observe(container);

            startTimeRef.current = performance.now();

            const animate = (time: number) => {
                const elapsed = (time - startTimeRef.current) / 1000;
                const speed = (params.speed / 100) * 5;

                gl.uniform1f(uniforms.u_time, elapsed * speed + params.offset * 0.01);
                gl.uniform2f(uniforms.u_resolution, canvas.width, canvas.height);
                gl.uniform1f(uniforms.u_pixelRatio, window.devicePixelRatio || 1);
                gl.uniform1f(uniforms.u_scale, params.scale);
                gl.uniform1f(uniforms.u_rotation, (params.rotation * Math.PI) / 180);

                const c1 = hexToRgba(params.color1);
                const c2 = hexToRgba(params.color2);
                const c3 = hexToRgba(params.color3);
                gl.uniform4f(uniforms.u_color1, c1[0], c1[1], c1[2], c1[3]);
                gl.uniform4f(uniforms.u_color2, c2[0], c2[1], c2[2], c2[3]);
                gl.uniform4f(uniforms.u_color3, c3[0], c3[1], c3[2], c3[3]);

                gl.uniform1f(uniforms.u_proportion, params.proportion / 100);
                gl.uniform1f(uniforms.u_softness, params.softness / 100);
                gl.uniform1f(uniforms.u_shape, PatternShapes[params.shape]);
                gl.uniform1f(uniforms.u_shapeScale, params.shapeSize / 100);
                gl.uniform1f(uniforms.u_distortion, params.distortion / 50);
                gl.uniform1f(uniforms.u_swirl, params.swirl / 100);
                gl.uniform1f(
                    uniforms.u_swirlIterations,
                    params.swirl === 0 ? 0 : params.swirlIterations
                );

                gl.drawArrays(gl.TRIANGLES, 0, 6);
                frameIdRef.current = requestAnimationFrame(animate);
            };

            frameIdRef.current = requestAnimationFrame(animate);

            return () => {
                if (frameIdRef.current !== undefined) {
                    cancelAnimationFrame(frameIdRef.current);
                }
                resizeObserver.disconnect();
                gl.deleteProgram(program);
                gl.deleteShader(vertexShader);
                gl.deleteShader(fragmentShader);
                gl.deleteBuffer(positionBuffer);
            };
        } catch {
            setHasWebGLError(true);
            return;
        }
    }, [hasWebGLError, isMounted, params]);

    if (hasWebGLError) {
        return <WebGLFallback className={cn("absolute inset-0 overflow-hidden", className)} />;
    }

    return (
        <WebGLErrorBoundary
            fallback={<WebGLFallback className={cn("absolute inset-0 overflow-hidden", className)} />}
        >
            <div
                ref={containerRef}
                className={cn("absolute inset-0 overflow-hidden", className)}
                style={{
                    borderRadius: radius,
                    ...style,
                    // eslint-disable-next-line @typescript-eslint/no-explicit-any
                } as any}
            >
                <canvas
                    ref={canvasRef}
                    style={{
                        display: "block",
                        width: "100%",
                        height: "100%",
                    }}
                />
                {noise && noise.opacity > 0 && (
                    <div
                        style={{
                            position: "absolute",
                            inset: 0,
                            backgroundImage: `url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwBAMAAAClLOS0AAAAElBMVEUAAAAAAAAAAAAAAAAAAAAAAADgKxmiAAAABnRSTlMCCgkGBAVJOAVJAAAASklEQVQ4y2NgGAWjYBSMglEwCgY/YGRgZBQUYmJiZGQEkYwMjIyMgoKCjIyMIJKBgRFIMjIyAklGRkYGRkFBYEcwMDIyMjAOUQAA1I4HwVwZAkYAAAAASUVORK5CYII=")`,
                            backgroundSize: (noise.scale ?? 1) * 200,
                            backgroundRepeat: "repeat",
                            opacity: noise.opacity / 2,
                            pointerEvents: "none",
                        }}
                    />
                )}
            </div>
        </WebGLErrorBoundary>
    );
}

function hexToRgba(hex: string): [number, number, number, number] {
    let r = 0,
        g = 0,
        b = 0,
        a = 1;

    if (hex.startsWith("rgba(")) {
        const parts = hex.slice(5, -1).split(",");
        r = parseInt(parts[0] ?? "0") / 255;
        g = parseInt(parts[1] ?? "0") / 255;
        b = parseInt(parts[2] ?? "0") / 255;
        a = parseFloat(parts[3] ?? "1");
    } else if (hex.startsWith("rgb(")) {
        const parts = hex.slice(4, -1).split(",");
        r = parseInt(parts[0] ?? "0") / 255;
        g = parseInt(parts[1] ?? "0") / 255;
        b = parseInt(parts[2] ?? "0") / 255;
    } else if (hex.startsWith("hsla(") || hex.startsWith("hsl(")) {
        const isHsla = hex.startsWith("hsla(");
        const parts = hex.slice(isHsla ? 5 : 4, -1).split(",");
        const h = parseFloat(parts[0] ?? "0") / 360;
        const s = parseFloat(parts[1] ?? "0") / 100;
        const l = parseFloat(parts[2] ?? "0") / 100;
        a = isHsla ? parseFloat(parts[3] ?? "1") : 1;
        [r, g, b] = hslToRgb(h, s, l);
    } else if (hex.startsWith("#")) {
        const c = hex.slice(1);
        if (c.length === 3) {
            r = parseInt(c.charAt(0) + c.charAt(0), 16) / 255;
            g = parseInt(c.charAt(1) + c.charAt(1), 16) / 255;
            b = parseInt(c.charAt(2) + c.charAt(2), 16) / 255;
        } else if (c.length >= 6) {
            r = parseInt(c.slice(0, 2), 16) / 255;
            g = parseInt(c.slice(2, 4), 16) / 255;
            b = parseInt(c.slice(4, 6), 16) / 255;
            if (c.length === 8) {
                a = parseInt(c.slice(6, 8), 16) / 255;
            }
        }
    }

    return [r, g, b, a];
}

function hslToRgb(h: number, s: number, l: number): [number, number, number] {
    let r: number, g: number, b: number;

    if (s === 0) {
        r = g = b = l;
    } else {
        const hue2rgb = (p: number, q: number, t: number) => {
            if (t < 0) t += 1;
            if (t > 1) t -= 1;
            if (t < 1 / 6) return p + (q - p) * 6 * t;
            if (t < 1 / 2) return q;
            if (t < 2 / 3) return p + (q - p) * (2 / 3 - t) * 6;
            return p;
        };

        const q = l < 0.5 ? l * (1 + s) : l + s - l * s;
        const p = 2 * l - q;
        r = hue2rgb(p, q, h + 1 / 3);
        g = hue2rgb(p, q, h);
        b = hue2rgb(p, q, h - 1 / 3);
    }

    return [r, g, b];
}

const FRAGMENT_SHADER = `#version 300 es
precision highp float;

uniform float u_time;
uniform float u_pixelRatio;
uniform vec2 u_resolution;

uniform float u_scale;
uniform float u_rotation;
uniform vec4 u_color1;
uniform vec4 u_color2;
uniform vec4 u_color3;
uniform float u_proportion;
uniform float u_softness;
uniform float u_shape;
uniform float u_shapeScale;
uniform float u_distortion;
uniform float u_swirl;
uniform float u_swirlIterations;

out vec4 fragColor;

#define TWO_PI 6.28318530718
#define PI 3.14159265358979323846

vec2 rotate(vec2 uv, float th) {
  return mat2(cos(th), sin(th), -sin(th), cos(th)) * uv;
}

float random(vec2 st) {
  return fract(sin(dot(st.xy, vec2(12.9898, 78.233))) * 43758.5453123);
}

float noise(vec2 st) {
  vec2 i = floor(st);
  vec2 f = fract(st);
  float a = random(i);
  float b = random(i + vec2(1.0, 0.0));
  float c = random(i + vec2(0.0, 1.0));
  float d = random(i + vec2(1.0, 1.0));

  vec2 u = f * f * (3.0 - 2.0 * f);

  float x1 = mix(a, b, u.x);
  float x2 = mix(c, d, u.x);
  return mix(x1, x2, u.y);
}

vec4 blend_colors(vec4 c1, vec4 c2, vec4 c3, float mixer, float edgesWidth, float edge_blur) {
    vec3 color1 = c1.rgb * c1.a;
    vec3 color2 = c2.rgb * c2.a;
    vec3 color3 = c3.rgb * c3.a;

    float r1 = smoothstep(.0 + .35 * edgesWidth, .7 - .35 * edgesWidth + .5 * edge_blur, mixer);
    float r2 = smoothstep(.3 + .35 * edgesWidth, 1. - .35 * edgesWidth + edge_blur, mixer);

    vec3 blended_color_2 = mix(color1, color2, r1);
    float blended_opacity_2 = mix(c1.a, c2.a, r1);

    vec3 c = mix(blended_color_2, color3, r2);
    float o = mix(blended_opacity_2, c3.a, r2);
    return vec4(c, o);
}

void main() {
    vec2 uv = gl_FragCoord.xy / u_resolution.xy;

    float t = .5 * u_time;

    float noise_scale = .0005 + .006 * u_scale;

    uv -= .5;
    uv *= (noise_scale * u_resolution);
    uv = rotate(uv, u_rotation * .5 * PI);
    uv /= u_pixelRatio;
    uv += .5;

    float n1 = noise(uv * 1. + t);
    float n2 = noise(uv * 2. - t);
    float angle = n1 * TWO_PI;
    uv.x += 4. * u_distortion * n2 * cos(angle);
    uv.y += 4. * u_distortion * n2 * sin(angle);

    float iterations_number = ceil(clamp(u_swirlIterations, 1., 30.));
    for (float i = 1.; i <= iterations_number; i++) {
        uv.x += clamp(u_swirl, 0., 2.) / i * cos(t + i * 1.5 * uv.y);
        uv.y += clamp(u_swirl, 0., 2.) / i * cos(t + i * 1. * uv.x);
    }

    float proportion = clamp(u_proportion, 0., 1.);

    float shape = 0.;
    float mixer = 0.;
    if (u_shape < .5) {
      vec2 checks_shape_uv = uv * (.5 + 3.5 * u_shapeScale);
      shape = .5 + .5 * sin(checks_shape_uv.x) * cos(checks_shape_uv.y);
      mixer = shape + .48 * sign(proportion - .5) * pow(abs(proportion - .5), .5);
    } else if (u_shape < 1.5) {
      vec2 stripes_shape_uv = uv * (.25 + 3. * u_shapeScale);
      float f = fract(stripes_shape_uv.y);
      shape = smoothstep(.0, .55, f) * smoothstep(1., .45, f);
      mixer = shape + .48 * sign(proportion - .5) * pow(abs(proportion - .5), .5);
    } else {
      float sh = 1. - uv.y;
      sh -= .5;
      sh /= (noise_scale * u_resolution.y);
      sh += .5;
      float shape_scaling = .2 * (1. - u_shapeScale);
      shape = smoothstep(.45 - shape_scaling, .55 + shape_scaling, sh + .3 * (proportion - .5));
      mixer = shape;
    }

    vec4 color_mix = blend_colors(u_color1, u_color2, u_color3, mixer, 1. - clamp(u_softness, 0., 1.), .01 + .01 * u_scale);

    fragColor = vec4(color_mix.rgb, color_mix.a);
}
`;
```
##### File: `components/ui/webgl-error-boundary.tsx`
```tsx
"use client";

import { cn } from "@/lib/utils";
import * as React from "react";

interface WebGLErrorBoundaryProps {
  children: React.ReactNode;
  fallback?: React.ReactNode;
  onError?: (error: Error, errorInfo: React.ErrorInfo) => void;
}

interface WebGLErrorBoundaryState {
  hasError: boolean;
}

export class WebGLErrorBoundary extends React.Component<
  WebGLErrorBoundaryProps,
  WebGLErrorBoundaryState
> {
  public state: WebGLErrorBoundaryState = { hasError: false };

  static getDerivedStateFromError(): WebGLErrorBoundaryState {
    return { hasError: true };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    this.props.onError?.(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback ?? <WebGLFallback />;
    }
    return this.props.children;
  }
}

interface WebGLFallbackProps {
  className?: string;
  message?: string;
}

export function WebGLFallback({
  className,
  message = "Interactive WebGL content is unavailable on this device/browser.",
}: WebGLFallbackProps) {
  return (
    <div
      className={cn(
        "flex h-full w-full items-center justify-center bg-gradient-to-br from-zinc-950 via-slate-900 to-zinc-900 px-4 text-center text-sm text-white/75",
        className,
      )}
      role="status"
      aria-live="polite"
    >
      <p>{message}</p>
    </div>
  );
}
```
---

## Auth Modal <a name="auth-modal"></a>

A premium, animated authentication modal with social login options.

- **Registry Key**: `auth-modal`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/auth-modal.json
```

#### Dependencies
- **NPM Packages**:
  - `framer-motion`
  - `lucide-react`

#### Component Source Code
##### File: `components/ui/auth-modal.tsx`
```tsx
"use client"

import * as React from "react"
import { motion, AnimatePresence, type Variants } from "framer-motion"
import { X, Mail, ArrowRight } from "lucide-react"
import { cn } from "@/lib/utils"

// Dummy icons for Google and Microsoft as Lucide doesn't have them
const GoogleIcon = ({ className }: { className?: string }) => (
    <svg
        viewBox="0 0 24 24"
        className={className}
        fill="currentColor"
    >
        <path d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z" fill="#4285F4" />
        <path d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z" fill="#34A853" />
        <path d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z" fill="#FBBC05" />
        <path d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z" fill="#EA4335" />
    </svg>
)

const MicrosoftIcon = ({ className }: { className?: string }) => (
    <svg viewBox="0 0 88 88" className={className}>
        <path fill="#f35325" d="M0 0h42v42H0z" />
        <path fill="#81bc06" d="M46 0h42v42H46z" />
        <path fill="#05a6f0" d="M0 46h42v42H0z" />
        <path fill="#ffba08" d="M46 46h42v42H46z" />
    </svg>
)

const AppleIcon = ({ className }: { className?: string }) => (
    <svg
        viewBox="0 0 24 24"
        className={className}
        fill="currentColor"
    >
        <path d="M12.152 6.896c-.948 0-2.415-1.078-3.96-1.04-2.04.027-3.91 1.183-4.961 3.014-2.117 3.675-.546 9.103 1.519 12.09 1.013 1.454 2.208 3.09 3.792 3.039 1.52-.065 2.09-.987 3.935-.987 1.831 0 2.35.987 3.96.948 1.637-.026 2.676-1.48 3.676-2.948 1.156-1.688 1.636-3.325 1.662-3.415-.039-.013-3.182-1.221-3.22-4.857-.026-3.04 2.48-4.494 2.597-4.559-1.429-2.09-3.623-2.324-4.39-2.376-2-.156-3.675 1.09-4.61 1.09zM15.53 3.83c.843-1.012 1.4-2.427 1.245-3.83-1.207.052-2.662.805-3.532 1.818-.78.896-1.454 2.338-1.273 3.714 1.338.104 2.715-.688 3.559-1.701" />
    </svg>
)

const TwitterIcon = ({ className }: { className?: string }) => (
    <svg
        viewBox="0 0 24 24"
        className={className}
        fill="currentColor"
    >
        <path d="M23.953 4.57a10 10 0 01-2.825.775 4.958 4.958 0 002.163-2.723c-.951.555-2.005.959-3.127 1.184a4.92 4.92 0 00-8.384 4.482C7.69 8.095 4.067 6.13 1.64 3.162a4.822 4.822 0 00-.666 2.475c0 1.71.87 3.213 2.188 4.096a4.904 4.904 0 01-2.228-.616v.06a4.923 4.923 0 003.946 4.827 4.996 4.996 0 01-2.212.085 4.936 4.936 0 004.604 3.417 9.867 9.867 0 01-6.102 2.105c-.39 0-.779-.023-1.17-.067a13.995 13.995 0 007.557 2.209c9.053 0 13.998-7.496 13.998-13.985 0-.21 0-.42-.015-.63A9.935 9.935 0 0024 4.59z" fill="#1DA1F2" />
    </svg>
)

const GitHubIcon = ({ className }: { className?: string }) => (
    <svg
        viewBox="0 0 24 24"
        className={className}
        fill="currentColor"
    >
        <path d="M12 2C6.477 2 2 6.484 2 12.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0112 6.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.202 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.943.359.309.678.92.678 1.855 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019 10.019 0 0022 12.017C22 6.484 17.522 2 12 2z" />
    </svg>
)

interface AuthModalProps {
    /**
     * The text to display on the trigger button
     */
    triggerText?: string
    /**
     * Callback when login is attempted
     */
    onLogin?: (provider: string) => void
    /**
     * Optional className for the trigger button
     */
    className?: string
}

function AuthModal({
    triggerText = "Sign up / Sign in",
    onLogin,
    className
}: AuthModalProps) {
    const [isOpen, setIsOpen] = React.useState(false)
    const [email, setEmail] = React.useState("")

    const container: Variants = {
        hidden: { opacity: 0, scale: 0.95 },
        show: {
            opacity: 1,
            scale: 1,
            transition: {
                duration: 0.3,
                ease: "easeInOut",
                staggerChildren: 0.05
            }
        },
        exit: {
            opacity: 0,
            scale: 0.95,
            transition: {
                duration: 0.2,
                ease: "easeInOut"
            }
        }
    }

    const item = {
        hidden: { opacity: 0, y: 20 },
        show: { opacity: 1, y: 0 }
    }

    const socialButtons = [
        { icon: GoogleIcon, label: "Google", color: "hover:bg-zinc-100 dark:hover:bg-zinc-800" },
        { icon: AppleIcon, label: "Apple", color: "hover:bg-zinc-100 dark:hover:bg-zinc-800" },
        { icon: MicrosoftIcon, label: "Microsoft", color: "hover:bg-zinc-100 dark:hover:bg-zinc-800" },
        { icon: GitHubIcon, label: "Github", color: "hover:bg-zinc-100 dark:hover:bg-zinc-800" },
        { icon: TwitterIcon, label: "Twitter", color: "hover:bg-zinc-100 dark:hover:bg-zinc-800" },
    ]

    return (
        <>
            <button
                onClick={() => setIsOpen(true)}
                className={cn(
                    "inline-flex h-10 items-center justify-center rounded-full bg-zinc-900 px-8 text-sm font-medium text-zinc-50 transition-colors hover:bg-zinc-900/90 focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-zinc-950 disabled:pointer-events-none disabled:opacity-50 dark:bg-zinc-50 dark:text-zinc-900 dark:hover:bg-zinc-50/90 dark:focus-visible:ring-zinc-300",
                    className
                )}
            >
                {triggerText}
            </button>

            <AnimatePresence>
                {isOpen && (
                    <div className="fixed inset-0 z-50 flex items-center justify-center p-4 sm:p-0">
                        <motion.div
                            initial={{ opacity: 0 }}
                            animate={{ opacity: 1 }}
                            exit={{ opacity: 0 }}
                            onClick={() => setIsOpen(false)}
                            className="absolute inset-0 bg-zinc-950/20 backdrop-blur-sm dark:bg-zinc-950/40"
                        />

                        <motion.div
                            variants={container}
                            initial="hidden"
                            animate="show"
                            exit="exit"
                            className="relative w-full max-w-[360px] overflow-hidden rounded-3xl bg-white p-6 shadow-2xl dark:bg-zinc-950 border border-zinc-100 dark:border-zinc-900 ring-1 ring-zinc-950/5"
                        >
                            <div className="absolute right-4 top-4">
                                <button
                                    onClick={() => setIsOpen(false)}
                                    className="rounded-full p-2 text-zinc-400 hover:bg-zinc-100 hover:text-zinc-500 dark:hover:bg-zinc-900"
                                >
                                    <X className="h-4 w-4" />
                                </button>
                            </div>

                            <motion.div variants={item} className="mb-8 text-center">
                                <h2 className="text-2xl font-semibold tracking-tight text-zinc-900 dark:text-zinc-50">
                                    Welcome back
                                </h2>
                                <p className="mt-2 text-sm text-zinc-500 dark:text-zinc-400">
                                    Sign in to your account to continue
                                </p>
                            </motion.div>

                            <motion.div variants={item} className="grid grid-cols-5 gap-3 mb-8">
                                {socialButtons.map((btn, i) => (
                                    <button
                                        key={i}
                                        onClick={() => onLogin?.(btn.label)}
                                        className={cn(
                                            "flex aspect-square items-center justify-center rounded-2xl border border-zinc-200 bg-white transition-all hover:scale-105 active:scale-95 dark:border-zinc-800 dark:bg-zinc-950",
                                            btn.color
                                        )}
                                        aria-label={`Sign in with ${btn.label}`}
                                    >
                                        <btn.icon className="h-5 w-5" />
                                    </button>
                                ))}
                            </motion.div>

                            <motion.div variants={item} className="relative mb-8">
                                <div className="absolute inset-0 flex items-center">
                                    <span className="w-full border-t border-zinc-200 dark:border-zinc-800" />
                                </div>
                                <div className="relative flex justify-center text-xs uppercase">
                                    <span className="bg-white px-2 text-zinc-400 dark:bg-zinc-950">
                                        Or continue with email
                                    </span>
                                </div>
                            </motion.div>

                            <motion.div variants={item}>
                                <div className="relative">
                                    <Mail className="absolute left-3 top-1/2 -translate-y-1/2 h-4 w-4 text-zinc-400" />
                                    <input
                                        type="email"
                                        value={email}
                                        onChange={(e) => setEmail(e.target.value)}
                                        placeholder="name@example.com"
                                        className="h-10 w-full rounded-full border border-zinc-200 bg-zinc-50 pl-10 pr-10 text-sm outline-none transition-all focus:border-zinc-900 focus:bg-white focus:ring-1 focus:ring-zinc-900 dark:border-zinc-800 dark:bg-zinc-900/50 dark:focus:border-zinc-100 dark:focus:bg-zinc-900"
                                    />
                                    <button
                                        className="absolute right-1 top-1/2 -translate-y-1/2 rounded-full h-8 w-8 flex items-center justify-center bg-zinc-900 text-zinc-50 transition-transform hover:scale-95 active:scale-90 dark:bg-zinc-50 dark:text-zinc-900"
                                    >
                                        <ArrowRight className="h-4 w-4" />
                                    </button>
                                </div>
                            </motion.div>

                            <motion.div variants={item} className="mt-8 text-center">
                                <p className="text-xs text-zinc-400">
                                    By clicking continue, you agree to our{" "}
                                    <a href="#" className="underline hover:text-zinc-900 dark:hover:text-zinc-50">
                                        Terms of Service
                                    </a>{" "}
                                    and{" "}
                                    <a href="#" className="underline hover:text-zinc-900 dark:hover:text-zinc-50">
                                        Privacy Policy
                                    </a>
                                </p>
                            </motion.div>
                        </motion.div>
                    </div>
                )}
            </AnimatePresence>
        </>
    )
}

export { AuthModal, type AuthModalProps }
```
---

## Border Beam <a name="border-beam"></a>

A moving gradient beam that travels along the border of its container.

- **Registry Key**: `border-beam`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/border-beam.json
```

#### Dependencies
- **NPM Packages**:
  - `framer-motion`

#### How to Use
```tsx
"use client";

import { BorderBeam } from "@workspace/ui/components/border-beam";

export function BorderBeamDemo() {
  return (
    <>
      <div className="flex min-h-[350px] w-full items-center justify-center bg-background">
        <div className="z-10 text-center">
          <h3 className="text-2xl font-bold tracking-tight">Border Beam</h3>
          <p className="text-muted-foreground">
            The beam follows the border path
          </p>
        </div>
      </div>
      <BorderBeam />
    </>
  );
}

export function BorderBeamCustomDemo() {
  return (
    <>
      <div className="flex min-h-[300px] w-full items-center justify-center bg-background">
        <div className="z-10 text-center">
          <h3 className="text-2xl font-bold tracking-tight">Customized</h3>
          <p className="text-muted-foreground">Slower, larger, custom colors</p>
        </div>
      </div>
      <BorderBeam
        size={500}
        duration={20}
        borderWidth={2}
        colorFrom="#10b981"
        colorTo="#3b82f6"
      />
    </>
  );
}

export function BorderBeamFastDemo() {
  return (
    <>
      <div className="flex min-h-[300px] w-full items-center justify-center bg-background">
        <div className="z-10 text-center">
          <h3 className="text-2xl font-bold tracking-tight">Fast Beam</h3>
          <p className="text-muted-foreground">
            Quick animation with warm colors
          </p>
        </div>
      </div>
      <BorderBeam
        size={150}
        duration={5}
        colorFrom="#f97316"
        colorTo="#eab308"
      />
    </>
  );
}

export function BorderBeamDelayedDemo() {
  return (
    <>
      <div className="flex min-h-[300px] w-full items-center justify-center bg-background">
        <div className="z-10 text-center">
          <h3 className="text-2xl font-bold tracking-tight">Delayed Start</h3>
          <p className="text-muted-foreground">
            Beam starts after a 3 second delay
          </p>
        </div>
      </div>
      <BorderBeam
        size={250}
        duration={12}
        delay={3}
        colorFrom="#ec4899"
        colorTo="#8b5cf6"
      />
    </>
  );
}
```
#### Component Source Code
##### File: `components/ui/border-beam.tsx`
```tsx
"use client"

import { motion } from "framer-motion"
import { cn } from "@/lib/utils"

interface BorderBeamProps {
  className?: string
  size?: number
  duration?: number
  borderWidth?: number
  anchor?: number
  colorFrom?: string
  colorTo?: string
  delay?: number
}

export function BorderBeam({
  className,
  size = 200,
  duration = 15,
  anchor = 90,
  borderWidth = 1.5,
  colorFrom = "#ffaa40",
  colorTo = "#9c40ff",
  delay = 0,
}: BorderBeamProps) {
  return (
    <div
      style={
        {
          "--border-width": `${borderWidth}px`,
        } as React.CSSProperties
      }
      className={cn(
        "pointer-events-none absolute inset-0 rounded-[inherit] [border:var(--border-width)_solid_transparent]",
        "![mask-clip:padding-box,border-box] ![mask-composite:intersect] [mask:linear-gradient(transparent,transparent),linear-gradient(white,white)]",
        className
      )}
    >
      <motion.div
        style={{
          width: size,
          height: size,
          background: `linear-gradient(to left, ${colorFrom}, ${colorTo}, transparent)`,
          offsetPath: `rect(0 auto auto 0 round ${size}px)`,
          offsetAnchor: `${anchor}% 50%`,
        }}
        initial={{ offsetDistance: "0%" }}
        animate={{ offsetDistance: "100%" }}
        transition={{
          duration,
          delay,
          repeat: Infinity,
          ease: "linear",
        }}
        className="absolute aspect-square"
      />
    </div>
  )
}
```
---

## Circuit Board <a name="circuit-board"></a>

An interactive circuit board layout component with animated electricity paths that pulse between connected nodes. Fully theme-aware with automatic light/dark mode support.

- **Registry Key**: `circuit-board`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/circuit-board.json
```

#### Dependencies
- **NPM Packages**:
  - `framer-motion`

#### Component Source Code
##### File: `components/ui/circuit-board.tsx`
```tsx
"use client"

import * as React from "react"
import { motion } from "framer-motion"
import { cn } from "@workspace/ui/lib/utils"

interface CircuitNode {
  id: string
  x: number
  y: number
  label?: string
  icon?: React.ReactNode
  status?: "active" | "inactive" | "processing" | "error"
  size?: "sm" | "md" | "lg"
}

interface CircuitConnection {
  from: string
  to: string
  animated?: boolean
  bidirectional?: boolean
  color?: string
  pulseColor?: string
}

interface CircuitBoardProps extends React.HTMLAttributes<HTMLDivElement> {
  nodes: CircuitNode[]
  connections: CircuitConnection[]
  width?: number
  height?: number
  gridSize?: number
  showGrid?: boolean
  gridColor?: string
  traceColor?: string
  pulseColor?: string
  nodeColor?: string
  pulseSpeed?: number
  traceWidth?: number
  /** Force a specific theme variant. Defaults to auto-detect from system. */
  variant?: "light" | "dark" | "auto"
}

function CircuitBoard({
  nodes,
  connections,
  width = 600,
  height = 400,
  gridSize = 20,
  showGrid = true,
  gridColor,
  traceColor,
  pulseColor,
  nodeColor,
  pulseSpeed = 2,
  traceWidth = 2,
  variant = "auto",
  className,
  ...props
}: CircuitBoardProps) {
  // Theme-aware color defaults
  const [isDark, setIsDark] = React.useState(true)

  React.useEffect(() => {
    if (variant !== "auto") {
      setIsDark(variant === "dark")
      return
    }

    // Check for dark class on html/body
    const checkTheme = () => {
      const isDarkMode = document.documentElement.classList.contains("dark") ||
        document.body.classList.contains("dark")
      setIsDark(isDarkMode)
    }

    checkTheme()

    // Listen for changes
    const observer = new MutationObserver(checkTheme)
    observer.observe(document.documentElement, { attributes: true, attributeFilter: ["class"] })
    observer.observe(document.body, { attributes: true, attributeFilter: ["class"] })

    const mediaQuery = window.matchMedia("(prefers-color-scheme: dark)")
    mediaQuery.addEventListener("change", checkTheme)

    return () => {
      observer.disconnect()
      mediaQuery.removeEventListener("change", checkTheme)
    }
  }, [variant])

  // Compute theme-aware colors
  const computedGridColor = gridColor || (isDark ? "rgba(163, 163, 163, 0.08)" : "rgba(64, 64, 64, 0.12)")
  const computedTraceColor = traceColor || (isDark ? "rgba(163, 163, 163, 0.25)" : "rgba(64, 64, 64, 0.35)")
  const computedPulseColor = pulseColor || (isDark ? "rgba(163, 163, 163, 0.6)" : "rgba(64, 64, 64, 0.7)")
  const computedNodeColor = nodeColor || (isDark ? "rgba(163, 163, 163, 0.5)" : "rgba(64, 64, 64, 0.6)")
  const nodeMap = React.useMemo(() => {
    return new Map(nodes.map((node) => [node.id, node]))
  }, [nodes])

  const getNodeSize = React.useCallback((size?: CircuitNode["size"]) => {
    switch (size) {
      case "sm":
        return 24
      case "lg":
        return 48
      default:
        return 36
    }
  }, [])

  const calculatePath = React.useCallback(
    (from: CircuitNode, to: CircuitNode): string => {
      const fromSize = getNodeSize(from.size) / 2 + 4
      const toSize = getNodeSize(to.size) / 2 + 4

      const dx = to.x - from.x
      const dy = to.y - from.y

      // Calculate start and end points offset from node centers
      let startX = from.x
      let startY = from.y
      let endX = to.x
      let endY = to.y

      // Create circuit-like paths with right angles
      if (Math.abs(dx) > Math.abs(dy)) {
        // Horizontal first, then vertical
        startX = from.x + (dx > 0 ? fromSize : -fromSize)
        endX = to.x + (dx > 0 ? -toSize : toSize)
        const midX = from.x + dx / 2
        return `M ${startX} ${startY} H ${midX} V ${endY} H ${endX}`
      } else {
        // Vertical first, then horizontal
        startY = from.y + (dy > 0 ? fromSize : -fromSize)
        endY = to.y + (dy > 0 ? -toSize : toSize)
        const midY = from.y + dy / 2
        return `M ${startX} ${startY} V ${midY} H ${endX} V ${endY}`
      }
    },
    [getNodeSize]
  )

  const getStatusColor = (status?: CircuitNode["status"]) => {
    if (isDark) {
      switch (status) {
        case "active":
          return "rgba(163, 163, 163, 0.7)"
        case "processing":
          return "rgba(163, 163, 163, 0.5)"
        case "error":
          return "rgba(120, 113, 108, 0.6)"
        default:
          return computedNodeColor
      }
    } else {
      switch (status) {
        case "active":
          return "rgba(64, 64, 64, 0.8)"
        case "processing":
          return "rgba(64, 64, 64, 0.6)"
        case "error":
          return "rgba(180, 83, 83, 0.7)"
        default:
          return computedNodeColor
      }
    }
  }

  return (
    <div
      className={cn(
        "relative overflow-hidden",
        className
      )}
      style={{ width, height }}
      {...props}
    >
      <svg
        width={width}
        height={height}
        className="absolute inset-0"
        style={{ overflow: "visible" }}
      >
        <defs>
          {/* Glow filter for the pulse effect */}
          <filter id="glow" x="-50%" y="-50%" width="200%" height="200%">
            <feGaussianBlur stdDeviation="3" result="coloredBlur" />
            <feMerge>
              <feMergeNode in="coloredBlur" />
              <feMergeNode in="SourceGraphic" />
            </feMerge>
          </filter>

          {/* Grid pattern */}
          {showGrid && (
            <pattern
              id="circuitGrid"
              width={gridSize}
              height={gridSize}
              patternUnits="userSpaceOnUse"
            >
              <circle cx={gridSize / 2} cy={gridSize / 2} r="0.5" fill={computedGridColor} />
            </pattern>
          )}

          {/* Animated gradient for electricity effect */}
          {connections.map((conn, i) => (
            <linearGradient
              key={`gradient-${i}`}
              id={`electricGradient-${i}`}
              gradientUnits="userSpaceOnUse"
            >
              <stop offset="0%" stopColor="transparent" />
              <stop offset="40%" stopColor="transparent" />
              <stop offset="50%" stopColor={conn.pulseColor || computedPulseColor} />
              <stop offset="60%" stopColor="transparent" />
              <stop offset="100%" stopColor="transparent" />
            </linearGradient>
          ))}
        </defs>

        {/* Grid background */}
        {showGrid && (
          <rect width={width} height={height} fill="url(#circuitGrid)" />
        )}

        {/* Connection traces */}
        {connections.map((conn, i) => {
          const fromNode = nodeMap.get(conn.from)
          const toNode = nodeMap.get(conn.to)
          if (!fromNode || !toNode) return null

          const path = calculatePath(fromNode, toNode)
          const pathLength = 500 // Approximate path length for animation

          return (
            <g key={`connection-${i}`}>
              {/* Base trace */}
              <motion.path
                d={path}
                fill="none"
                stroke={conn.color || computedTraceColor}
                strokeWidth={traceWidth}
                strokeLinecap="round"
                strokeLinejoin="round"
                initial={{ pathLength: 0 }}
                animate={{ pathLength: 1 }}
                transition={{ duration: 1, delay: i * 0.2 }}
              />

              {/* Animated electricity pulse */}
              {conn.animated !== false && (
                <motion.path
                  d={path}
                  fill="none"
                  stroke={conn.pulseColor || computedPulseColor}
                  strokeWidth={traceWidth + 2}
                  strokeLinecap="round"
                  strokeLinejoin="round"
                  filter="url(#glow)"
                  strokeDasharray={`${pathLength * 0.1} ${pathLength * 0.9}`}
                  initial={{ strokeDashoffset: pathLength }}
                  animate={{ strokeDashoffset: -pathLength }}
                  transition={{
                    duration: pulseSpeed,
                    repeat: Infinity,
                    ease: "linear",
                    delay: i * 0.3,
                  }}
                />
              )}

              {/* Bidirectional pulse */}
              {conn.bidirectional && (
                <motion.path
                  d={path}
                  fill="none"
                  stroke={conn.pulseColor || computedPulseColor}
                  strokeWidth={traceWidth + 2}
                  strokeLinecap="round"
                  strokeLinejoin="round"
                  filter="url(#glow)"
                  strokeDasharray={`${pathLength * 0.1} ${pathLength * 0.9}`}
                  initial={{ strokeDashoffset: -pathLength }}
                  animate={{ strokeDashoffset: pathLength }}
                  transition={{
                    duration: pulseSpeed,
                    repeat: Infinity,
                    ease: "linear",
                    delay: i * 0.3 + pulseSpeed / 2,
                  }}
                />
              )}


            </g>
          )
        })}
      </svg>

      {/* Nodes */}
      {nodes.map((node, i) => {
        const size = getNodeSize(node.size)
        const statusColor = getStatusColor(node.status)

        return (
          <motion.div
            key={node.id}
            className="absolute flex items-center justify-center"
            style={{
              left: node.x - size / 2,
              top: node.y - size / 2,
              width: size,
              height: size,
            }}
            initial={{ scale: 0, opacity: 0 }}
            animate={{ scale: 1, opacity: 1 }}
            transition={{ delay: i * 0.1 + 0.5, type: "spring" }}
          >
            {/* Node background with pulse */}
            <motion.div
              className="absolute inset-0 rounded-lg"
              style={{ backgroundColor: statusColor }}
              animate={
                node.status === "processing"
                  ? { opacity: [0.2, 0.5, 0.2] }
                  : { opacity: 0.2 }
              }
              transition={
                node.status === "processing"
                  ? { duration: 1.5, repeat: Infinity }
                  : {}
              }
            />

            {/* Node border */}
            <div
              className="absolute inset-0 rounded-lg border-2"
              style={{ borderColor: statusColor }}
            />

            {/* Inner glow for active nodes */}
            {node.status === "active" && (
              <motion.div
                className="absolute inset-0 rounded-lg"
                style={{
                  boxShadow: `0 0 20px ${statusColor}40, inset 0 0 10px ${statusColor}20`,
                }}
                animate={{ opacity: [0.5, 1, 0.5] }}
                transition={{ duration: 2, repeat: Infinity }}
              />
            )}

            {/* Node content */}
            <div className="relative z-10 flex flex-col items-center justify-center">
              {node.icon && (
                <div style={{ color: statusColor }}>{node.icon}</div>
              )}
            </div>

            {/* Label */}
            {node.label && (
              <div
                className="absolute -bottom-6 left-1/2 -translate-x-1/2 whitespace-nowrap text-xs font-medium"
                style={{ color: statusColor }}
              >
                {node.label}
              </div>
            )}
          </motion.div>
        )
      })}
    </div>
  )
}

// Pre-built circuit patterns
interface CircuitPatternProps extends Omit<CircuitBoardProps, "nodes" | "connections"> {
  pattern: "data-flow" | "network" | "processor" | "tree"
}

function CircuitPattern({ pattern, ...props }: CircuitPatternProps) {
  const patterns = {
    "data-flow": {
      nodes: [
        { id: "input", x: 50, y: 200, label: "Input", status: "active" as const },
        { id: "process1", x: 200, y: 100, label: "Process", status: "processing" as const },
        { id: "process2", x: 200, y: 300, label: "Validate", status: "active" as const },
        { id: "merge", x: 400, y: 200, label: "Merge", status: "active" as const },
        { id: "output", x: 550, y: 200, label: "Output", status: "active" as const },
      ],
      connections: [
        { from: "input", to: "process1", animated: true },
        { from: "input", to: "process2", animated: true },
        { from: "process1", to: "merge", animated: true },
        { from: "process2", to: "merge", animated: true },
        { from: "merge", to: "output", animated: true },
      ],
    },
    network: {
      nodes: [
        { id: "server", x: 300, y: 80, label: "Server", status: "active" as const, size: "lg" as const },
        { id: "client1", x: 100, y: 200, label: "Client 1", status: "active" as const },
        { id: "client2", x: 300, y: 250, label: "Client 2", status: "processing" as const },
        { id: "client3", x: 500, y: 200, label: "Client 3", status: "active" as const },
        { id: "db", x: 300, y: 350, label: "Database", status: "active" as const },
      ],
      connections: [
        { from: "server", to: "client1", bidirectional: true },
        { from: "server", to: "client2", bidirectional: true },
        { from: "server", to: "client3", bidirectional: true },
        { from: "server", to: "db", bidirectional: true },
      ],
    },
    processor: {
      nodes: [
        { id: "alu", x: 300, y: 200, label: "ALU", status: "processing" as const, size: "lg" as const },
        { id: "reg1", x: 150, y: 100, label: "R1", status: "active" as const, size: "sm" as const },
        { id: "reg2", x: 150, y: 200, label: "R2", status: "active" as const, size: "sm" as const },
        { id: "reg3", x: 150, y: 300, label: "R3", status: "active" as const, size: "sm" as const },
        { id: "cache", x: 450, y: 200, label: "Cache", status: "active" as const },
        { id: "out", x: 550, y: 200, label: "Out", status: "active" as const, size: "sm" as const },
      ],
      connections: [
        { from: "reg1", to: "alu", animated: true },
        { from: "reg2", to: "alu", animated: true },
        { from: "reg3", to: "alu", animated: true },
        { from: "alu", to: "cache", animated: true },
        { from: "cache", to: "out", animated: true },
      ],
    },
    tree: {
      nodes: [
        { id: "root", x: 300, y: 50, label: "Root", status: "active" as const },
        { id: "l1", x: 150, y: 150, label: "L1", status: "active" as const },
        { id: "r1", x: 450, y: 150, label: "R1", status: "processing" as const },
        { id: "l1l", x: 80, y: 280, label: "L1L", status: "active" as const, size: "sm" as const },
        { id: "l1r", x: 220, y: 280, label: "L1R", status: "active" as const, size: "sm" as const },
        { id: "r1l", x: 380, y: 280, label: "R1L", status: "error" as const, size: "sm" as const },
        { id: "r1r", x: 520, y: 280, label: "R1R", status: "active" as const, size: "sm" as const },
      ],
      connections: [
        { from: "root", to: "l1", animated: true },
        { from: "root", to: "r1", animated: true },
        { from: "l1", to: "l1l", animated: true },
        { from: "l1", to: "l1r", animated: true },
        { from: "r1", to: "r1l", animated: true },
        { from: "r1", to: "r1r", animated: true },
      ],
    },
  }

  const selectedPattern = patterns[pattern]
  return <CircuitBoard nodes={selectedPattern.nodes} connections={selectedPattern.connections} {...props} />
}

// Interactive circuit node for building custom circuits
interface CircuitNodeComponentProps {
  status?: "active" | "inactive" | "processing" | "error"
  size?: "sm" | "md" | "lg"
  glowColor?: string
  children?: React.ReactNode
  className?: string
  onClick?: () => void
}

function CircuitNode({
  status = "inactive",
  size = "md",
  glowColor,
  children,
  className,
  onClick,
}: CircuitNodeComponentProps) {
  const [isDark, setIsDark] = React.useState(true)

  React.useEffect(() => {
    const checkTheme = () => {
      const isDarkMode = document.documentElement.classList.contains("dark") ||
        document.body.classList.contains("dark")
      setIsDark(isDarkMode)
    }

    checkTheme()

    const observer = new MutationObserver(checkTheme)
    observer.observe(document.documentElement, { attributes: true, attributeFilter: ["class"] })
    observer.observe(document.body, { attributes: true, attributeFilter: ["class"] })

    const mediaQuery = window.matchMedia("(prefers-color-scheme: dark)")
    mediaQuery.addEventListener("change", checkTheme)

    return () => {
      observer.disconnect()
      mediaQuery.removeEventListener("change", checkTheme)
    }
  }, [])

  const sizeClasses = {
    sm: "w-8 h-8",
    md: "w-12 h-12",
    lg: "w-16 h-16",
  }

  const statusColors = isDark
    ? {
      active: "rgba(163, 163, 163, 0.7)",
      inactive: "rgba(115, 115, 115, 0.4)",
      processing: "rgba(163, 163, 163, 0.5)",
      error: "rgba(120, 113, 108, 0.6)",
    }
    : {
      active: "rgba(64, 64, 64, 0.8)",
      inactive: "rgba(100, 100, 100, 0.5)",
      processing: "rgba(64, 64, 64, 0.6)",
      error: "rgba(180, 83, 83, 0.7)",
    }

  const color = glowColor || statusColors[status]

  return (
    <motion.div
      className={cn(
        "relative flex items-center justify-center rounded-lg border",
        isDark ? "bg-neutral-900/50" : "bg-neutral-200/60",
        sizeClasses[size],
        className
      )}
      style={{ borderColor: color }}
      whileHover={{ scale: 1.1 }}
      whileTap={{ scale: 0.95 }}
      onClick={onClick}
    >
      {/* Pulse animation for processing state */}
      {status === "processing" && (
        <motion.div
          className="absolute inset-0 rounded-lg"
          style={{ backgroundColor: color }}
          animate={{ opacity: [0.1, 0.3, 0.1] }}
          transition={{ duration: 1.5, repeat: Infinity }}
        />
      )}

      {/* Active glow */}
      {status === "active" && (
        <div
          className="absolute inset-0 rounded-lg"
          style={{
            boxShadow: `0 0 20px ${color}60, 0 0 40px ${color}30`,
          }}
        />
      )}

      {/* Error pulse */}
      {status === "error" && (
        <motion.div
          className="absolute inset-0 rounded-lg"
          style={{ boxShadow: `0 0 20px ${color}80` }}
          animate={{ opacity: [0.5, 1, 0.5] }}
          transition={{ duration: 0.5, repeat: Infinity }}
        />
      )}

      <div className="relative z-10" style={{ color }}>
        {children}
      </div>
    </motion.div>
  )
}

// Animated trace line component for custom layouts
interface CircuitTraceProps {
  path: string
  animated?: boolean
  color?: string
  pulseColor?: string
  width?: number
  pulseSpeed?: number
}

function CircuitTrace({
  path,
  animated = true,
  color,
  pulseColor,
  width = 2,
  pulseSpeed = 2,
}: CircuitTraceProps) {
  const [isDark, setIsDark] = React.useState(true)

  React.useEffect(() => {
    const checkTheme = () => {
      const isDarkMode = document.documentElement.classList.contains("dark") ||
        document.body.classList.contains("dark")
      setIsDark(isDarkMode)
    }

    checkTheme()

    const observer = new MutationObserver(checkTheme)
    observer.observe(document.documentElement, { attributes: true, attributeFilter: ["class"] })
    observer.observe(document.body, { attributes: true, attributeFilter: ["class"] })

    const mediaQuery = window.matchMedia("(prefers-color-scheme: dark)")
    mediaQuery.addEventListener("change", checkTheme)

    return () => {
      observer.disconnect()
      mediaQuery.removeEventListener("change", checkTheme)
    }
  }, [])

  const computedColor = color || (isDark ? "rgba(163, 163, 163, 0.25)" : "rgba(64, 64, 64, 0.35)")
  const computedPulseColor = pulseColor || (isDark ? "rgba(163, 163, 163, 0.6)" : "rgba(64, 64, 64, 0.7)")
  const pathLength = 500

  return (
    <svg className="absolute inset-0 overflow-visible pointer-events-none">
      <defs>
        <filter id="traceGlow" x="-50%" y="-50%" width="200%" height="200%">
          <feGaussianBlur stdDeviation="3" result="coloredBlur" />
          <feMerge>
            <feMergeNode in="coloredBlur" />
            <feMergeNode in="SourceGraphic" />
          </feMerge>
        </filter>
      </defs>

      {/* Base trace */}
      <motion.path
        d={path}
        fill="none"
        stroke={computedColor}
        strokeWidth={width}
        strokeLinecap="round"
        strokeLinejoin="round"
        initial={{ pathLength: 0 }}
        animate={{ pathLength: 1 }}
        transition={{ duration: 1 }}
      />

      {/* Animated pulse */}
      {animated && (
        <motion.path
          d={path}
          fill="none"
          stroke={computedPulseColor}
          strokeWidth={width + 2}
          strokeLinecap="round"
          strokeLinejoin="round"
          filter="url(#traceGlow)"
          strokeDasharray={`${pathLength * 0.1} ${pathLength * 0.9}`}
          initial={{ strokeDashoffset: pathLength }}
          animate={{ strokeDashoffset: -pathLength }}
          transition={{
            duration: pulseSpeed,
            repeat: Infinity,
            ease: "linear",
          }}
        />
      )}
    </svg>
  )
}

export {
  CircuitBoard,
  CircuitPattern,
  CircuitNode,
  CircuitTrace,
  type CircuitNode as CircuitNodeType,
  type CircuitConnection,
  type CircuitBoardProps,
}
```
---

## Closing Plasma <a name="closing-plasma"></a>

Premium plasma background with customizable motion, palette, and interaction.

- **Registry Key**: `closing-plasma`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/closing-plasma.json
```

#### Dependencies
- **NPM Packages**:
  - `clsx`
  - `tailwind-merge`

#### How to Use
```tsx
"use client";

import { useEffect, useMemo, useState } from "react";
import ClosingPlasma from "@workspace/ui/components/closing-plasma";
import { cn } from "@/lib/utils";
import {
  CLOSING_PLASMA_DEFAULT_CONFIG,
  type ClosingPlasmaConfig,
  usePlaygroundStore,
} from "@/hooks/use-playground-store";

const PRESETS: Array<{ name: string; config: ClosingPlasmaConfig }> = [
  {
    name: "Default",
    config: {
      ...CLOSING_PLASMA_DEFAULT_CONFIG,
      darkColorA: "#0d0d14",
      darkColorB: "#1f2540",
      darkColorC: "#4a6191",
    },
  },
  {
    name: "Nocturne",
    config: {
      ...CLOSING_PLASMA_DEFAULT_CONFIG,
      speed: 0.85,
      turbulence: 0.9,
      darkColorA: "#07040f",
      darkColorB: "#2a1047",
      darkColorC: "#8d4fd1",
    },
  },
  {
    name: "Pearl",
    config: {
      ...CLOSING_PLASMA_DEFAULT_CONFIG,
      speed: 0.7,
      sparkle: 0.65,
      grain: 0.8,
      darkColorA: "#071217",
      darkColorB: "#0e3f4b",
      darkColorC: "#63d8d2",
    },
  },
  {
    name: "Kinetic",
    config: {
      ...CLOSING_PLASMA_DEFAULT_CONFIG,
      speed: 1.5,
      turbulence: 1.35,
      mouseInfluence: 1.4,
      sparkle: 1.2,
      grain: 1.15,
      darkColorA: "#050708",
      darkColorB: "#143929",
      darkColorC: "#72ff88",
    },
  },
  {
    name: "Still",
    config: {
      ...CLOSING_PLASMA_DEFAULT_CONFIG,
      speed: 0.5,
      turbulence: 0.7,
      mouseInfluence: 0,
      interactive: false,
      sparkle: 0.45,
      grain: 0.75,
      darkColorA: "#111214",
      darkColorB: "#2a2d33",
      darkColorC: "#6b7280",
    },
  },
];

const SectionTitle = ({ children }: { children: React.ReactNode }) => (
  <div className="mb-3 text-[10px] font-mono uppercase tracking-widest text-muted-foreground">{children}</div>
);

const Slider = ({
  value,
  min,
  max,
  step,
  onChange,
  label,
  unit = "",
}: {
  value: number;
  min: number;
  max: number;
  step: number;
  onChange: (val: number) => void;
  label: string;
  unit?: string;
}) => (
  <div className="space-y-2">
    <div className="flex items-center justify-between text-sm">
      <span className="text-foreground/90">{label}</span>
      <span className="font-mono text-muted-foreground">
        {Number(value).toFixed(step < 0.1 ? 2 : 1)}
        {unit}
      </span>
    </div>
    <input
      type="range"
      min={min}
      max={max}
      step={step}
      value={value}
      onChange={(e) => onChange(parseFloat(e.target.value))}
      className="h-2 w-full cursor-pointer appearance-none rounded-full bg-zinc-300/70 dark:bg-zinc-700/70
      [&::-webkit-slider-thumb]:h-4 [&::-webkit-slider-thumb]:w-4 [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:rounded-full [&::-webkit-slider-thumb]:border [&::-webkit-slider-thumb]:border-border [&::-webkit-slider-thumb]:bg-white dark:[&::-webkit-slider-thumb]:bg-zinc-100"
    />
  </div>
);

const Switch = ({
  checked,
  onChange,
  label,
}: {
  checked: boolean;
  onChange: (val: boolean) => void;
  label: string;
}) => (
  <label className="flex items-center justify-between rounded-md border border-border/70 p-2.5">
    <span className="text-sm text-foreground/90">{label}</span>
    <button
      type="button"
      role="switch"
      aria-checked={checked}
      onClick={() => onChange(!checked)}
      className={cn(
        "inline-flex h-6 w-10 items-center rounded-full border border-border transition-colors",
        checked ? "bg-zinc-900 dark:bg-zinc-100" : "bg-zinc-200 dark:bg-zinc-800",
      )}
    >
      <span
        className={cn(
          "mx-[2px] h-5 w-5 rounded-full bg-white transition-transform dark:bg-zinc-900",
          checked ? "translate-x-4" : "translate-x-0.5",
        )}
      />
    </button>
  </label>
);

function normalizeHexColor(value: string) {
  const trimmed = value.trim();
  if (!/^#?[0-9a-fA-F]{6}$/.test(trimmed)) {
    return null;
  }
  return `#${trimmed.replace("#", "").toLowerCase()}`;
}

const ColorPicker = ({
  value,
  onChange,
  label,
}: {
  value: string;
  onChange: (val: string) => void;
  label: string;
}) => {
  const [draft, setDraft] = useState(value);

  useEffect(() => {
    setDraft(value);
  }, [value]);

  return (
    <label className="flex items-center gap-3 rounded-md border border-border/70 p-2.5">
      <span className="relative h-8 w-8 overflow-hidden rounded border border-border">
        <span aria-hidden="true" className="absolute inset-0" style={{ backgroundColor: value }} />
        <input
          type="color"
          value={value}
          onInput={(e) => onChange((e.target as HTMLInputElement).value)}
          onChange={(e) => onChange(e.target.value)}
          aria-label={`${label} color picker`}
          className="absolute inset-0 h-full w-full cursor-pointer opacity-0"
        />
      </span>
      <span className="min-w-0 flex-1">
        <span className="mb-1 block text-xs font-semibold text-foreground/90">{label}</span>
        <input
          value={draft}
          onChange={(e) => {
            const next = e.target.value;
            setDraft(next);
            const normalized = normalizeHexColor(next);
            if (normalized) {
              onChange(normalized);
            }
          }}
          placeholder="#000000"
          className="h-8 w-full rounded border border-border/70 bg-transparent px-2.5 font-mono text-xs text-foreground placeholder:text-muted-foreground/70 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-zinc-400/25"
        />
      </span>
    </label>
  );
};

function generateCode(config: ClosingPlasmaConfig) {
  const props = Object.entries(config)
    .filter(([, value]) => value !== undefined && value !== null)
    .map(([key, value]) => {
      if (typeof value === "string") {
        return `${key}="${value}"`;
      }
      if (typeof value === "boolean") {
        return value ? `${key}` : `${key}={false}`;
      }
      return `${key}={${value}}`;
    })
    .join("\n    ");

  return `<div className="w-full h-screen">\n  <ClosingPlasma \n    ${props}\n  />\n</div>`;
}

export function ClosingPlasmaPlayground() {
  const config = usePlaygroundStore((state) => state.closingPlasmaConfig);
  const renderVersion = usePlaygroundStore((state) => state.closingPlasmaRenderVersion);

  useEffect(() => {
    const code = generateCode(config);
    const timeoutId = setTimeout(() => {
      usePlaygroundStore.getState().setCode(code);
    }, 250);

    return () => clearTimeout(timeoutId);
  }, [config]);

  return (
    <div className="relative h-full w-full bg-[#f3f4f6] dark:bg-[#080808]">
      <div className="relative h-full w-full overflow-hidden rounded-none">
        <ClosingPlasma key={renderVersion} {...config} className="h-full w-full" />
      </div>
    </div>
  );
}

export function ClosingPlasmaPersonalizePanel() {
  const config = usePlaygroundStore((state) => state.closingPlasmaConfig);
  const activePreset = usePlaygroundStore((state) => state.activeClosingPlasmaPreset);
  const setConfig = usePlaygroundStore((state) => state.setClosingPlasmaConfig);
  const setActivePreset = usePlaygroundStore((state) => state.setActiveClosingPlasmaPreset);
  const updateConfig = usePlaygroundStore((state) => state.updateClosingPlasmaConfig);
  const resetPreview = usePlaygroundStore((state) => state.resetClosingPlasmaPreview);
  const resetConfig = usePlaygroundStore((state) => state.resetClosingPlasmaConfig);

  const selectedPresetConfig = useMemo(
    () => PRESETS.find((preset) => preset.name === activePreset)?.config,
    [activePreset],
  );

  const handleChange = (key: keyof ClosingPlasmaConfig, value: string | number | boolean) => {
    updateConfig({ [key]: value } as Partial<ClosingPlasmaConfig>);

    if (activePreset === "Custom") {
      return;
    }

    const matchedPreset = PRESETS.find(
      (preset) => JSON.stringify(preset.config) === JSON.stringify({ ...config, [key]: value }),
    );
    setActivePreset(matchedPreset?.name ?? "Custom");
  };

  const handlePresetChange = (presetName: string) => {
    const preset = PRESETS.find((item) => item.name === presetName);
    if (!preset) {
      return;
    }

    setConfig({ ...preset.config });
    setActivePreset(presetName);
    resetPreview();
  };

  return (
    <div className="h-full overflow-auto bg-[#f3f4f6] dark:bg-[#080808] [&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]">
      <div className="space-y-6 px-4 pb-10 pt-20">
        <header className="space-y-2">
          <h2 className="text-2xl font-bold tracking-tighter text-foreground">Personalize</h2>
          <p className="text-sm leading-relaxed text-muted-foreground/90">
            Shape the closing plasma mood with theme, palette, and atmospheric motion controls.
          </p>
        </header>

        <div className="flex items-center justify-between">
          <div className="text-sm text-muted-foreground/90">
            Current preset: <span className="font-mono text-foreground">{activePreset}</span>
          </div>
          <button
            type="button"
            onClick={resetConfig}
            className="rounded-md border border-border/40 bg-white/50 px-3 py-1.5 text-[10px] font-mono uppercase tracking-widest text-muted-foreground transition-colors hover:text-foreground dark:bg-white/[0.03]"
          >
            Reset
          </button>
        </div>

        <div>
          <SectionTitle>Presets</SectionTitle>
          <div className="grid grid-cols-5 gap-1.5">
            {PRESETS.map((preset) => {
              const isActive =
                activePreset === preset.name ||
                (activePreset !== "Custom" && selectedPresetConfig && preset.name === activePreset);

              return (
                <button
                  key={preset.name}
                  type="button"
                  onClick={() => handlePresetChange(preset.name)}
                  className={cn(
                    "rounded-md border p-1.5 text-center transition-colors",
                    isActive
                      ? "border-zinc-500/70 bg-zinc-100 dark:border-zinc-500/80 dark:bg-zinc-900/70"
                      : "border-border/70 bg-transparent",
                  )}
                >
                  <div
                    className="mb-1.5 h-4 rounded-sm border border-white/20"
                    style={{
                      background: `linear-gradient(125deg, ${preset.config.darkColorA} 0%, ${preset.config.darkColorB} 55%, ${preset.config.darkColorC} 100%)`,
                    }}
                  />
                  <span className="block truncate text-[10px] font-mono uppercase tracking-widest text-foreground/90">
                    {preset.name}
                  </span>
                </button>
              );
            })}
          </div>
        </div>

        <div>
          <SectionTitle>Palette</SectionTitle>
          <div className="grid grid-cols-3 gap-2.5">
            <ColorPicker label="Dark A" value={config.darkColorA} onChange={(v) => handleChange("darkColorA", v)} />
            <ColorPicker label="Dark B" value={config.darkColorB} onChange={(v) => handleChange("darkColorB", v)} />
            <ColorPicker label="Dark C" value={config.darkColorC} onChange={(v) => handleChange("darkColorC", v)} />
            <ColorPicker label="Light A" value={config.lightColorA} onChange={(v) => handleChange("lightColorA", v)} />
            <ColorPicker label="Light B" value={config.lightColorB} onChange={(v) => handleChange("lightColorB", v)} />
            <ColorPicker label="Light C" value={config.lightColorC} onChange={(v) => handleChange("lightColorC", v)} />
          </div>
        </div>

        <div>
          <SectionTitle>Motion</SectionTitle>
          <div className="grid grid-cols-2 gap-x-4 gap-y-3">
            <Slider label="Speed" min={0.2} max={2} step={0.1} value={config.speed} onChange={(v) => handleChange("speed", v)} />
            <Slider
              label="Turbulence"
              min={0.2}
              max={2}
              step={0.1}
              value={config.turbulence}
              onChange={(v) => handleChange("turbulence", v)}
            />
            <Slider
              label="Mouse Influence"
              min={0}
              max={2}
              step={0.1}
              value={config.mouseInfluence}
              onChange={(v) => handleChange("mouseInfluence", v)}
            />
            <Slider label="Sparkle" min={0} max={2} step={0.1} value={config.sparkle} onChange={(v) => handleChange("sparkle", v)} />
            <Slider label="Grain" min={0} max={2} step={0.1} value={config.grain} onChange={(v) => handleChange("grain", v)} />
            <Slider
              label="Vignette"
              min={0}
              max={2}
              step={0.1}
              value={config.vignette}
              onChange={(v) => handleChange("vignette", v)}
            />
            <Slider
              label="Opacity"
              min={0.1}
              max={1}
              step={0.05}
              value={config.opacity}
              onChange={(v) => handleChange("opacity", v)}
            />
            <div className="col-span-2">
              <Switch label="Pointer Interaction" checked={config.interactive} onChange={(v) => handleChange("interactive", v)} />
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/closing-plasma.tsx`
```tsx
"use client";

import { useEffect, useMemo, useRef } from "react";
import { cn } from "@/lib/utils";

const VERTEX_SHADER = `
attribute vec2 position;
void main() {
  gl_Position = vec4(position, 0.0, 1.0);
}
`;

const FRAGMENT_SHADER = `
precision highp float;

uniform vec2 u_res;
uniform float u_time;
uniform vec2 u_mouse;
uniform float u_isDark;
uniform float u_speed;
uniform float u_turbulence;
uniform float u_mouseInfluence;
uniform float u_grain;
uniform float u_sparkle;
uniform float u_vignette;
uniform float u_opacity;

uniform vec3 u_darkA;
uniform vec3 u_darkB;
uniform vec3 u_darkC;
uniform vec3 u_lightA;
uniform vec3 u_lightB;
uniform vec3 u_lightC;

vec3 mod289(vec3 x) { return x - floor(x * (1.0 / 289.0)) * 289.0; }
vec2 mod289(vec2 x) { return x - floor(x * (1.0 / 289.0)) * 289.0; }
vec3 permute(vec3 x) { return mod289(((x * 34.0) + 1.0) * x); }

float snoise(vec2 v) {
  const vec4 C = vec4(0.211324865405187, 0.366025403784439,
                     -0.577350269189626, 0.024390243902439);
  vec2 i = floor(v + dot(v, C.yy));
  vec2 x0 = v - i + dot(i, C.xx);
  vec2 i1 = (x0.x > x0.y) ? vec2(1.0, 0.0) : vec2(0.0, 1.0);
  vec4 x12 = x0.xyxy + C.xxzz;
  x12.xy -= i1;
  i = mod289(i);
  vec3 p = permute(permute(i.y + vec3(0.0, i1.y, 1.0)) + i.x + vec3(0.0, i1.x, 1.0));
  vec3 m = max(0.5 - vec3(dot(x0, x0), dot(x12.xy, x12.xy), dot(x12.zw, x12.zw)), 0.0);
  m = m * m;
  m = m * m;
  vec3 x = 2.0 * fract(p * C.www) - 1.0;
  vec3 h = abs(x) - 0.5;
  vec3 ox = floor(x + 0.5);
  vec3 a0 = x - ox;
  m *= 1.79284291400159 - 0.85373472095314 * (a0 * a0 + h * h);
  vec3 g;
  g.x = a0.x * x0.x + h.x * x0.y;
  g.yz = a0.yz * x12.xz + h.yz * x12.yw;
  return 130.0 * dot(m, g);
}

float fbm(vec2 p, float turbulence) {
  float total = 0.0;
  float amp = 0.5;
  float freq = 1.0;
  mat2 rot = mat2(cos(0.45), sin(0.45), -sin(0.45), cos(0.45));
  for (int i = 0; i < 5; i++) {
    total += snoise(p * freq) * amp;
    p = rot * p;
    freq *= mix(1.85, 2.35, clamp(turbulence, 0.0, 2.0) * 0.5);
    amp *= 0.5;
  }
  return total;
}

void main() {
  vec2 uv = gl_FragCoord.xy / u_res;
  float aspect = u_res.x / max(u_res.y, 1.0);
  vec2 p = (uv - 0.5) * vec2(aspect, 1.0);
  float t = u_time * (0.15 * u_speed);

  vec2 mouse = (u_mouse - 0.5) * vec2(aspect, 1.0);
  float dMouse = length(p - mouse);
  p += (mouse - p) * 0.02 * u_mouseInfluence * smoothstep(0.45, 0.0, dMouse);

  vec2 flow = vec2(
    fbm(p + vec2(t * 0.2, t * 0.1), u_turbulence),
    fbm(p + vec2(-t * 0.1, t * 0.3), u_turbulence)
  );

  float n = fbm(p * 2.0 + flow * 1.45, u_turbulence);
  float ridges = 1.0 - abs(snoise(p * 4.0 + n) * 2.0);
  ridges = pow(ridges, 3.0);

  vec3 colorA = mix(u_lightA, u_darkA, u_isDark);
  vec3 colorB = mix(u_lightB, u_darkB, u_isDark);
  vec3 colorC = mix(u_lightC, u_darkC, u_isDark);

  vec3 col = mix(colorA, colorB, smoothstep(-0.5, 0.5, n));
  col = mix(col, colorC, smoothstep(0.25, 1.0, n * 0.52 + ridges * 0.48));

  float sparkle = pow(max(0.0, snoise(gl_FragCoord.xy * 0.2 + t * 2.0)), 18.0) * 0.5 * u_sparkle;
  vec3 sparkleColor = mix(vec3(0.56, 0.58, 0.72), vec3(0.8, 0.9, 1.0), u_isDark);
  col += sparkleColor * sparkle;

  float vigDark = 1.0 - smoothstep(0.5, mix(1.8, 1.55, u_isDark), length(p));
  col = mix(col, col * vigDark, u_isDark * u_vignette);
  float vigLight = 1.0 - smoothstep(0.4, 1.45, length(p));
  col = mix(mix(vec3(1.0), col, vigLight), col, u_isDark);

  float grain = (fract(sin(dot(gl_FragCoord.xy + t * 50.0, vec2(12.9898, 78.233))) * 43758.5453) - 0.5) * (0.06 * u_grain);
  col += grain;

  gl_FragColor = vec4(clamp(col, 0.0, 1.0), u_opacity);
}
`;

const HEX_COLOR_REGEX = /^#?[0-9a-fA-F]{6}$/;

function sanitizeHexColor(value: string, fallback: string) {
  const trimmed = value.trim();
  if (!HEX_COLOR_REGEX.test(trimmed)) {
    return fallback;
  }
  return trimmed.startsWith("#") ? trimmed : `#${trimmed}`;
}

function hexToRgb01(hex: string, fallback: string): [number, number, number] {
  const normalized = sanitizeHexColor(hex, fallback).replace("#", "");
  const r = parseInt(normalized.slice(0, 2), 16) / 255;
  const g = parseInt(normalized.slice(2, 4), 16) / 255;
  const b = parseInt(normalized.slice(4, 6), 16) / 255;
  return [r, g, b];
}

const DARK_A = "#0d0d14";
const DARK_B = "#1f2540";
const DARK_C = "#4a6191";
const LIGHT_A = "#f0f2f7";
const LIGHT_B = "#d7dceb";
const LIGHT_C = "#bcc5e0";

export interface ClosingPlasmaProps extends React.HTMLAttributes<HTMLDivElement> {
  themeMode?: "auto" | "light" | "dark";
  speed?: number;
  turbulence?: number;
  mouseInfluence?: number;
  grain?: number;
  sparkle?: number;
  vignette?: number;
  opacity?: number;
  interactive?: boolean;
  darkColorA?: string;
  darkColorB?: string;
  darkColorC?: string;
  lightColorA?: string;
  lightColorB?: string;
  lightColorC?: string;
  children?: React.ReactNode;
}

export function ClosingPlasma({
  themeMode = "auto",
  speed = 1,
  turbulence = 1,
  mouseInfluence = 1,
  grain = 1,
  sparkle = 1,
  vignette = 1,
  opacity = 1,
  interactive = true,
  darkColorA = DARK_A,
  darkColorB = DARK_B,
  darkColorC = DARK_C,
  lightColorA = LIGHT_A,
  lightColorB = LIGHT_B,
  lightColorC = LIGHT_C,
  className,
  children,
  style,
  ...props
}: ClosingPlasmaProps) {
  const containerRef = useRef<HTMLDivElement | null>(null);
  const canvasRef = useRef<HTMLCanvasElement | null>(null);
  const mouseRef = useRef({ x: 0.5, y: 0.5 });
  const targetMouseRef = useRef({ x: 0.5, y: 0.5 });
  const isDarkRef = useRef(0);

  const settings = useMemo(
    () => ({
      speed,
      turbulence,
      mouseInfluence,
      grain,
      sparkle,
      vignette,
      opacity,
      interactive,
      darkColorA,
      darkColorB,
      darkColorC,
      lightColorA,
      lightColorB,
      lightColorC,
    }),
    [
      speed,
      turbulence,
      mouseInfluence,
      grain,
      sparkle,
      vignette,
      opacity,
      interactive,
      darkColorA,
      darkColorB,
      darkColorC,
      lightColorA,
      lightColorB,
      lightColorC,
    ],
  );

  useEffect(() => {
    const computeTheme = () => {
      if (themeMode === "dark") {
        isDarkRef.current = 1;
        return;
      }
      if (themeMode === "light") {
        isDarkRef.current = 0;
        return;
      }
      isDarkRef.current = document.documentElement.classList.contains("dark") ? 1 : 0;
    };

    computeTheme();

    if (themeMode !== "auto") {
      return;
    }

    const observer = new MutationObserver(computeTheme);
    observer.observe(document.documentElement, {
      attributes: true,
      attributeFilter: ["class"],
    });

    return () => observer.disconnect();
  }, [themeMode]);

  useEffect(() => {
    const container = containerRef.current;
    const canvas = canvasRef.current;
    if (!container || !canvas) {
      return;
    }

    const handlePointerMove = (event: PointerEvent) => {
      if (!settings.interactive) {
        return;
      }
      const rect = container.getBoundingClientRect();
      targetMouseRef.current = {
        x: (event.clientX - rect.left) / rect.width,
        y: 1 - (event.clientY - rect.top) / rect.height,
      };
    };

    const handlePointerLeave = () => {
      targetMouseRef.current = { x: 0.5, y: 0.5 };
    };

    container.addEventListener("pointermove", handlePointerMove);
    container.addEventListener("pointerleave", handlePointerLeave);

    const gl = canvas.getContext("webgl", { antialias: false, alpha: true });
    if (!gl) {
      return () => {
        container.removeEventListener("pointermove", handlePointerMove);
        container.removeEventListener("pointerleave", handlePointerLeave);
      };
    }

    const compileShader = (type: number, source: string) => {
      const shader = gl.createShader(type);
      if (!shader) {
        return null;
      }
      gl.shaderSource(shader, source);
      gl.compileShader(shader);
      if (!gl.getShaderParameter(shader, gl.COMPILE_STATUS)) {
        gl.deleteShader(shader);
        return null;
      }
      return shader;
    };

    const vertexShader = compileShader(gl.VERTEX_SHADER, VERTEX_SHADER);
    const fragmentShader = compileShader(gl.FRAGMENT_SHADER, FRAGMENT_SHADER);
    if (!vertexShader || !fragmentShader) {
      return;
    }

    const program = gl.createProgram();
    if (!program) {
      gl.deleteShader(vertexShader);
      gl.deleteShader(fragmentShader);
      return;
    }

    gl.attachShader(program, vertexShader);
    gl.attachShader(program, fragmentShader);
    gl.linkProgram(program);
    if (!gl.getProgramParameter(program, gl.LINK_STATUS)) {
      gl.deleteProgram(program);
      gl.deleteShader(vertexShader);
      gl.deleteShader(fragmentShader);
      return;
    }

    gl.useProgram(program);

    const position = gl.getAttribLocation(program, "position");
    const buffer = gl.createBuffer();
    gl.bindBuffer(gl.ARRAY_BUFFER, buffer);
    gl.bufferData(
      gl.ARRAY_BUFFER,
      new Float32Array([-1, -1, 1, -1, -1, 1, 1, 1]),
      gl.STATIC_DRAW,
    );
    gl.enableVertexAttribArray(position);
    gl.vertexAttribPointer(position, 2, gl.FLOAT, false, 0, 0);

    const uRes = gl.getUniformLocation(program, "u_res");
    const uTime = gl.getUniformLocation(program, "u_time");
    const uMouse = gl.getUniformLocation(program, "u_mouse");
    const uIsDark = gl.getUniformLocation(program, "u_isDark");
    const uSpeed = gl.getUniformLocation(program, "u_speed");
    const uTurbulence = gl.getUniformLocation(program, "u_turbulence");
    const uMouseInfluence = gl.getUniformLocation(program, "u_mouseInfluence");
    const uGrain = gl.getUniformLocation(program, "u_grain");
    const uSparkle = gl.getUniformLocation(program, "u_sparkle");
    const uVignette = gl.getUniformLocation(program, "u_vignette");
    const uOpacity = gl.getUniformLocation(program, "u_opacity");
    const uDarkA = gl.getUniformLocation(program, "u_darkA");
    const uDarkB = gl.getUniformLocation(program, "u_darkB");
    const uDarkC = gl.getUniformLocation(program, "u_darkC");
    const uLightA = gl.getUniformLocation(program, "u_lightA");
    const uLightB = gl.getUniformLocation(program, "u_lightB");
    const uLightC = gl.getUniformLocation(program, "u_lightC");

    if (
      !uRes ||
      !uTime ||
      !uMouse ||
      !uIsDark ||
      !uSpeed ||
      !uTurbulence ||
      !uMouseInfluence ||
      !uGrain ||
      !uSparkle ||
      !uVignette ||
      !uOpacity ||
      !uDarkA ||
      !uDarkB ||
      !uDarkC ||
      !uLightA ||
      !uLightB ||
      !uLightC
    ) {
      gl.deleteBuffer(buffer);
      gl.deleteProgram(program);
      gl.deleteShader(vertexShader);
      gl.deleteShader(fragmentShader);
      return;
    }

    const resize = () => {
      const dpr = Math.min(window.devicePixelRatio || 1, 1.75);
      const { width, height } = container.getBoundingClientRect();
      canvas.width = Math.max(1, Math.floor(width * dpr));
      canvas.height = Math.max(1, Math.floor(height * dpr));
      gl.viewport(0, 0, canvas.width, canvas.height);
      gl.uniform2f(uRes, canvas.width, canvas.height);
    };

    resize();
    const resizeObserver = new ResizeObserver(resize);
    resizeObserver.observe(container);

    const darkA = hexToRgb01(settings.darkColorA, DARK_A);
    const darkB = hexToRgb01(settings.darkColorB, DARK_B);
    const darkC = hexToRgb01(settings.darkColorC, DARK_C);
    const lightA = hexToRgb01(settings.lightColorA, LIGHT_A);
    const lightB = hexToRgb01(settings.lightColorB, LIGHT_B);
    const lightC = hexToRgb01(settings.lightColorC, LIGHT_C);

    gl.uniform3f(uDarkA, darkA[0], darkA[1], darkA[2]);
    gl.uniform3f(uDarkB, darkB[0], darkB[1], darkB[2]);
    gl.uniform3f(uDarkC, darkC[0], darkC[1], darkC[2]);
    gl.uniform3f(uLightA, lightA[0], lightA[1], lightA[2]);
    gl.uniform3f(uLightB, lightB[0], lightB[1], lightB[2]);
    gl.uniform3f(uLightC, lightC[0], lightC[1], lightC[2]);

    let rafId = 0;
    const start = performance.now();

    const render = (now: number) => {
      const elapsed = (now - start) / 1000;
      mouseRef.current.x += (targetMouseRef.current.x - mouseRef.current.x) * 0.05;
      mouseRef.current.y += (targetMouseRef.current.y - mouseRef.current.y) * 0.05;

      gl.uniform1f(uTime, elapsed);
      gl.uniform2f(uMouse, mouseRef.current.x, mouseRef.current.y);
      gl.uniform1f(uIsDark, isDarkRef.current);
      gl.uniform1f(uSpeed, settings.speed);
      gl.uniform1f(uTurbulence, settings.turbulence);
      gl.uniform1f(uMouseInfluence, settings.mouseInfluence);
      gl.uniform1f(uGrain, settings.grain);
      gl.uniform1f(uSparkle, settings.sparkle);
      gl.uniform1f(uVignette, settings.vignette);
      gl.uniform1f(uOpacity, settings.opacity);
      gl.drawArrays(gl.TRIANGLE_STRIP, 0, 4);
      rafId = requestAnimationFrame(render);
    };

    rafId = requestAnimationFrame(render);

    return () => {
      container.removeEventListener("pointermove", handlePointerMove);
      container.removeEventListener("pointerleave", handlePointerLeave);
      cancelAnimationFrame(rafId);
      resizeObserver.disconnect();
      gl.deleteBuffer(buffer);
      gl.deleteProgram(program);
      gl.deleteShader(vertexShader);
      gl.deleteShader(fragmentShader);
    };
  }, [settings]);

  return (
    <div
      ref={containerRef}
      className={cn("relative h-full w-full overflow-hidden", className)}
      style={style}
      {...props}
    >
      <canvas
        ref={canvasRef}
        aria-hidden="true"
        className="absolute inset-0 h-full w-full"
        style={{ width: "100%", height: "100%", display: "block" }}
      />
      {children && <div className="relative z-10 h-full w-full">{children}</div>}
    </div>
  );
}

export default ClosingPlasma;
```
---

## Collection Surfer <a name="collection-surfer"></a>

- **Registry Key**: `collection-surfer`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/collection-surfer.json
```

#### How to Use
```tsx
"use client";

import * as React from "react";
import { cn } from "@/lib/utils";

interface CollectionSurferPreviewProps {
  src: string;
  title: string;
  className?: string;
}

export function CollectionSurferPreview({
  src,
  title,
  className,
}: CollectionSurferPreviewProps) {
  return (
    <div className={cn("relative w-full h-full overflow-hidden", className)}>
      <iframe src={src} className="w-full h-full border-0" title={title} />
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/collection-surfer.tsx`
```tsx
"use client";

import React, { useRef } from "react";
import { motion, useScroll, useTransform, useSpring, useMotionValue, MotionValue } from "framer-motion";

export interface CollectionItem {
    id: number;
    image: string;
    title: string;
}

export type CollectionSurferVariant = "magnetic" | "uplift" | "simple";

// Default items for the component in case none are provided
const ITEMS: CollectionItem[] = [
    { id: 1, image: "https://images.unsplash.com/photo-1515886657613-9f3515b0c78f?w=800&q=80", title: "HERITAGE 01" },
    { id: 2, image: "https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=800&q=80", title: "HERITAGE 02" },
    { id: 3, image: "https://images.unsplash.com/photo-1483985988355-763728e1935b?w=800&q=80", title: "HERITAGE 03" },
    { id: 4, image: "https://images.unsplash.com/photo-1500917293891-ef795e70e1f6?w=800&q=80", title: "HERITAGE 04" },
    { id: 5, image: "https://images.unsplash.com/photo-1496747611176-843222e1e57c?w=800&q=80", title: "HERITAGE 05" },
    { id: 6, image: "https://images.unsplash.com/photo-1532453288672-3a27e9be9efd?w=800&q=80", title: "HERITAGE 06" },
    { id: 7, image: "https://images.unsplash.com/photo-1552374196-1ab2a1c593e8?w=800&q=80", title: "HERITAGE 07" },
    { id: 8, image: "https://images.unsplash.com/photo-1509631179647-0177331693ae?w=800&q=80", title: "HERITAGE 08" },
    { id: 9, image: "https://images.unsplash.com/photo-1502716119720-b23a93e5fe1b?w=800&q=80", title: "HERITAGE 09" },
    { id: 10, image: "https://images.unsplash.com/photo-1539008835657-9e8e9680c956?w=800&q=80", title: "HERITAGE 10" },
    { id: 11, image: "https://images.unsplash.com/photo-1469334031218-e382a71b716b?w=800&q=80", title: "HERITAGE 11" },
    { id: 12, image: "https://images.unsplash.com/photo-1531746020798-e6953c6e8e04?w=800&q=80", title: "HERITAGE 12" },
    { id: 13, image: "https://images.unsplash.com/photo-1581044777550-4cfa60707c03?w=800&q=80", title: "HERITAGE 13" },
    { id: 14, image: "https://images.unsplash.com/photo-1506794778202-cad84cf45f1d?w=800&q=80", title: "HERITAGE 14" },
    { id: 15, image: "https://images.unsplash.com/photo-1496217590455-aa63a8350eea?w=800&q=80", title: "HERITAGE 15" },
    { id: 16, image: "https://images.unsplash.com/photo-1571513722275-4b41940f54b8?w=800&q=80", title: "HERITAGE 16" },
];

interface CollectionSurferProps {
    items?: CollectionItem[];
    variant?: CollectionSurferVariant;
}

export function CollectionSurfer({ items = ITEMS, variant = "magnetic" }: CollectionSurferProps) {
    // 1. Loop Setup: Duplicate items to create a buffer
    // We render the list twice: [Original Set, Duplicate Set]
    // When we scroll past the Original Set, we snap back to the start.
    const duplicatedItems = [...items, ...items];

    // Scroll sensitivity
    const scrollPerItem = 600;

    // The exact scroll distance to complete one full loop of the ORIGINAL items
    const loopDistance = items.length * scrollPerItem;

    const { scrollY } = useScroll();

    const smoothScroll = useSpring(scrollY, {
        mass: 0.1,
        stiffness: 100,
        damping: 20
    });

    // 2. Modulo Logic:
    // Instead of mapping 0 -> totalScroll, we map to a looped value.
    // loops 0 -> loopDistance -> 0 -> loopDistance...
    const loopedProgress = useTransform(smoothScroll, (value) => value % loopDistance);

    // Step vector
    const stepX = 240;
    const stepY = -84;
    const stepZ = -288;

    // We only move the scene backwards by the length of ONE set of items
    const x = useTransform(loopedProgress, [0, loopDistance], [0, -items.length * stepX]);
    const y = useTransform(loopedProgress, [0, loopDistance], [0, -items.length * stepY]);
    const z = useTransform(loopedProgress, [0, loopDistance], [0, -items.length * stepZ]);

    // Mouse position for magnetic effect
    // Initialize off-screen so no card is scaled by default
    const mouseX = useMotionValue(-10000);
    const mouseY = useMotionValue(-10000);

    const handleMouseMove = (e: React.MouseEvent) => {
        if (variant === "simple") return;
        mouseX.set(e.clientX);
        mouseY.set(e.clientY);
    };

    const handleMouseLeave = () => {
        if (variant === "simple") return;
        mouseX.set(-10000);
        mouseY.set(-10000);
    };

    return (
        <div className="relative bg-black min-h-screen text-white w-full">
            {/* 3. Infinite Spacer: 
                We make this huge so the user can scroll for a very long time.
                (e.g., 50,000px) */}
            <div style={{ height: "50000px" }} className="w-full" />

            {/* Fixed viewport */}
            <div
                className="fixed inset-0 w-full h-screen overflow-hidden flex items-center justify-center perspective-container"
                onMouseMove={handleMouseMove}
                onMouseLeave={handleMouseLeave}
            >

                {/* UI Overlays */}
                <div className="absolute top-[3vw] left-[3vw] z-50 pointer-events-none mix-blend-difference">
                    <h1 className="font-heading font-bold text-[clamp(2rem,6vw,5rem)] leading-[0.9] tracking-tighter ml-[4vw]">
                        HERITAGE FW25/26
                    </h1>
                    <h1 className="font-heading font-bold text-[clamp(2rem,6vw,5rem)] leading-[0.9] tracking-tighter">
                        COLLECTION
                        <span className="text-[0.4em] align-top relative top-[0.6em] ml-2 font-mono tabular-nums">
                            ({items.length})
                        </span>
                    </h1>
                </div>

                <div className="absolute bottom-[3vw] right-[3vw] z-50 font-mono text-xs tracking-wider uppercase opacity-70">
                    scroll to surf
                </div>

                {/* 3D Scene */}
                <div
                    className="absolute inset-0 flex items-center justify-center"
                    style={{
                        perspective: "2000px",
                        perspectiveOrigin: "10% 10%",
                    }}
                >
                    {/* Animated Track */}
                    <motion.div
                        className="relative w-0 h-0"
                        style={{
                            x,
                            y,
                            z,
                            transformStyle: "preserve-3d",
                        }}
                    >
                        {duplicatedItems.map((item, i) => (
                            <Card
                                key={`${item.id}-${i}`}
                                item={item}
                                i={i}
                                stepX={stepX}
                                stepY={stepY}
                                stepZ={stepZ}
                                mouseX={mouseX}
                                mouseY={mouseY}
                                scrollSpring={smoothScroll}
                                variant={variant}
                            />
                        ))}
                    </motion.div>
                </div>
            </div>
        </div>
    );
}

function Card({
    item,
    i,
    stepX,
    stepY,
    stepZ,
    mouseX,
    mouseY,
    scrollSpring,
    variant
}: {
    item: CollectionItem,
    i: number,
    stepX: number,
    stepY: number,
    stepZ: number,
    mouseX: MotionValue<number>,
    mouseY: MotionValue<number>,
    scrollSpring: MotionValue<number>,
    variant: CollectionSurferVariant
}) {
    const ref = useRef<HTMLDivElement>(null);

    // Calculate distance from mouse to center of card
    const distance = useTransform([mouseX, mouseY, scrollSpring], ([x, y]) => {
        if (!ref.current || variant === "simple") return 200; // Default large distance
        const rect = ref.current.getBoundingClientRect();
        const centerX = rect.left + rect.width / 2;
        const centerY = rect.top + rect.height / 2;
        const dist = Math.sqrt(Math.pow(x - centerX, 2) + Math.pow(y - centerY, 2));
        return dist;
    });

    // --- Magnetic Variant ---
    // Map distance to scale: Closer = larger
    const targetScale = useTransform(distance, [0, 400], [1.5, 1]);
    const springScale = useSpring(targetScale, {
        mass: 0.5,
        stiffness: 300,
        damping: 20
    });

    // --- Uplift Variant ---
    // Map distance to Y uplift: Closer = move up (negative Y)
    const targetUplift = useTransform(distance, [0, 400], [-100, 0]);
    const springUplift = useSpring(targetUplift, {
        mass: 0.5,
        stiffness: 300,
        damping: 20
    });

    // Combine transforms based on variant
    const transform = useTransform(
        [springScale, springUplift],
        ([s, u]) => {
            let scaleValue = 1;
            let upliftValue = 0;

            if (variant === "magnetic") {
                scaleValue = Number(s);
            } else if (variant === "uplift") {
                upliftValue = Number(u);
            }

            const baseX = i * stepX;
            const baseY = i * stepY;
            const baseZ = i * stepZ;

            return `translate3d(${baseX}px, ${baseY + upliftValue}px, ${baseZ}px) rotateY(-50deg) scale(${scaleValue})`;
        }
    );

    return (
        <motion.div
            ref={ref}
            className="absolute w-[300px] h-[400px] bg-neutral-900 overflow-hidden shadow-2xl transition-colors duration-500 ease-out group"
            style={{
                transform,
                transformStyle: "preserve-3d",
            }}
        >
            {/* Index number: Using i % 16 + 1 so the duplicate cards show correct numbers (01-16) */}
            <div className="absolute -top-6 -left-4 text-white font-mono text-xs opacity-50 transition-opacity group-hover:opacity-100">
                {String((i % 16) + 1).padStart(2, '0')}
            </div>

            {/* Image */}
            <div className="relative w-full h-full brightness-75 group-hover:brightness-100 transition-all duration-300">
                <img
                    src={item.image}
                    alt={item.title}
                    className="w-full h-full object-cover"
                />
            </div>

            <div className="absolute inset-0 bg-gradient-to-tr from-black/20 to-transparent pointer-events-none" />
        </motion.div>
    );
}
```
---

## Command Menu <a name="command-menu"></a>

A macOS Spotlight-style command menu with animated search, keyboard navigation, and customizable groups. Features backdrop blur, smooth animations, and full keyboard support.

- **Registry Key**: `command-menu`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/command-menu.json
```

#### Dependencies
- **NPM Packages**:
  - `cmdk`
  - `framer-motion`
  - `lucide-react`

#### How to Use
```tsx
"use client"

import { CommandMenu } from "@workspace/ui/components/command-menu"
import { FileText, Settings, User, Hash, Home, BookOpen } from "lucide-react"

const demoGroups = [
    {
        title: "Pages",
        items: [
            { id: "home", title: "Home", icon: <Home className="h-4 w-4" />, onSelect: () => console.log("Home") },
            { id: "about", title: "About", icon: <FileText className="h-4 w-4" />, onSelect: () => console.log("About") },
            { id: "docs", title: "Documentation", icon: <BookOpen className="h-4 w-4" />, onSelect: () => console.log("Docs") },
        ],
    },
    {
        title: "Settings",
        items: [
            { id: "profile", title: "Profile", icon: <User className="h-4 w-4" />, onSelect: () => console.log("Profile") },
            { id: "settings", title: "Settings", icon: <Settings className="h-4 w-4" />, onSelect: () => console.log("Settings") },
        ],
    },
]

export function CommandMenuDemo() {
    return (
        <div className="w-full h-full flex items-center justify-center p-8">
            <CommandMenu
                groups={demoGroups}
                placeholder="Search..."
                brandName="Demo App"
            />
        </div>
    )
}

const customGroups = [
    {
        title: "Documentation",
        items: [
            { id: "intro", title: "Introduction", icon: <FileText className="h-4 w-4" />, onSelect: () => console.log("Intro") },
            { id: "api", title: "API Reference", icon: <Hash className="h-4 w-4" />, onSelect: () => console.log("API") },
            { id: "examples", title: "Examples", icon: <BookOpen className="h-4 w-4" />, onSelect: () => console.log("Examples") },
        ],
    },
]

export function CommandMenuCustomDemo() {
    return (
        <div className="w-full h-full flex items-center justify-center p-8">
            <CommandMenu
                groups={customGroups}
                placeholder="Search documentation..."
                triggerLabel="Docs..."
                shortcutKey="J"
                brandName="Docs"
            />
        </div>
    )
}
```
#### Component Source Code
##### File: `components/ui/command-menu.tsx`
```tsx
"use client"

import * as React from "react"
import * as ReactDOM from "react-dom"
import { Command } from "cmdk"
import { Search, ArrowRight, X } from "lucide-react"
import { motion, AnimatePresence } from "framer-motion"

import { cn } from "@/lib/utils"

export interface CommandMenuItem {
  id: string
  title: string
  group?: string
  icon?: React.ReactNode
  onSelect?: () => void
}

export interface CommandMenuGroup {
  title: string
  items: CommandMenuItem[]
}

export interface CommandMenuProps {
  groups: CommandMenuGroup[]
  placeholder?: string
  emptyMessage?: string
  brandName?: string
  triggerClassName?: string
  triggerLabel?: string
  shortcutKey?: string
  open?: boolean
  onOpenChange?: (open: boolean) => void
}

function CommandMenu({
  groups,
  placeholder = "Search...",
  emptyMessage = "No results found",
  brandName = "Command Menu",
  triggerClassName,
  triggerLabel = "Search...",
  shortcutKey = "K",
  open: controlledOpen,
  onOpenChange,
}: CommandMenuProps) {
  const [internalOpen, setInternalOpen] = React.useState(false)
  const [query, setQuery] = React.useState("")
  const inputRef = React.useRef<HTMLInputElement>(null)

  const isControlled = controlledOpen !== undefined
  const open = isControlled ? controlledOpen : internalOpen
  const setOpen = isControlled ? (onOpenChange ?? (() => {})) : setInternalOpen

  React.useEffect(() => {
    const down = (e: KeyboardEvent) => {
      if (e.key.toLowerCase() === shortcutKey.toLowerCase() && (e.metaKey || e.ctrlKey)) {
        e.preventDefault()
        setOpen(!open)
      }
      if (e.key === "Escape") {
        setOpen(false)
      }
    }

    document.addEventListener("keydown", down)
    return () => document.removeEventListener("keydown", down)
  }, [open, setOpen, shortcutKey])

  React.useEffect(() => {
    if (open) {
      setTimeout(() => {
        inputRef.current?.focus()
      }, 0)
    } else {
      setQuery("")
    }
  }, [open])

  const handleSelect = React.useCallback((item: CommandMenuItem) => {
    setOpen(false)
    item.onSelect?.()
  }, [setOpen])

  return (
    <>
      <button
        onClick={() => setOpen(true)}
        className={cn(
          "group inline-flex items-center gap-2 whitespace-nowrap transition-all duration-200",
          "focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring/50",
          "disabled:pointer-events-none disabled:opacity-50",
          "border border-input/50 hover:border-input hover:bg-accent/50",
          "px-3 py-2 relative h-9 w-full justify-start rounded-lg bg-muted/30",
          "text-sm font-normal text-muted-foreground sm:pr-12 md:w-40 lg:w-56",
          triggerClassName
        )}
      >
        <Search className="h-4 w-4 opacity-50 group-hover:opacity-70 transition-opacity" />
        <span className="hidden lg:inline-flex">{triggerLabel}</span>
        <span className="inline-flex lg:hidden">Search</span>
        <kbd className="pointer-events-none absolute right-1.5 top-1.5 hidden h-6 select-none items-center gap-0.5 rounded-md border bg-background/80 px-1.5 font-mono text-[10px] font-medium text-muted-foreground/70 sm:flex">
          <span className="text-xs">⌘</span>{shortcutKey}
        </kbd>
      </button>

      {typeof document !== "undefined" && ReactDOM.createPortal(
        <AnimatePresence>
          {open && (
            <>
              <motion.div
                initial={{ opacity: 0 }}
                animate={{ opacity: 1 }}
                exit={{ opacity: 0 }}
                transition={{ duration: 0.15 }}
                className="fixed inset-0 z-40 bg-black/50 backdrop-blur-sm"
                onClick={() => setOpen(false)}
              />
              <motion.div
                initial={{ opacity: 0, scale: 0.96 }}
                animate={{ opacity: 1, scale: 1 }}
                exit={{ opacity: 0, scale: 0.96 }}
                transition={{ 
                  duration: 0.2,
                  ease: [0.32, 0.72, 0, 1]
                }}
                className="fixed left-1/2 top-1/2 z-50 w-full max-w-[680px] -translate-x-1/2 -translate-y-1/2 p-4"
              >
                <Command
                  label="Command Menu"
                  className="overflow-hidden rounded-2xl border border-border/50 bg-popover/95 backdrop-blur-xl shadow-2xl shadow-black/20"
                  shouldFilter={true}
                >
                  <div className="flex items-center gap-3 border-b border-border/50 px-4 py-3">
                    <div className="flex h-8 w-8 items-center justify-center rounded-lg bg-gradient-to-br from-primary/20 to-primary/5">
                      <Search className="h-4 w-4 text-primary" />
                    </div>
                    <Command.Input
                      ref={inputRef}
                      value={query}
                      onValueChange={setQuery}
                      placeholder={placeholder}
                      className="flex-1 bg-transparent text-base font-normal outline-none placeholder:text-muted-foreground/60"
                      autoFocus
                    />
                    {query && (
                      <motion.button
                        initial={{ opacity: 0, scale: 0.8 }}
                        animate={{ opacity: 1, scale: 1 }}
                        onClick={() => setQuery("")}
                        className="rounded-md px-2 py-1 text-xs text-muted-foreground hover:bg-accent hover:text-accent-foreground transition-colors"
                      >
                        Clear
                      </motion.button>
                    )}
                    <kbd className="hidden sm:inline-flex h-6 items-center gap-1 rounded-md border bg-muted/50 px-2 font-mono text-[10px] font-medium text-muted-foreground">
                      ESC
                    </kbd>
                  </div>

                  <Command.List className="max-h-[400px] overflow-y-auto overscroll-contain p-2">
                    <Command.Empty className="flex flex-col items-center justify-center py-14 text-center">
                      <div className="mb-3 flex h-12 w-12 items-center justify-center rounded-full bg-muted/50">
                        <Search className="h-5 w-5 text-muted-foreground/50" />
                      </div>
                      <p className="text-sm text-muted-foreground">{emptyMessage}</p>
                      <p className="text-xs text-muted-foreground/60">Try searching for something else</p>
                    </Command.Empty>

                    {groups.map((group) => (
                      <Command.Group 
                        key={group.title} 
                        heading={group.title}
                        className="[&_[cmdk-group-heading]]:px-3 [&_[cmdk-group-heading]]:py-2 [&_[cmdk-group-heading]]:text-xs [&_[cmdk-group-heading]]:font-semibold [&_[cmdk-group-heading]]:uppercase [&_[cmdk-group-heading]]:tracking-wider [&_[cmdk-group-heading]]:text-muted-foreground/60"
                      >
                        {group.items.map((item) => (
                          <Command.Item
                            key={item.id}
                            value={`${group.title} ${item.title}`}
                            onSelect={() => handleSelect(item)}
                            className="group/item relative flex cursor-pointer select-none items-center gap-3 rounded-xl px-3 py-2.5 text-sm outline-none transition-colors hover:bg-accent/70 hover:text-accent-foreground aria-[selected='true']:bg-accent aria-[selected='true']:text-accent-foreground data-[disabled='true']:pointer-events-none data-[disabled='true']:opacity-50"
                          >
                            <div className="flex h-9 w-9 shrink-0 items-center justify-center rounded-lg bg-muted/50 text-muted-foreground group-aria-[selected='true']/item:bg-primary/10 group-aria-[selected='true']/item:text-primary transition-colors">
                              {item.icon}
                            </div>
                            <div className="flex flex-1 flex-col gap-0.5">
                              <span className="font-medium">{item.title}</span>
                              <span className="text-xs text-muted-foreground/60">{group.title}</span>
                            </div>
                            <ArrowRight className="h-4 w-4 opacity-0 transition-all group-aria-selected/item:opacity-100 group-aria-selected/item:translate-x-0 -translate-x-2 text-muted-foreground" />
                          </Command.Item>
                        ))}
                      </Command.Group>
                    ))}
                  </Command.List>

                  <div className="flex items-center justify-between border-t border-border/50 bg-muted/30 px-4 py-2.5">
                    <div className="flex items-center gap-4 text-xs text-muted-foreground/60">
                      <span className="flex items-center gap-1.5">
                        <kbd className="rounded border bg-background/80 px-1.5 py-0.5 font-mono text-[10px]">↑↓</kbd>
                        Navigate
                      </span>
                      <span className="flex items-center gap-1.5">
                        <kbd className="rounded border bg-background/80 px-1.5 py-0.5 font-mono text-[10px]">↵</kbd>
                        Select
                      </span>
                    </div>
                    <span className="text-xs text-muted-foreground/40">{brandName}</span>
                  </div>
                </Command>
              </motion.div>
            </>
          )}
        </AnimatePresence>,
        document.body
      )}
    </>
  )
}

CommandMenu.displayName = "CommandMenu"

export { CommandMenu }
```
---

## Cursor Driven Particle Typography <a name="cursor-driven-particle-typography"></a>

Component for cursor-driven-particle-typography

- **Registry Key**: `cursor-driven-particle-typography`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/cursor-driven-particle-typography.json
```

#### Component Source Code
##### File: `components/ui/cursor-driven-particle-typography.tsx`
```tsx
"use client";

import { cn } from "@workspace/ui/lib/utils";
import React, { useEffect, useRef } from "react";

export interface CursorDrivenParticleTypographyProps {
    /** Additional CSS classes */
    className?: string;
    /** The text to render */
    text: string;
    /** Font size in pixels */
    fontSize?: number;
    /** Font family */
    fontFamily?: string;
    /** Size of each particle */
    particleSize?: number;
    /** Density of particles (lower number = more particles, minimum 1) */
    particleDensity?: number;
    /** How strongly the cursor pushes particles away */
    dispersionStrength?: number;
    /** Speed at which particles return to origin */
    returnSpeed?: number;
    /** Custom color for particles. Overrides inherited text color if set. */
    color?: string;
}

class Particle {
    x: number;
    y: number;
    originX: number;
    originY: number;
    vx: number;
    vy: number;
    size: number;
    color: string;
    dispersion: number;
    returnSpd: number;

    constructor(
        x: number,
        y: number,
        size: number,
        color: string,
        dispersion: number,
        returnSpd: number
    ) {
        this.x = x + (Math.random() - 0.5) * 10; // start with slight randomness
        this.y = y + (Math.random() - 0.5) * 10;
        this.originX = x;
        this.originY = y;
        this.vx = (Math.random() - 0.5) * 5;
        this.vy = (Math.random() - 0.5) * 5;
        this.size = size;
        this.color = color;
        this.dispersion = dispersion;
        this.returnSpd = returnSpd;
    }

    update(mouseX: number, mouseY: number) {
        const dx = mouseX - this.x;
        const dy = mouseY - this.y;
        const distance = Math.sqrt(dx * dx + dy * dy);

        // Physics interaction with mouse
        const interactionRadius = 120; // 120px interaction radius

        if (distance < interactionRadius && mouseX !== -1000 && mouseY !== -1000) {
            const forceDirectionX = dx / distance;
            const forceDirectionY = dy / distance;

            const force = (interactionRadius - distance) / interactionRadius;

            // Calculate repulsion
            const repulsionX = forceDirectionX * force * this.dispersion;
            const repulsionY = forceDirectionY * force * this.dispersion;

            this.vx -= repulsionX;
            this.vy -= repulsionY;
        }

        // Return to origin (spring physics)
        this.vx += (this.originX - this.x) * this.returnSpd;
        this.vy += (this.originY - this.y) * this.returnSpd;

        // Friction
        this.vx *= 0.85;
        this.vy *= 0.85;

        // Add subtle noise/jitter when close to origin
        const distToOrigin = Math.sqrt(
            Math.pow(this.x - this.originX, 2) + Math.pow(this.y - this.originY, 2)
        );
        if (distToOrigin < 1 && Math.random() > 0.95) {
            this.vx += (Math.random() - 0.5) * 0.2;
            this.vy += (Math.random() - 0.5) * 0.2;
        }

        this.x += this.vx;
        this.y += this.vy;
    }

    draw(ctx: CanvasRenderingContext2D) {
        ctx.fillStyle = this.color;
        ctx.beginPath();
        ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
        ctx.fill();
    }
}

export function CursorDrivenParticleTypography({
    className,
    text,
    fontSize = 120,
    fontFamily = "Inter, sans-serif",
    particleSize = 1.5,
    particleDensity = 6,
    dispersionStrength = 15,
    returnSpeed = 0.08,
    color,
}: CursorDrivenParticleTypographyProps) {
    const canvasRef = useRef<HTMLCanvasElement>(null);
    const containerRef = useRef<HTMLDivElement>(null);

    useEffect(() => {
        const canvas = canvasRef.current;
        if (!canvas) return;
        const ctx = canvas.getContext("2d", { willReadFrequently: true });
        if (!ctx) return;

        let animationFrameId: number;
        let particles: Particle[] = [];

        let mouseX = -1000;
        let mouseY = -1000;

        let containerWidth = 0;
        let containerHeight = 0;

        const init = () => {
            const container = containerRef.current;
            if (!container) return;

            containerWidth = container.clientWidth;
            containerHeight = container.clientHeight;

            const dpr = window.devicePixelRatio || 1;
            canvas.width = containerWidth * dpr;
            canvas.height = containerHeight * dpr;
            canvas.style.width = `${containerWidth}px`;
            canvas.style.height = `${containerHeight}px`;

            ctx.scale(dpr, dpr);

            // Determine text color
            const computedStyle = window.getComputedStyle(container);
            const textColor = color || computedStyle.color || "#000000";

            ctx.clearRect(0, 0, containerWidth, containerHeight);

            // Draw text to generate pixel map
            ctx.fillStyle = textColor;
            // Responsive font size based on container width if text is large
            const effectiveFontSize = Math.min(fontSize, containerWidth * 0.15);
            ctx.font = `bold ${effectiveFontSize}px ${fontFamily}`;
            ctx.textAlign = "center";
            ctx.textBaseline = "middle";

            // Draw standard text first to measure it
            ctx.fillText(text, containerWidth / 2, containerHeight / 2);

            // Get pixel data
            const textCoordinates = ctx.getImageData(0, 0, canvas.width, canvas.height);
            particles = [];

            // Create particles from text pixels
            // Step by density multiplied by dpr
            const step = Math.max(1, Math.floor(particleDensity * dpr));
            for (let y = 0; y < textCoordinates.height; y += step) {
                for (let x = 0; x < textCoordinates.width; x += step) {
                    const index = (y * textCoordinates.width + x) * 4;
                    const alpha = textCoordinates.data[index + 3] || 0;

                    if (alpha > 128) {
                        particles.push(
                            new Particle(
                                x / dpr,
                                y / dpr,
                                particleSize,
                                textColor,
                                dispersionStrength,
                                returnSpeed
                            )
                        );
                    }
                }
            }
        };

        const animate = () => {
            ctx.clearRect(0, 0, containerWidth, containerHeight);

            particles.forEach((particle) => {
                particle.update(mouseX, mouseY);
                particle.draw(ctx);
            });
            animationFrameId = requestAnimationFrame(animate);
        };

        const handleMouseMove = (e: MouseEvent) => {
            const rect = canvas.getBoundingClientRect();
            mouseX = e.clientX - rect.left;
            mouseY = e.clientY - rect.top;
        };

        const handleMouseLeave = () => {
            mouseX = -1000;
            mouseY = -1000;
        };

        const handleResize = () => {
            init();
        };

        // Initialize with a short delay to ensure fonts/layout are ready
        const timeoutId = setTimeout(() => {
            init();
            animate();
        }, 100);

        const resizeObserver = new ResizeObserver(() => {
            handleResize();
        });

        if (containerRef.current) {
            resizeObserver.observe(containerRef.current);
        }

        // Re-initialize particles when the theme changes (detects class changes on html tag)
        const themeObserver = new MutationObserver(() => {
            init();
        });
        themeObserver.observe(document.documentElement, {
            attributes: true,
            attributeFilter: ["class"]
        });

        canvas.addEventListener("mousemove", handleMouseMove);
        canvas.addEventListener("mouseleave", handleMouseLeave);
        canvas.addEventListener("touchstart", (e) => {
            if (!e.touches[0]) return;
            const rect = canvas.getBoundingClientRect();
            mouseX = e.touches[0].clientX - rect.left;
            mouseY = e.touches[0].clientY - rect.top;
        });
        canvas.addEventListener("touchmove", (e) => {
            if (!e.touches[0]) return;
            const rect = canvas.getBoundingClientRect();
            mouseX = e.touches[0].clientX - rect.left;
            mouseY = e.touches[0].clientY - rect.top;
        });
        canvas.addEventListener("touchend", handleMouseLeave);

        return () => {
            clearTimeout(timeoutId);
            resizeObserver.disconnect();
            themeObserver.disconnect();
            canvas.removeEventListener("mousemove", handleMouseMove);
            canvas.removeEventListener("mouseleave", handleMouseLeave);
            cancelAnimationFrame(animationFrameId);
        };
    }, [text, fontSize, fontFamily, particleSize, particleDensity, dispersionStrength, returnSpeed, color]);

    return (
        <div
            ref={containerRef}
            className={cn("w-full h-full min-h-[400px] flex items-center justify-center relative touch-none", className)}
        >
            <canvas
                ref={canvasRef}
                className="block w-full h-full"
            />
        </div>
    );
}
```
---

## Dither Gradient <a name="dither-gradient"></a>

An animated dithered gradient background effect using canvas with Bayer matrix dithering.

- **Registry Key**: `dither-gradient`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/dither-gradient.json
```

#### How to Use
```tsx
"use client";

import { DitherGradient } from "@workspace/ui/components/dither-gradient";

export function DitherGradientDemo() {
  return (
    <div className="relative h-full w-full overflow-hidden bg-background">
      <DitherGradient />
      <div className="relative z-10 flex h-full items-center justify-center">
        <h3 className="text-2xl font-bold text-white drop-shadow-lg">
          Dither Gradient
        </h3>
      </div>
    </div>
  );
}

export function DitherGradientOceanDemo() {
  return (
    <div className="relative h-full w-full overflow-hidden bg-background">
      <DitherGradient
        colorFrom="#06b6d4"
        colorMid="#10b981"
        colorTo="#84cc16"
      />
      <div className="relative z-10 flex h-full items-center justify-center">
        <h3 className="text-2xl font-bold text-white drop-shadow-lg">
          Ocean to Forest
        </h3>
      </div>
    </div>
  );
}

export function DitherGradientSunsetDemo() {
  return (
    <div className="relative h-full w-full overflow-hidden bg-background">
      <DitherGradient
        colorFrom="#f97316"
        colorMid="#ef4444"
        colorTo="#be185d"
        intensity={0.2}
        speed={4}
        angle={120}
      />
      <div className="relative z-10 flex h-full items-center justify-center">
        <h3 className="text-2xl font-bold text-white drop-shadow-lg">
          Sunset Fire
        </h3>
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/dither-gradient.tsx`
```tsx
"use client"

import { useEffect, useRef } from "react"
import { cn } from "@/lib/utils"

interface DitherGradientProps {
  className?: string
  colorFrom?: string
  colorTo?: string
  colorMid?: string
  intensity?: number
  speed?: number
  angle?: number
}

export function DitherGradient({
  className,
  colorFrom = "#4f46e5",
  colorTo = "#ec4899",
  colorMid = "#a855f7",
  intensity = 0.15,
  speed = 3,
  angle = 45,
}: DitherGradientProps) {
  const canvasRef = useRef<HTMLCanvasElement>(null)
  const animationRef = useRef<number>(0)

  useEffect(() => {
    const canvas = canvasRef.current
    if (!canvas) return

    const ctx = canvas.getContext("2d")
    if (!ctx) return

    const resize = () => {
      const rect = canvas.getBoundingClientRect()
      canvas.width = rect.width
      canvas.height = rect.height
    }

    resize()
    window.addEventListener("resize", resize)

    let time = 0
    const bayerMatrix = [
      [0, 8, 2, 10],
      [12, 4, 14, 6],
      [3, 11, 1, 9],
      [15, 7, 13, 5],
    ]

    const hexToRgb = (hex: string) => {
      const result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex)
      if (!result) return { r: 0, g: 0, b: 0 }
      return {
        r: parseInt(result[1]!, 16),
        g: parseInt(result[2]!, 16),
        b: parseInt(result[3]!, 16),
      }
    }

    const smoothstep = (t: number) => t * t * (3 - 2 * t)

    const animate = () => {
      const { width, height } = canvas
      const imageData = ctx.createImageData(width, height)
      const data = imageData.data

      const from = hexToRgb(colorFrom)
      const mid = hexToRgb(colorMid)
      const to = hexToRgb(colorTo)
      const rad = (angle * Math.PI) / 180

      for (let y = 0; y < height; y++) {
        for (let x = 0; x < width; x++) {
          const normalizedX = x / width
          const normalizedY = y / height
          
          const gradientPos = 
            (normalizedX * Math.cos(rad) + normalizedY * Math.sin(rad)) * 0.8 + 
            0.1 + 
            Math.sin(time * speed * 0.0008) * 0.1

          const clampedPos = Math.max(0, Math.min(1, gradientPos))

          let r: number, g: number, b: number
          if (clampedPos < 0.5) {
            const t = smoothstep(clampedPos * 2)
            r = from.r + (mid.r - from.r) * t
            g = from.g + (mid.g - from.g) * t
            b = from.b + (mid.b - from.b) * t
          } else {
            const t = smoothstep((clampedPos - 0.5) * 2)
            r = mid.r + (to.r - mid.r) * t
            g = mid.g + (to.g - mid.g) * t
            b = mid.b + (to.b - mid.b) * t
          }

          const threshold = (bayerMatrix[y % 4]![x % 4]! / 16 - 0.5) * intensity * 180
          const noise = (Math.random() - 0.5) * intensity * 60

          const idx = (y * width + x) * 4
          data[idx] = Math.min(255, Math.max(0, r + threshold + noise))
          data[idx + 1] = Math.min(255, Math.max(0, g + threshold + noise))
          data[idx + 2] = Math.min(255, Math.max(0, b + threshold + noise))
          data[idx + 3] = 255
        }
      }

      ctx.putImageData(imageData, 0, 0)
      time += 16
      animationRef.current = requestAnimationFrame(animate)
    }

    animate()

    return () => {
      window.removeEventListener("resize", resize)
      cancelAnimationFrame(animationRef.current)
    }
  }, [colorFrom, colorTo, colorMid, intensity, speed, angle])

  return (
    <canvas
      ref={canvasRef}
      className={cn("absolute inset-0 h-full w-full", className)}
    />
  )
}
```
---

## Dither Prism Hero <a name="dither-prism-hero"></a>

A stunning WebGL hero background featuring advanced dithering patterns, prismatic color refraction, holographic iridescence, and center-focused ripple energy.

- **Registry Key**: `dither-prism-hero`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/dither-prism-hero.json
```

#### Dependencies
- **NPM Packages**:
  - `@react-three/fiber`
  - `@react-three/drei`
  - `three`
  - `framer-motion`

#### How to Use
```tsx
"use client";

import { useState } from "react";
import { RotateCcw } from "lucide-react";
import DitherPrismHero from "@workspace/ui/components/dither-prism-hero";
import { cn } from "@/lib/utils";
import { Instrument_Serif } from "next/font/google";

const instrumentSerif = Instrument_Serif({
    subsets: ["latin"],
    weight: "400",
    style: ["normal", "italic"],
    variable: "--font-serif",
});

interface DitherPrismHeroPreviewProps {
    title1?: string;
    title2?: string;
    color1?: string;
    color2?: string;
    color3?: string;
    speed?: number;
    ditherIntensity?: number;
    prismIntensity?: number;
    showParticles?: boolean;
    particleCount?: number;
}

function DitherPrismHeroPreviewWrapper({
    title1 = "Experience",
    title2 = "The Future",
    color1,
    color2,
    color3,
    speed,
    ditherIntensity,
    prismIntensity,
    showParticles,
    particleCount,
}: DitherPrismHeroPreviewProps) {
    const [key, setKey] = useState(0);

    return (
        <div className="relative">
            <div
                className={cn(
                    "max-w-full overflow-hidden aspect-video w-full group relative",
                    instrumentSerif.variable
                )}
            >
                <button
                    onClick={() => setKey((prev) => prev + 1)}
                    className="absolute top-4 right-4 z-50 p-2 bg-white/10 backdrop-blur-md border border-white/20 rounded-full text-white hover:scale-110 transition-all cursor-pointer"
                    aria-label="Reload animation"
                >
                    <RotateCcw className="w-4 h-4 text-white" />
                </button>
                <DitherPrismHero
                    key={key}
                    title1={title1}
                    title2={title2}
                    color1={color1}
                    color2={color2}
                    color3={color3}
                    speed={speed}
                    ditherIntensity={ditherIntensity}
                    prismIntensity={prismIntensity}
                    showParticles={showParticles}
                    particleCount={particleCount}
                    className="min-h-full"
                />
            </div>
        </div>
    );
}

// Default demo - vibrant purple/pink theme
export function DitherPrismHeroDemo() {
    return (
        <DitherPrismHeroPreviewWrapper
            title1="Experience"
            title2="The Future"
        />
    );
}

// Cyberpunk theme - neon green/cyan
export function DitherPrismHeroCyberpunkDemo() {
    return (
        <DitherPrismHeroPreviewWrapper
            color1="#0a0a0a"
            color2="#00ff88"
            color3="#00ffff"
            title1="Cyber"
            title2="Punk"
            ditherIntensity={0.25}
            prismIntensity={0.7}
        />
    );
}

// Sunset theme - warm oranges and reds
export function DitherPrismHeroSunsetDemo() {
    return (
        <DitherPrismHeroPreviewWrapper
            color1="#1a0a0a"
            color2="#ff6b35"
            color3="#ffd93d"
            title1="Golden"
            title2="Hour"
            ditherIntensity={0.12}
            prismIntensity={0.4}
        />
    );
}

// Ocean theme - deep blues and teals
export function DitherPrismHeroOceanDemo() {
    return (
        <DitherPrismHeroPreviewWrapper
            color1="#0a1628"
            color2="#0ea5e9"
            color3="#22d3ee"
            title1="Deep"
            title2="Ocean"
            speed={0.7}
            showParticles={true}
            particleCount={100}
        />
    );
}

// Maximum intensity - for showcase
export function DitherPrismHeroIntenseDemo() {
    return (
        <DitherPrismHeroPreviewWrapper
            ditherIntensity={0.3}
            prismIntensity={0.9}
            speed={1.5}
            title1="Maximum"
            title2="Impact"
        />
    );
}
```
#### Component Source Code
##### File: `components/ui/dither-prism-hero.tsx`
```tsx
"use client";

/* eslint-disable react/no-unknown-property */
import { useRef, useMemo, useEffect, useState } from "react";
import { Canvas, useFrame, ThreeElements, useThree } from "@react-three/fiber";
import * as THREE from "three";
import { motion } from "framer-motion";

import { cn } from "@/lib/utils";
import { WebGLErrorBoundary, WebGLFallback } from "./webgl-error-boundary";

// Type augmentation for R3F
declare global {
    // eslint-disable-next-line @typescript-eslint/no-namespace
    namespace JSX {
        type IntrinsicElements = ThreeElements;
    }
}

// ═══════════════════════════════════════════════════════════════════════════════
// VERTEX SHADER
// ═══════════════════════════════════════════════════════════════════════════════
const vertexShader = `
varying vec2 vUv;
varying vec3 vPosition;

void main() {
  vUv = uv;
  vPosition = position;
  gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
}
`;

// ═══════════════════════════════════════════════════════════════════════════════
// FRAGMENT SHADER - The magic happens here!
// ═══════════════════════════════════════════════════════════════════════════════
const fragmentShader = `
uniform float uTime;
uniform vec2 uResolution;
uniform vec2 uMouse;
uniform float uMouseIntensity;
uniform vec3 uColor1;
uniform vec3 uColor2;
uniform vec3 uColor3;
uniform float uDitherIntensity;
uniform float uPrismIntensity;
varying vec2 vUv;
varying vec3 vPosition;

// ─────────────────────────────────────────────────────────────────────────────
// Hash functions for procedural noise
// ─────────────────────────────────────────────────────────────────────────────
float hash(vec2 p) {
  return fract(sin(dot(p, vec2(127.1, 311.7))) * 43758.5453);
}

float hash3(vec3 p) {
  return fract(sin(dot(p, vec3(127.1, 311.7, 74.7))) * 43758.5453);
}

// ─────────────────────────────────────────────────────────────────────────────
// Simplex 2D Noise
// ─────────────────────────────────────────────────────────────────────────────
vec3 permute(vec3 x) { return mod(((x*34.0)+1.0)*x, 289.0); }

float snoise(vec2 v) {
  const vec4 C = vec4(0.211324865405187, 0.366025403784439,
           -0.577350269189626, 0.024390243902439);
  vec2 i  = floor(v + dot(v, C.yy));
  vec2 x0 = v -   i + dot(i, C.xx);
  vec2 i1;
  i1 = (x0.x > x0.y) ? vec2(1.0, 0.0) : vec2(0.0, 1.0);
  vec4 x12 = x0.xyxy + C.xxzz;
  x12.xy -= i1;
  i = mod(i, 289.0);
  vec3 p = permute( permute( i.y + vec3(0.0, i1.y, 1.0))
  + i.x + vec3(0.0, i1.x, 1.0));
  vec3 m = max(0.5 - vec3(dot(x0,x0), dot(x12.xy,x12.xy), dot(x12.zw,x12.zw)), 0.0);
  m = m*m;
  m = m*m;
  vec3 x = 2.0 * fract(p * C.www) - 1.0;
  vec3 h = abs(x) - 0.5;
  vec3 ox = floor(x + 0.5);
  vec3 a0 = x - ox;
  m *= 1.79284291400159 - 0.85373472095314 * (a0*a0 + h*h);
  vec3 g;
  g.x  = a0.x  * x0.x  + h.x  * x0.y;
  g.yz = a0.yz * x12.xz + h.yz * x12.yw;
  return 130.0 * dot(m, g);
}

// ─────────────────────────────────────────────────────────────────────────────
// FBM (Fractal Brownian Motion) for layered noise
// ─────────────────────────────────────────────────────────────────────────────
float fbm(vec2 p, int octaves) {
  float value = 0.0;
  float amplitude = 0.5;
  float frequency = 1.0;
  for (int i = 0; i < 6; i++) {
    if (i >= octaves) break;
    value += amplitude * snoise(p * frequency);
    frequency *= 2.0;
    amplitude *= 0.5;
  }
  return value;
}

// ─────────────────────────────────────────────────────────────────────────────
// Advanced 8x8 Bayer Matrix for ordered dithering
// ─────────────────────────────────────────────────────────────────────────────
float bayer8x8(vec2 uv) {
  ivec2 p = ivec2(mod(uv, 8.0));
  int matrix[64];
  matrix[0] = 0;  matrix[1] = 32; matrix[2] = 8;  matrix[3] = 40; matrix[4] = 2;  matrix[5] = 34; matrix[6] = 10; matrix[7] = 42;
  matrix[8] = 48; matrix[9] = 16; matrix[10] = 56; matrix[11] = 24; matrix[12] = 50; matrix[13] = 18; matrix[14] = 58; matrix[15] = 26;
  matrix[16] = 12; matrix[17] = 44; matrix[18] = 4; matrix[19] = 36; matrix[20] = 14; matrix[21] = 46; matrix[22] = 6; matrix[23] = 38;
  matrix[24] = 60; matrix[25] = 28; matrix[26] = 52; matrix[27] = 20; matrix[28] = 62; matrix[29] = 30; matrix[30] = 54; matrix[31] = 22;
  matrix[32] = 3;  matrix[33] = 35; matrix[34] = 11; matrix[35] = 43; matrix[36] = 1;  matrix[37] = 33; matrix[38] = 9;  matrix[39] = 41;
  matrix[40] = 51; matrix[41] = 19; matrix[42] = 59; matrix[43] = 27; matrix[44] = 49; matrix[45] = 17; matrix[46] = 57; matrix[47] = 25;
  matrix[48] = 15; matrix[49] = 47; matrix[50] = 7; matrix[51] = 39; matrix[52] = 13; matrix[53] = 45; matrix[54] = 5; matrix[55] = 37;
  matrix[56] = 63; matrix[57] = 31; matrix[58] = 55; matrix[59] = 23; matrix[60] = 61; matrix[61] = 29; matrix[62] = 53; matrix[63] = 21;
  return float(matrix[p.y * 8 + p.x]) / 64.0;
}

// ─────────────────────────────────────────────────────────────────────────────
// Blue Noise approximation for organic dithering
// ─────────────────────────────────────────────────────────────────────────────
float blueNoise(vec2 uv, float time) {
  float n1 = hash(uv + vec2(time * 0.1, 0.0));
  float n2 = hash(uv * 2.1 + vec2(0.0, time * 0.13));
  float n3 = hash(uv * 4.3 + vec2(time * 0.07, time * 0.11));
  return fract(n1 + n2 * 0.5 + n3 * 0.25);
}

// ─────────────────────────────────────────────────────────────────────────────
// Prismatic color refraction - creates rainbow light scattering
// ─────────────────────────────────────────────────────────────────────────────
vec3 prism(vec2 uv, float time, float intensity) {
  float angle = atan(uv.y - 0.5, uv.x - 0.5);
  float dist = length(uv - 0.5);
  
  // Create rotating prismatic effect
  float prismAngle = angle + time * 0.3 + dist * 3.0;
  
  // RGB separation based on angle
  float r = 0.5 + 0.5 * sin(prismAngle);
  float g = 0.5 + 0.5 * sin(prismAngle + 2.094); // 120 degrees
  float b = 0.5 + 0.5 * sin(prismAngle + 4.188); // 240 degrees
  
  return vec3(r, g, b) * intensity;
}

// ─────────────────────────────────────────────────────────────────────────────
// Holographic iridescence effect
// ─────────────────────────────────────────────────────────────────────────────
vec3 iridescence(vec2 uv, float time) {
  float t = time * 0.5;
  vec2 p = uv * 3.0;
  
  float n1 = snoise(p + vec2(t, 0.0));
  float n2 = snoise(p * 1.3 + vec2(0.0, t * 0.7));
  float n3 = snoise(p * 0.7 + vec2(t * 0.5, t * 0.3));
  
  vec3 col1 = vec3(0.5 + 0.5 * sin(n1 * 3.14159 + t));
  vec3 col2 = vec3(0.5 + 0.5 * sin(n2 * 3.14159 + t * 1.3 + 2.0));
  vec3 col3 = vec3(0.5 + 0.5 * sin(n3 * 3.14159 + t * 0.7 + 4.0));
  
  return (col1 + col2 + col3) / 3.0;
}

// ─────────────────────────────────────────────────────────────────────────────
// Geometrically morphing shapes - creates flowing crystal/diamond patterns
// ─────────────────────────────────────────────────────────────────────────────
float diamond(vec2 p) {
  return abs(p.x) + abs(p.y);
}

float morphShape(vec2 uv, float time) {
  float morph = sin(time * 0.4) * 0.5 + 0.5;
  
  vec2 p = uv * 4.0 - 2.0;
  p = p + vec2(sin(time * 0.3), cos(time * 0.4)) * 0.5;
  
  // Morphing between circle and diamond
  float circle = length(p) - 1.0;
  float diam = diamond(p) - 1.4;
  
  float shape = mix(circle, diam, morph);
  
  // Create multiple copies
  vec2 q = mod(uv * 8.0, 2.0) - 1.0;
  float multiShape = mix(length(q), diamond(q), morph) - 0.3;
  
  return min(shape, multiShape);
}

// ─────────────────────────────────────────────────────────────────────────────
// Mouse interaction - creates DRAMATIC ripple effect from cursor
// ─────────────────────────────────────────────────────────────────────────────
float mouseRipple(vec2 uv, vec2 mouse, float time, float intensity) {
  float dist = length(uv - mouse);
  // Multiple concentric ripples
  float ripple1 = sin(dist * 40.0 - time * 5.0) * exp(-dist * 3.0);
  float ripple2 = sin(dist * 25.0 - time * 3.5 + 1.0) * exp(-dist * 4.0);
  float ripple3 = sin(dist * 60.0 - time * 7.0) * exp(-dist * 5.0);
  return (ripple1 + ripple2 * 0.5 + ripple3 * 0.3) * intensity;
}

// Mouse glow - creates bright aura around cursor
vec3 mouseGlow(vec2 uv, vec2 mouse, float time, float intensity, vec3 glowColor) {
  float dist = length(uv - mouse);
  
  // Inner bright core
  float core = exp(-dist * 15.0) * 1.5;
  
  // Outer soft glow
  float outer = exp(-dist * 5.0) * 0.8;
  
  // Pulsing effect
  float pulse = 0.8 + 0.2 * sin(time * 3.0);
  
  // Rainbow chromatic aberration around cursor
  float chromatic = sin(dist * 30.0 + time * 2.0) * exp(-dist * 8.0);
  vec3 rainbow = vec3(
    sin(time * 2.0) * 0.5 + 0.5,
    sin(time * 2.0 + 2.094) * 0.5 + 0.5,
    sin(time * 2.0 + 4.188) * 0.5 + 0.5
  );
  
  vec3 glow = glowColor * (core + outer) * pulse * intensity;
  glow += rainbow * chromatic * intensity * 0.5;
  
  return glow;
}

// Lens distortion around cursor
vec2 mouseLensDistort(vec2 uv, vec2 mouse, float intensity) {
  vec2 delta = uv - mouse;
  float dist = length(delta);
  float distortion = exp(-dist * 6.0) * intensity * 0.15;
  return uv + normalize(delta + 0.001) * distortion;
}

// ─────────────────────────────────────────────────────────────────────────────
// MAIN SHADER
// ─────────────────────────────────────────────────────────────────────────────
void main() {
  vec2 uv = vUv;
  vec2 pixelCoord = gl_FragCoord.xy;
  float time = uTime;
  
  // ═══════════════════════════════════════════════════════════════════
  // Layer 0: Apply mouse lens distortion to UV coordinates FIRST
  // ═══════════════════════════════════════════════════════════════════
  vec2 distortedUv = mouseLensDistort(uv, uMouse, uMouseIntensity);
  
  // ═══════════════════════════════════════════════════════════════════
  // Layer 1: Base flowing gradient with noise (using distorted UVs)
  // ═══════════════════════════════════════════════════════════════════
  float noise1 = fbm(distortedUv * 2.0 + vec2(time * 0.05, time * 0.03), 4);
  float noise2 = fbm(distortedUv * 3.0 + vec2(-time * 0.04, time * 0.06), 3);
  
  float diagonal = (distortedUv.x + distortedUv.y) * 0.5;
  float flow = diagonal + noise1 * 0.3 + noise2 * 0.2;
  flow += sin(time * 0.2) * 0.1;
  
  // ═══════════════════════════════════════════════════════════════════
  // Layer 2: Color mixing with tri-color gradient
  // ═══════════════════════════════════════════════════════════════════
  vec3 col;
  float t1 = smoothstep(0.0, 0.5, flow);
  float t2 = smoothstep(0.5, 1.0, flow);
  
  col = mix(uColor1, uColor2, t1);
  col = mix(col, uColor3, t2);
  
  // ═══════════════════════════════════════════════════════════════════
  // Layer 3: Prismatic light refraction
  // ═══════════════════════════════════════════════════════════════════
  vec3 prismColor = prism(distortedUv, time, uPrismIntensity);
  
  // Apply prism only at edges/transitions
  float edgeMask = abs(fract(flow * 5.0) - 0.5) * 2.0;
  edgeMask = smoothstep(0.3, 0.7, edgeMask);
  col += prismColor * edgeMask * 0.4;
  
  // ═══════════════════════════════════════════════════════════════════
  // Layer 4: Iridescent holographic overlay
  // ═══════════════════════════════════════════════════════════════════
  vec3 iris = iridescence(distortedUv, time);
  float irisMask = snoise(distortedUv * 5.0 + time * 0.1);
  irisMask = smoothstep(-0.2, 0.8, irisMask) * 0.15;
  col = mix(col, iris, irisMask);
  
  // ═══════════════════════════════════════════════════════════════════
  // Layer 5: Geometric crystal patterns
  // ═══════════════════════════════════════════════════════════════════
  float shape = morphShape(distortedUv, time);
  float shapeMask = 1.0 - smoothstep(-0.1, 0.1, shape);
  col = mix(col, col * 1.15 + vec3(0.08), shapeMask * 0.3);
  
  // ═══════════════════════════════════════════════════════════════════
  // Layer 6: VISIBLE Mouse interaction - ripples + glow + color shift
  // ═══════════════════════════════════════════════════════════════════
  float ripple = mouseRipple(uv, uMouse, time, uMouseIntensity);
  
  // Add dramatic ripple color changes
  col += ripple * prismColor * 1.2;
  col += ripple * vec3(0.3, 0.2, 0.4);
  
  // Add bright glowing cursor aura
  vec3 glow = mouseGlow(uv, uMouse, time, uMouseIntensity, vec3(1.0, 0.8, 1.0));
  col += glow;
  
  // Color shift near cursor - make area around mouse more vibrant
  float mouseDist = length(uv - uMouse);
  float proximityBoost = exp(-mouseDist * 4.0) * uMouseIntensity;
  col = mix(col, col * 1.5 + prismColor * 0.3, proximityBoost);
  
  // ═══════════════════════════════════════════════════════════════════
  // Layer 7: ADVANCED DITHERING - The signature look!
  // ═══════════════════════════════════════════════════════════════════
  
  // 8x8 Bayer ordered dithering
  float bayer = bayer8x8(pixelCoord);
  
  // Animated blue noise
  float blue = blueNoise(pixelCoord * 0.1, time);
  
  // Combine dithering patterns
  float ditherPattern = mix(bayer, blue, 0.3 + 0.2 * sin(time * 0.5));
  
  // Apply dithering to create the signature grainy/retro look
  vec3 ditherOffset = (vec3(ditherPattern) - 0.5) * uDitherIntensity;
  col += ditherOffset;
  
  // Quantize colors for retro dithered appearance
  float levels = 16.0;
  vec3 quantized = floor(col * levels + ditherPattern) / levels;
  col = mix(col, quantized, uDitherIntensity * 0.5);
  
  // ═══════════════════════════════════════════════════════════════════
  // Layer 8: Scanline effect for extra depth
  // ═══════════════════════════════════════════════════════════════════
  float scanline = sin(pixelCoord.y * 2.0 + time * 2.0) * 0.02;
  col += scanline * uDitherIntensity;
  
  // ═══════════════════════════════════════════════════════════════════
  // Layer 9: Vignette for cinematic depth
  // ═══════════════════════════════════════════════════════════════════
  float vignette = 1.0 - length((uv - 0.5) * 1.2);
  vignette = smoothstep(0.0, 0.7, vignette);
  col *= 0.85 + vignette * 0.15;
  
  // ═══════════════════════════════════════════════════════════════════
  // Final output
  // ═══════════════════════════════════════════════════════════════════
  col = clamp(col, 0.0, 1.0);
  gl_FragColor = vec4(col, 1.0);
}
`;

// ═══════════════════════════════════════════════════════════════════════════════
// The WebGL Plane Component
// ═══════════════════════════════════════════════════════════════════════════════
const DitherPrismPlane = ({
    color1,
    color2,
    color3,
    speed = 1,
    ditherIntensity = 0.15,
    prismIntensity = 0.5,
}: {
    color1: string;
    color2: string;
    color3: string;
    speed?: number;
    ditherIntensity?: number;
    prismIntensity?: number;
}) => {
    const meshRef = useRef<THREE.Mesh>(null);
    const { size } = useThree();

    const uniforms = useMemo(
        () => ({
            uTime: { value: 0 },
            uResolution: { value: new THREE.Vector2(1000, 1000) },
            uMouse: { value: new THREE.Vector2(0.5, 0.5) },
            uMouseIntensity: { value: 0.8 },
            uColor1: { value: new THREE.Color(color1) },
            uColor2: { value: new THREE.Color(color2) },
            uColor3: { value: new THREE.Color(color3) },
            uDitherIntensity: { value: ditherIntensity },
            uPrismIntensity: { value: prismIntensity },
        }),
        // eslint-disable-next-line react-hooks/exhaustive-deps
        [] // Depend on nothing to keep reference stable
    );

    useFrame((state) => {
        const { clock } = state;

        uniforms.uTime.value = clock.getElapsedTime() * speed;
        uniforms.uResolution.value.set(size.width, size.height);
        uniforms.uMouse.value.set(0.5, 0.5);
        uniforms.uMouseIntensity.value = 0.8;
        uniforms.uColor1.value.set(color1);
        uniforms.uColor2.value.set(color2);
        uniforms.uColor3.value.set(color3);
        uniforms.uDitherIntensity.value = ditherIntensity;
        uniforms.uPrismIntensity.value = prismIntensity;
    });

    return (
        <mesh ref={meshRef} scale={[2, 2, 1]}>
            <planeGeometry args={[2, 2]} />
            <shaderMaterial
                vertexShader={vertexShader}
                fragmentShader={fragmentShader}
                uniforms={uniforms}
                transparent={true}
                depthWrite={false}
                depthTest={false}
            />
        </mesh>
    );
};

// ═══════════════════════════════════════════════════════════════════════════════
// Floating Particles Layer - Adds depth and interactivity
// ═══════════════════════════════════════════════════════════════════════════════
const FloatingParticles = ({
    count = 50,
    color = "#ffffff",
}: {
    count?: number;
    color?: string;
}) => {
    const pointsRef = useRef<THREE.Points>(null);

    const particles = useMemo(() => {
        const positions = new Float32Array(count * 3);
        const sizes = new Float32Array(count);
        const phases = new Float32Array(count);

        for (let i = 0; i < count; i++) {
            positions[i * 3] = (Math.random() - 0.5) * 4;
            positions[i * 3 + 1] = (Math.random() - 0.5) * 4;
            positions[i * 3 + 2] = (Math.random() - 0.5) * 2;
            sizes[i] = Math.random() * 3 + 1;
            phases[i] = Math.random() * Math.PI * 2;
        }

        return { positions, sizes, phases };
    }, [count]);

    useFrame(({ clock }) => {
        if (!pointsRef.current?.geometry?.attributes?.position) return;
        const time = clock.getElapsedTime();
        const positionAttr = pointsRef.current.geometry.attributes.position;
        const positions = positionAttr.array as Float32Array;

        for (let i = 0; i < count; i++) {
            const phase = particles.phases[i] ?? 0;
            const yIdx = i * 3 + 1;
            const xIdx = i * 3;
            positions[yIdx] = (positions[yIdx] ?? 0) + Math.sin(time + phase) * 0.001;
            positions[xIdx] = (positions[xIdx] ?? 0) + Math.cos(time * 0.5 + phase) * 0.0005;

            // Wrap particles
            if ((positions[yIdx] ?? 0) > 2) positions[yIdx] = -2;
            if ((positions[yIdx] ?? 0) < -2) positions[yIdx] = 2;
        }

        positionAttr.needsUpdate = true;
    });

    return (
        <points ref={pointsRef}>
            <bufferGeometry>
                <bufferAttribute
                    attach="attributes-position"
                    args={[particles.positions, 3]}
                    count={count}
                />
                <bufferAttribute
                    attach="attributes-size"
                    args={[particles.sizes, 1]}
                    count={count}
                />
            </bufferGeometry>
            <pointsMaterial
                color={color}
                size={0.02}
                transparent
                opacity={0.6}
                sizeAttenuation
                blending={THREE.AdditiveBlending}
            />
        </points>
    );
};

// ═══════════════════════════════════════════════════════════════════════════════
// Main Component Props
// ═══════════════════════════════════════════════════════════════════════════════
interface DitherPrismHeroProps extends React.HTMLAttributes<HTMLDivElement> {
    /** First line of headline */
    title1?: string;
    /** Second line of headline */
    title2?: string;
    /** Primary color (deep/dark) */
    color1?: string;
    /** Secondary color (mid) */
    color2?: string;
    /** Tertiary color (light/accent) */
    color3?: string;
    /** Animation speed multiplier */
    speed?: number;
    /** Dithering intensity (0-1) */
    ditherIntensity?: number;
    /** Prismatic refraction intensity (0-1) */
    prismIntensity?: number;
    /** Number of floating particles */
    particleCount?: number;
    /** Show floating particles */
    showParticles?: boolean;
    /** Particle color */
    particleColor?: string;
    /** Children to render on top */
    children?: React.ReactNode;
}

const HERO_HEADLINE_CLASS =
    "pb-[0.08em] text-[12cqi] md:text-[8cqi] lg:text-[6cqi] leading-[0.96] tracking-tighter font-bold text-transparent bg-clip-text bg-gradient-to-b from-zinc-900 via-zinc-500 to-zinc-800";

// ═══════════════════════════════════════════════════════════════════════════════
// Main Export Component
// ═══════════════════════════════════════════════════════════════════════════════
export default function DitherPrismHero({
    title1,
    title2,
    color1 = "#0f0f23",
    color2 = "#6366f1",
    color3 = "#ec4899",
    speed = 1,
    ditherIntensity = 0.15,
    prismIntensity = 0.5,
    particleCount = 50,
    showParticles = true,
    particleColor = "#ffffff",
    className,
    children,
    style,
    ...props
}: DitherPrismHeroProps) {
    const [mounted, setMounted] = useState(false);

    useEffect(() => {
        setMounted(true);
    }, []);

    return (
        <div
            className={cn(
                "relative w-full min-h-screen flex flex-col items-center overflow-hidden text-gray-900",
                className
            )}
            style={{ containerType: "size", ...style }}
            {...props}
        >
            {/* WebGL Background */}
            {mounted && (
                <div className="absolute top-0 left-0 w-full h-full z-0">
                    <WebGLErrorBoundary fallback={<WebGLFallback className="absolute inset-0 h-full w-full" />}>
                        <Canvas
                            camera={{ position: [0, 0, 1] }}
                            dpr={[1, 2]}
                            gl={{
                                antialias: false,
                                alpha: true,
                                powerPreference: "high-performance",
                            }}
                        >
                            <DitherPrismPlane
                                color1={color1}
                                color2={color2}
                                color3={color3}
                                speed={speed}
                                ditherIntensity={ditherIntensity}
                                prismIntensity={prismIntensity}
                            />
                            {showParticles && (
                                <FloatingParticles count={particleCount} color={particleColor} />
                            )}
                        </Canvas>
                    </WebGLErrorBoundary>
                </div>
            )}

            {/* Content Overlay */}
            {(title1 || title2 || children) && (
                <div className="relative z-10 w-full flex-1 flex flex-col items-center justify-center pt-8 pb-8 md:pt-20 md:pb-20">
                    <div className="w-full max-w-[1200px] px-6 flex flex-col items-center">
                        {/* Headline */}
                        {(title1 || title2) && (
                            <div className="flex flex-col items-center text-center gap-2 md:gap-4 mb-8 md:mb-12">
                                {title1 && (
                                    <div className="overflow-hidden">
                                        <motion.h1
                                            initial={{ y: "100%", opacity: 0 }}
                                            animate={{ y: "0%", opacity: 1 }}
                                            transition={{
                                                duration: 1,
                                                ease: [0.16, 1, 0.3, 1],
                                                delay: 0.2,
                                            }}
                                            className={HERO_HEADLINE_CLASS}
                                        >
                                            <span>
                                                {title1}
                                            </span>
                                        </motion.h1>
                                    </div>
                                )}
                                {title2 && (
                                    <div className="overflow-hidden">
                                        <motion.h1
                                            initial={{ y: "100%", opacity: 0 }}
                                            animate={{ y: "0%", opacity: 1 }}
                                            transition={{
                                                duration: 1,
                                                ease: [0.16, 1, 0.3, 1],
                                                delay: 0.35,
                                            }}
                                            className={HERO_HEADLINE_CLASS}
                                        >
                                            {title2}
                                        </motion.h1>
                                    </div>
                                )}
                            </div>
                        )}

                        {/* Custom Children */}
                        {children && (
                            <motion.div
                                initial={{ opacity: 0, y: 20 }}
                                animate={{ opacity: 1, y: 0 }}
                                transition={{ duration: 0.8, delay: 0.7, ease: "easeOut" }}
                            >
                                {children}
                            </motion.div>
                        )}
                    </div>
                </div>
            )}
        </div>
    );
}

// Named export for easier imports
export { DitherPrismHero };
```
##### File: `components/ui/webgl-error-boundary.tsx`
```tsx
"use client";

import { cn } from "@/lib/utils";
import * as React from "react";

interface WebGLErrorBoundaryProps {
  children: React.ReactNode;
  fallback?: React.ReactNode;
  onError?: (error: Error, errorInfo: React.ErrorInfo) => void;
}

interface WebGLErrorBoundaryState {
  hasError: boolean;
}

export class WebGLErrorBoundary extends React.Component<
  WebGLErrorBoundaryProps,
  WebGLErrorBoundaryState
> {
  public state: WebGLErrorBoundaryState = { hasError: false };

  static getDerivedStateFromError(): WebGLErrorBoundaryState {
    return { hasError: true };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    this.props.onError?.(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback ?? <WebGLFallback />;
    }
    return this.props.children;
  }
}

interface WebGLFallbackProps {
  className?: string;
  message?: string;
}

export function WebGLFallback({
  className,
  message = "Interactive WebGL content is unavailable on this device/browser.",
}: WebGLFallbackProps) {
  return (
    <div
      className={cn(
        "flex h-full w-full items-center justify-center bg-gradient-to-br from-zinc-950 via-slate-900 to-zinc-900 px-4 text-center text-sm text-white/75",
        className,
      )}
      role="status"
      aria-live="polite"
    >
      <p>{message}</p>
    </div>
  );
}
```
---

## Eye Tracking <a name="eye-tracking"></a>

Component for eye-tracking

- **Registry Key**: `eye-tracking`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/eye-tracking.json
```

#### How to Use
```tsx
"use client"

import * as React from "react"
import { EyeTracking } from "@workspace/ui/components/eye-tracking"

export function EyeTrackingDemo() {
  return (
    <div className="flex min-h-[350px] w-full items-center justify-center">
      <EyeTracking eyeSize={140} gap={50} />
    </div>
  )
}

export function EyeTrackingCartoonDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center">
      <EyeTracking
        variant="cartoon"
        eyeSize={160}
        gap={30}
        irisColor="#3B82F6"
        irisColorSecondary="#60A5FA"
        pupilRange={0.8}
      />
    </div>
  )
}

export function EyeTrackingCyberDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center">
      <EyeTracking
        variant="cyber"
        eyeSize={130}
        gap={60}
      />
    </div>
  )
}

export function EyeTrackingMinimalDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center">
      <EyeTracking
        variant="minimal"
        eyeSize={100}
        gap={30}
        irisColor="#18181B"
        irisColorSecondary="#3F3F46"
        showIrisDetail={false}
        showEyelids={false}
        showReflection={true}
      />
    </div>
  )
}

export function EyeTrackingTripleDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center">
      <EyeTracking
        eyeCount={3}
        eyeSize={100}
        gap={30}
        irisColor="#8B5CF6"
        irisColorSecondary="#A78BFA"
      />
    </div>
  )
}

export function EyeTrackingBrownDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center">
      <EyeTracking
        eyeSize={150}
        gap={45}
        irisColor="#6B3A1F"
        irisColorSecondary="#D4A574"
        blinkInterval={3000}
      />
    </div>
  )
}
```
#### Component Source Code
##### File: `components/ui/eye-tracking.tsx`
```tsx
"use client"

import * as React from "react"
import { motion, useMotionValue, useSpring, useTransform } from "framer-motion"
import { cn } from "@/lib/utils"

interface EyeTrackingProps {
  /** Additional CSS classes */
  className?: string
  /** Size of each eye in pixels */
  eyeSize?: number
  /** Gap between eyes in pixels */
  gap?: number
  /** Color of the iris */
  irisColor?: string
  /** Secondary iris color for gradient */
  irisColorSecondary?: string
  /** Pupil color */
  pupilColor?: string
  /** Sclera (white) color */
  scleraColor?: string
  /** How far the pupil can travel (0-1) */
  pupilRange?: number
  /** Enable the reflection/glint effect */
  showReflection?: boolean
  /** Enable iris detail pattern */
  showIrisDetail?: boolean
  /** Enable subtle idle animation when cursor is away */
  idleAnimation?: boolean
  /** Blink interval in milliseconds (0 to disable) */
  blinkInterval?: number
  /** Number of eyes */
  eyeCount?: number
  /** Variant style */
  variant?: "realistic" | "cartoon" | "minimal" | "cyber"
  /** Enable reactive pupil dilation */
  reactivePupil?: boolean
  /** Eyelid visibility */
  showEyelids?: boolean
}

interface EyeProps {
  eyeSize: number
  irisColor: string
  irisColorSecondary: string
  pupilColor: string
  scleraColor: string
  pupilRange: number
  showReflection: boolean
  showIrisDetail: boolean
  blinkInterval: number
  variant: "realistic" | "cartoon" | "minimal" | "cyber"
  reactivePupil: boolean
  showEyelids: boolean
  mouseX: React.MutableRefObject<number>
  mouseY: React.MutableRefObject<number>
  index: number
}

function Eye({
  eyeSize,
  irisColor,
  irisColorSecondary,
  pupilColor,
  scleraColor,
  pupilRange,
  showReflection,
  showIrisDetail,
  blinkInterval,
  variant,
  reactivePupil,
  showEyelids,
  mouseX,
  mouseY,
  index,
}: EyeProps) {
  const eyeRef = React.useRef<HTMLDivElement>(null)
  const [isBlinking, setIsBlinking] = React.useState(false)
  const [pupilScale, setPupilScale] = React.useState(1)

  const x = useMotionValue(0)
  const y = useMotionValue(0)
  const springX = useSpring(x, { stiffness: 300, damping: 25, mass: 0.5 })
  const springY = useSpring(y, { stiffness: 300, damping: 25, mass: 0.5 })

  const irisSize = eyeSize * 0.45
  const pupilSize = irisSize * 0.5
  const maxOffset = (eyeSize / 2 - irisSize / 2) * pupilRange

  // Blink animation
  React.useEffect(() => {
    if (blinkInterval <= 0) return

    const blink = () => {
      setIsBlinking(true)
      setTimeout(() => setIsBlinking(false), 150)
    }

    // Random offset per eye so they don't all blink at the exact same time
    const randomOffset = Math.random() * 200
    const timeout = setTimeout(() => {
      blink()
      const interval = setInterval(blink, blinkInterval + Math.random() * 1000)
      return () => clearInterval(interval)
    }, randomOffset)

    const interval = setInterval(blink, blinkInterval + Math.random() * 1000)
    return () => {
      clearTimeout(timeout)
      clearInterval(interval)
    }
  }, [blinkInterval])

  // Track mouse position and update pupil
  React.useEffect(() => {
    let animFrame: number

    const update = () => {
      if (!eyeRef.current) {
        animFrame = requestAnimationFrame(update)
        return
      }

      const rect = eyeRef.current.getBoundingClientRect()
      const eyeCenterX = rect.left + rect.width / 2
      const eyeCenterY = rect.top + rect.height / 2

      const dx = mouseX.current - eyeCenterX
      const dy = mouseY.current - eyeCenterY
      const distance = Math.sqrt(dx * dx + dy * dy)
      const angle = Math.atan2(dy, dx)

      const clampedDistance = Math.min(distance, maxOffset * 3)
      const normalizedDistance = clampedDistance / (maxOffset * 3)
      const offset = normalizedDistance * maxOffset

      x.set(Math.cos(angle) * offset)
      y.set(Math.sin(angle) * offset)

      // Reactive pupil dilation based on distance
      if (reactivePupil) {
        const proximityScale = distance < 200 ? 1.3 - (distance / 200) * 0.3 : 0.85 + (Math.min(distance, 800) / 800) * 0.15
        setPupilScale(proximityScale)
      }

      animFrame = requestAnimationFrame(update)
    }

    animFrame = requestAnimationFrame(update)
    return () => cancelAnimationFrame(animFrame)
  }, [x, y, maxOffset, reactivePupil, mouseX, mouseY])

  // Rotation transform for iris detail
  const irisRotation = useTransform(springX, [-maxOffset, maxOffset], [-15, 15])

  const eyeAspect = variant === "cartoon" ? 1 : 0.85
  const borderRadius = variant === "minimal" ? "50%" : variant === "cartoon" ? "50%" : "50%"

  const scleraGradient =
    variant === "realistic"
      ? `radial-gradient(circle at 35% 35%, ${scleraColor} 0%, ${scleraColor}ee 60%, ${scleraColor}cc 100%)`
      : variant === "cyber"
        ? `radial-gradient(circle at 50% 50%, #0a0a1a 0%, #111128 100%)`
        : scleraColor

  const eyeWidth = eyeSize
  const eyeHeight = eyeSize * eyeAspect

  return (
    <motion.div
      ref={eyeRef}
      className={cn(
        "relative overflow-hidden",
        variant === "cyber" && "border border-cyan-500/30"
      )}
      style={{
        width: eyeWidth,
        height: eyeHeight,
        borderRadius,
        background: scleraGradient,
        boxShadow:
          variant === "realistic"
            ? "inset 0 2px 8px rgba(0,0,0,0.15), inset 0 -1px 4px rgba(0,0,0,0.05), 0 4px 20px rgba(0,0,0,0.1)"
            : variant === "cyber"
              ? "inset 0 0 30px rgba(0,200,255,0.1), 0 0 20px rgba(0,200,255,0.15)"
              : variant === "cartoon"
                ? "inset 0 4px 12px rgba(0,0,0,0.1), 0 6px 24px rgba(0,0,0,0.15)"
                : "0 2px 10px rgba(0,0,0,0.1)",
      }}
      animate={{
        scaleY: isBlinking ? 0.05 : 1,
      }}
      transition={{
        scaleY: { duration: 0.1, ease: "easeInOut" },
      }}
    >
      {/* Blood vessel details for realistic variant */}
      {variant === "realistic" && (
        <div className="absolute inset-0 overflow-hidden rounded-full opacity-[0.07]">
          {[...Array(6)].map((_, i) => (
            <div
              key={i}
              className="absolute bg-red-500"
              style={{
                width: "1px",
                height: eyeSize * 0.4,
                left: `${20 + i * 12}%`,
                top: `${10 + (i % 3) * 15}%`,
                transform: `rotate(${-30 + i * 20}deg)`,
                opacity: 0.3 + Math.random() * 0.4,
              }}
            />
          ))}
        </div>
      )}

      {/* Iris */}
      <motion.div
        className="absolute"
        style={{
          width: irisSize,
          height: irisSize,
          borderRadius: "50%",
          left: eyeWidth / 2 - irisSize / 2,
          top: eyeHeight / 2 - irisSize / 2,
          x: springX,
          y: springY,
          background:
            variant === "cyber"
              ? `conic-gradient(from 0deg, ${irisColor}, ${irisColorSecondary}, ${irisColor})`
              : `radial-gradient(circle at 40% 40%, ${irisColorSecondary}, ${irisColor} 60%, ${irisColor}dd 100%)`,
          boxShadow:
            variant === "realistic"
              ? `inset 0 2px 6px rgba(0,0,0,0.3), 0 0 0 1px rgba(0,0,0,0.1)`
              : variant === "cyber"
                ? `0 0 15px ${irisColor}66, inset 0 0 10px ${irisColor}33`
                : `inset 0 1px 4px rgba(0,0,0,0.2)`,
        }}
      >
        {/* Iris detail pattern */}
        {showIrisDetail && (
          <motion.div
            className="absolute inset-0 rounded-full overflow-hidden"
            style={{ rotate: irisRotation }}
          >
            {variant === "cyber" ? (
              // Cyber circuit pattern
              <>
                <div
                  className="absolute inset-[15%] rounded-full border border-dashed opacity-40"
                  style={{ borderColor: irisColor }}
                />
                <div
                  className="absolute inset-[30%] rounded-full border opacity-30"
                  style={{ borderColor: irisColorSecondary }}
                />
                {[...Array(8)].map((_, i) => (
                  <div
                    key={i}
                    className="absolute left-1/2 top-1/2 origin-left opacity-25"
                    style={{
                      width: irisSize * 0.45,
                      height: "1px",
                      background: irisColor,
                      transform: `rotate(${i * 45}deg)`,
                    }}
                  />
                ))}
              </>
            ) : (
              // Realistic iris fibers
              <>
                {[...Array(24)].map((_, i) => (
                  <div
                    key={i}
                    className="absolute left-1/2 top-1/2 origin-left"
                    style={{
                      width: irisSize * 0.45,
                      height: "1px",
                      background: `linear-gradient(to right, transparent 20%, ${irisColor}44 50%, transparent 80%)`,
                      transform: `rotate(${i * 15}deg)`,
                      opacity: 0.3 + (i % 3) * 0.15,
                    }}
                  />
                ))}
                <div
                  className="absolute inset-[20%] rounded-full"
                  style={{
                    border: `1px solid ${irisColor}33`,
                  }}
                />
              </>
            )}
          </motion.div>
        )}

        {/* Pupil */}
        <motion.div
          className="absolute rounded-full"
          style={{
            width: pupilSize,
            height: pupilSize,
            left: irisSize / 2 - pupilSize / 2,
            top: irisSize / 2 - pupilSize / 2,
            background:
              variant === "cyber"
                ? `radial-gradient(circle, ${pupilColor} 40%, transparent 100%)`
                : pupilColor,
            boxShadow:
              variant === "cyber"
                ? `0 0 10px ${irisColor}88`
                : undefined,
          }}
          animate={{
            scale: reactivePupil ? pupilScale : 1,
          }}
          transition={{ duration: 0.3, ease: "easeOut" }}
        />

        {/* Primary light reflection */}
        {showReflection && (
          <>
            <div
              className="absolute rounded-full"
              style={{
                width: pupilSize * 0.35,
                height: pupilSize * 0.35,
                left: irisSize * 0.3,
                top: irisSize * 0.25,
                background:
                  variant === "cyber"
                    ? `radial-gradient(circle, rgba(0,255,255,0.9), transparent)`
                    : "radial-gradient(circle, rgba(255,255,255,0.95), rgba(255,255,255,0.6))",
                filter: "blur(0.5px)",
              }}
            />
            {/* Secondary smaller reflection */}
            <div
              className="absolute rounded-full"
              style={{
                width: pupilSize * 0.15,
                height: pupilSize * 0.15,
                left: irisSize * 0.58,
                top: irisSize * 0.6,
                background:
                  variant === "cyber"
                    ? "rgba(0,255,255,0.5)"
                    : "rgba(255,255,255,0.7)",
              }}
            />
          </>
        )}
      </motion.div>

      {/* Top eyelid shadow */}
      {showEyelids && variant !== "minimal" && (
        <>
          <div
            className="pointer-events-none absolute inset-x-0 top-0"
            style={{
              height: eyeHeight * 0.35,
              background:
                variant === "cyber"
                  ? "linear-gradient(to bottom, rgba(0,10,30,0.6) 0%, transparent 100%)"
                  : "linear-gradient(to bottom, rgba(0,0,0,0.08) 0%, transparent 100%)",
              borderRadius: "50% 50% 0 0",
            }}
          />
          <div
            className="pointer-events-none absolute inset-x-0 bottom-0"
            style={{
              height: eyeHeight * 0.2,
              background:
                variant === "cyber"
                  ? "linear-gradient(to top, rgba(0,10,30,0.4) 0%, transparent 100%)"
                  : "linear-gradient(to top, rgba(0,0,0,0.04) 0%, transparent 100%)",
              borderRadius: "0 0 50% 50%",
            }}
          />
        </>
      )}

      {/* Cyber scan line */}
      {variant === "cyber" && (
        <motion.div
          className="pointer-events-none absolute inset-x-0 h-[2px]"
          style={{
            background: `linear-gradient(to right, transparent, ${irisColor}44, transparent)`,
          }}
          animate={{
            top: [0, eyeHeight, 0],
          }}
          transition={{
            duration: 3,
            repeat: Infinity,
            ease: "linear",
          }}
        />
      )}
    </motion.div>
  )
}

export function EyeTracking({
  className,
  eyeSize = 120,
  gap = 40,
  irisColor = "#4A6741",
  irisColorSecondary = "#6B8F62",
  pupilColor = "#0a0a0a",
  scleraColor = "#F5F0EB",
  pupilRange = 0.7,
  showReflection = true,
  showIrisDetail = true,
  idleAnimation = true,
  blinkInterval = 4000,
  eyeCount = 2,
  variant = "realistic",
  reactivePupil = true,
  showEyelids = true,
}: EyeTrackingProps) {
  const mouseX = React.useRef(typeof window !== "undefined" ? window.innerWidth / 2 : 0)
  const mouseY = React.useRef(typeof window !== "undefined" ? window.innerHeight / 2 : 0)
  const [isMounted, setIsMounted] = React.useState(false)

  // Default colors per variant
  const resolvedIrisColor =
    variant === "cyber" ? (irisColor === "#4A6741" ? "#00d4ff" : irisColor) : irisColor
  const resolvedIrisSecondary =
    variant === "cyber"
      ? irisColorSecondary === "#6B8F62"
        ? "#0088ff"
        : irisColorSecondary
      : irisColorSecondary
  const resolvedPupilColor =
    variant === "cyber" ? (pupilColor === "#0a0a0a" ? "#001122" : pupilColor) : pupilColor
  const resolvedScleraColor =
    variant === "cyber" ? (scleraColor === "#F5F0EB" ? "#0a0a1a" : scleraColor) : scleraColor

  React.useEffect(() => {
    setIsMounted(true)
  }, [])

  // Global mouse tracker
  React.useEffect(() => {
    const handleMouseMove = (e: MouseEvent) => {
      mouseX.current = e.clientX
      mouseY.current = e.clientY
    }

    const handleTouchMove = (e: TouchEvent) => {
      if (e.touches[0]) {
        mouseX.current = e.touches[0].clientX
        mouseY.current = e.touches[0].clientY
      }
    }

    window.addEventListener("mousemove", handleMouseMove, { passive: true })
    window.addEventListener("touchmove", handleTouchMove, { passive: true })
    return () => {
      window.removeEventListener("mousemove", handleMouseMove)
      window.removeEventListener("touchmove", handleTouchMove)
    }
  }, [])

  // Idle animation - subtle random eye movement when no cursor activity
  React.useEffect(() => {
    if (!idleAnimation) return

    let idleTimeout: ReturnType<typeof setTimeout>
    let idleInterval: ReturnType<typeof setInterval>
    let lastX = mouseX.current
    let lastY = mouseY.current

    const checkIdle = () => {
      if (mouseX.current === lastX && mouseY.current === lastY) {
        // Start idle micro-movements
        idleInterval = setInterval(() => {
          const currentX = mouseX.current
          const currentY = mouseY.current
          mouseX.current = currentX + (Math.random() - 0.5) * 30
          mouseY.current = currentY + (Math.random() - 0.5) * 30
          // Restore after brief moment
          setTimeout(() => {
            mouseX.current = currentX
            mouseY.current = currentY
          }, 500)
        }, 2000)
      }
      lastX = mouseX.current
      lastY = mouseY.current
    }

    idleTimeout = setInterval(checkIdle, 3000)

    return () => {
      clearInterval(idleTimeout)
      clearInterval(idleInterval)
    }
  }, [idleAnimation])

  if (!isMounted) {
    return (
      <div
        className={cn("flex items-center justify-center", className)}
        style={{ gap }}
      >
        {[...Array(eyeCount)].map((_, i) => (
          <div
            key={i}
            className="rounded-full bg-neutral-200 dark:bg-neutral-800"
            style={{ width: eyeSize, height: eyeSize * 0.85 }}
          />
        ))}
      </div>
    )
  }

  return (
    <div
      className={cn("flex items-center justify-center", className)}
      style={{ gap }}
    >
      {[...Array(eyeCount)].map((_, i) => (
        <Eye
          key={i}
          index={i}
          eyeSize={eyeSize}
          irisColor={resolvedIrisColor}
          irisColorSecondary={resolvedIrisSecondary}
          pupilColor={resolvedPupilColor}
          scleraColor={resolvedScleraColor}
          pupilRange={pupilRange}
          showReflection={showReflection}
          showIrisDetail={showIrisDetail}
          blinkInterval={blinkInterval}
          variant={variant}
          reactivePupil={reactivePupil}
          showEyelids={showEyelids}
          mouseX={mouseX}
          mouseY={mouseY}
        />
      ))}
    </div>
  )
}
```
---

## Flight Status Card <a name="flight-status-card"></a>

A detailed flight status widget with dot-matrix airport codes, progress tracking, and ETA information.

- **Registry Key**: `flight-status-card`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/flight-status-card.json
```

#### Dependencies
- **NPM Packages**:
  - `framer-motion`

#### Component Source Code
##### File: `components/ui/flight-status-card.tsx`
```tsx
"use client"

import * as React from "react"
import { cn } from "@/lib/utils"
import { motion } from "framer-motion"

const DOT_MATRIX: Record<string, number[][]> = {
  Y: [[1,0,0,0,1],[0,1,0,1,0],[0,0,1,0,0],[0,0,1,0,0],[0,0,1,0,0],[0,0,1,0,0],[0,0,1,0,0]],
  Z: [[1,1,1,1,1],[0,0,0,1,0],[0,0,1,0,0],[0,1,0,0,0],[1,0,0,0,0],[1,0,0,0,0],[1,1,1,1,1]],
  H: [[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[1,1,1,1,1],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1]],
  N: [[1,0,0,0,1],[1,1,0,0,1],[1,0,1,0,1],[1,0,0,1,1],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1]],
  D: [[1,1,1,1,0],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[1,1,1,1,0]],
  A: [[0,0,1,0,0],[0,1,0,1,0],[1,0,0,0,1],[1,1,1,1,1],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1]],
  B: [[1,1,1,1,0],[1,0,0,0,1],[1,0,0,0,1],[1,1,1,1,0],[1,0,0,0,1],[1,0,0,0,1],[1,1,1,1,0]],
  C: [[0,1,1,1,0],[1,0,0,0,1],[1,0,0,0,0],[1,0,0,0,0],[1,0,0,0,0],[1,0,0,0,1],[0,1,1,1,0]],
  E: [[1,1,1,1,1],[1,0,0,0,0],[1,0,0,0,0],[1,1,1,1,0],[1,0,0,0,0],[1,0,0,0,0],[1,1,1,1,1]],
  F: [[1,1,1,1,1],[1,0,0,0,0],[1,0,0,0,0],[1,1,1,1,0],[1,0,0,0,0],[1,0,0,0,0],[1,0,0,0,0]],
  G: [[0,1,1,1,0],[1,0,0,0,1],[1,0,0,0,0],[1,0,1,1,1],[1,0,0,0,1],[1,0,0,0,1],[0,1,1,1,0]],
  I: [[1,1,1,1,1],[0,0,1,0,0],[0,0,1,0,0],[0,0,1,0,0],[0,0,1,0,0],[0,0,1,0,0],[1,1,1,1,1]],
  J: [[0,0,1,1,1],[0,0,0,1,0],[0,0,0,1,0],[0,0,0,1,0],[1,0,0,1,0],[1,0,0,1,0],[0,1,1,0,0]],
  K: [[1,0,0,0,1],[1,0,0,1,0],[1,0,1,0,0],[1,1,0,0,0],[1,0,1,0,0],[1,0,0,1,0],[1,0,0,0,1]],
  L: [[1,0,0,0,0],[1,0,0,0,0],[1,0,0,0,0],[1,0,0,0,0],[1,0,0,0,0],[1,0,0,0,0],[1,1,1,1,1]],
  M: [[1,0,0,0,1],[1,1,0,1,1],[1,0,1,0,1],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1]],
  O: [[0,1,1,1,0],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[0,1,1,1,0]],
  P: [[1,1,1,1,0],[1,0,0,0,1],[1,0,0,0,1],[1,1,1,1,0],[1,0,0,0,0],[1,0,0,0,0],[1,0,0,0,0]],
  Q: [[0,1,1,1,0],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[1,0,1,0,1],[1,0,0,1,0],[0,1,1,0,1]],
  R: [[1,1,1,1,0],[1,0,0,0,1],[1,0,0,0,1],[1,1,1,1,0],[1,0,1,0,0],[1,0,0,1,0],[1,0,0,0,1]],
  S: [[0,1,1,1,1],[1,0,0,0,0],[1,0,0,0,0],[0,1,1,1,0],[0,0,0,0,1],[0,0,0,0,1],[1,1,1,1,0]],
  T: [[1,1,1,1,1],[0,0,1,0,0],[0,0,1,0,0],[0,0,1,0,0],[0,0,1,0,0],[0,0,1,0,0],[0,0,1,0,0]],
  U: [[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[0,1,1,1,0]],
  V: [[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[0,1,0,1,0],[0,1,0,1,0],[0,0,1,0,0]],
  W: [[1,0,0,0,1],[1,0,0,0,1],[1,0,0,0,1],[1,0,1,0,1],[1,0,1,0,1],[1,1,0,1,1],[1,0,0,0,1]],
  X: [[1,0,0,0,1],[0,1,0,1,0],[0,0,1,0,0],[0,0,1,0,0],[0,0,1,0,0],[0,1,0,1,0],[1,0,0,0,1]]
}

interface DotMatrixCharProps {
  char: string
  dotSize?: number
  gap?: number
  activeColor?: string
  inactiveColor?: string
  className?: string
  delay?: number
}

function DotMatrixChar({
  char,
  dotSize = 4,
  gap = 2,
  activeColor = "#b4f54e",
  inactiveColor = "rgba(180, 245, 78, 0.1)",
  className,
  delay = 0,
}: DotMatrixCharProps) {
  const matrix = DOT_MATRIX[char.toUpperCase()] ?? DOT_MATRIX["A"]!
  const width = 5 * dotSize + 4 * gap
  const height = 7 * dotSize + 6 * gap

  return (
    <motion.svg
      width={width}
      height={height}
      viewBox={`0 0 ${width} ${height}`}
      className={className}
      initial="hidden"
      animate="visible"
    >
      {matrix!.map((row, rowIndex) =>
        row.map((cell, colIndex) => (
          <motion.rect
            key={`${rowIndex}-${colIndex}`}
            x={colIndex * (dotSize + gap)}
            y={rowIndex * (dotSize + gap)}
            width={dotSize}
            height={dotSize}
            rx={dotSize / 2}
            ry={dotSize / 2}
            fill={cell ? activeColor : inactiveColor}
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            transition={{
              delay: delay + colIndex * 0.05 + rowIndex * 0.05,
              duration: 0.2,
            }}
            style={cell ? { filter: "drop-shadow(0 0 3px rgba(180, 245, 78, 0.6))" } : {}}
          />
        ))
      )}
    </motion.svg>
  )
}

interface DotMatrixTextProps {
  text: string
  dotSize?: number
  gap?: number
  charGap?: number
  activeColor?: string
  inactiveColor?: string
  className?: string
}

function DotMatrixText({
  text,
  dotSize = 4,
  gap = 2,
  charGap = 8,
  activeColor = "#b4f54e",
  inactiveColor = "rgba(180, 245, 78, 0.1)",
  className,
}: DotMatrixTextProps) {
  return (
    <div className={cn("flex items-center", className)} style={{ gap: charGap }}>
      {text.split("").map((char, index) => (
        <DotMatrixChar
          key={index}
          char={char}
          dotSize={dotSize}
          gap={gap}
          activeColor={activeColor}
          inactiveColor={inactiveColor}
          delay={index * 0.1}
        />
      ))}
    </div>
  )
}

function HalftonePattern({ className }: { className?: string }) {
  return (
    <motion.svg
      className={cn("absolute inset-0 pointer-events-none", className)}
      width="100%"
      height="100%"
      xmlns="http://www.w3.org/2000/svg"
      animate={{ backgroundPosition: ["0% 0%", "100% 100%"] }}
      transition={{ duration: 20, repeat: Infinity, ease: "linear" }}
    >
      <defs>
        <pattern id="halftone" x="0" y="0" width="8" height="8" patternUnits="userSpaceOnUse">
          <motion.circle
            cx="2"
            cy="2"
            r="1.2"
            fill="rgba(180, 245, 78, 0.15)"
            animate={{ opacity: [0.3, 0.6, 0.3] }}
            transition={{ duration: 4, repeat: Infinity, ease: "easeInOut" }}
          />
        </pattern>
        <linearGradient id="halftone-fade" x1="0%" y1="0%" x2="100%" y2="0%">
          <stop offset="0%" stopColor="white" stopOpacity="0.8" />
          <stop offset="50%" stopColor="white" stopOpacity="0.3" />
          <stop offset="100%" stopColor="white" stopOpacity="0" />
        </linearGradient>
        <mask id="halftone-mask">
          <rect width="100%" height="100%" fill="url(#halftone-fade)" />
        </mask>
      </defs>
      <rect width="100%" height="100%" fill="url(#halftone)" mask="url(#halftone-mask)" />
    </motion.svg>
  )
}

function PlaneIcon({ className }: { className?: string }) {
  return (
    <svg className={className} viewBox="0 0 24 24" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
      <path d="M21 16v-2l-8-5V3.5c0-.83-.67-1.5-1.5-1.5S10 2.67 10 3.5V9l-8 5v2l8-2.5V19l-2 1.5V22l3.5-1 3.5 1v-1.5L13 19v-5.5l8 2.5z" />
    </svg>
  )
}

function SwapIcon({ className }: { className?: string }) {
  return (
    <svg className={className} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
      <path d="M7 16V4m0 0L3 8m4-4l4 4" />
      <path d="M17 8v12m0 0l4-4m-4 4l-4-4" />
    </svg>
  )
}

interface FlightStatusCardProps {
  departureCode?: string
  arrivalCode?: string
  departureCity?: string
  arrivalCity?: string
  departureTime?: string
  arrivalTime?: string
  eta?: string
  timezone?: string
  nextEvent?: string
  nextEventTime?: string
  progress?: number
  remainingTime?: string
  className?: string
}

function FlightStatusCard({
  departureCode = "YYZ",
  arrivalCode = "HND",
  departureCity = "Toronto",
  arrivalCity = "Tokyo",
  departureTime = "MON, 6:14 PM",
  arrivalTime = "TUE, 7:14 AM",
  eta = "ETA 2:15 PM",
  timezone = "Tokyo Time",
  nextEvent = "DINNER IN",
  nextEventTime = "2:34H",
  progress = 45,
  remainingTime = "-7H 01M",
  className,
}: FlightStatusCardProps) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.6, ease: "easeOut" }}
      className={cn(
        "relative w-full max-w-[480px] rounded-[28px] p-6 overflow-hidden",
        "bg-[#1a1a1a] dark:bg-[#1a1a1a]",
        "shadow-[0_20px_60px_-10px_rgba(0,0,0,0.5),0_10px_30px_-5px_rgba(0,0,0,0.3),inset_0_1px_0_rgba(255,255,255,0.05)]",
        className
      )}
      style={{ background: "linear-gradient(145deg, #1e1e1e 0%, #1a1a1a 50%, #161616 100%)" }}
    >
      <HalftonePattern className="opacity-60" />
      <div className="relative z-10">
        <div className="flex items-start justify-between mb-6">
          <div className="flex items-center gap-4">
            <div className="flex flex-col items-start">
              <DotMatrixText text={departureCode} dotSize={5} gap={2} charGap={6} />
              <motion.span initial={{ opacity: 0, x: -10 }} animate={{ opacity: 1, x: 0 }} transition={{ delay: 0.3 }} className="text-[#8a8a8a] text-sm mt-2 font-medium">{departureCity}</motion.span>
              <motion.span initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ delay: 0.5 }} className="text-[#6a6a6a] text-xs mt-0.5 uppercase tracking-wide">{departureTime}</motion.span>
            </div>
            <div className="flex items-center px-2 mt-1">
              <motion.svg width="24" height="24" viewBox="0 0 24 24" fill="none" className="text-orange-500" initial={{ scaleX: 0, opacity: 0 }} animate={{ scaleX: 1, opacity: 1 }} transition={{ delay: 0.4, type: "spring" }}>
                <path d="M5 12h14m0 0l-4-4m4 4l-4 4" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" />
              </motion.svg>
            </div>
            <div className="flex flex-col items-start">
              <DotMatrixText text={arrivalCode} dotSize={5} gap={2} charGap={6} />
              <motion.span initial={{ opacity: 0, x: 10 }} animate={{ opacity: 1, x: 0 }} transition={{ delay: 0.3 }} className="text-[#8a8a8a] text-sm mt-2 font-medium">{arrivalCity}</motion.span>
              <motion.span initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ delay: 0.5 }} className="text-[#6a6a6a] text-xs mt-0.5 uppercase tracking-wide">{arrivalTime}</motion.span>
            </div>
          </div>
          <motion.div initial={{ opacity: 0, scale: 0.9 }} animate={{ opacity: 1, scale: 1 }} transition={{ delay: 0.6 }} className="flex flex-col bg-[#252525] rounded-xl p-3 min-w-[130px] border border-[#333]">
            <div className="flex items-center justify-between mb-1">
              <span className="text-white text-sm font-semibold">{eta}</span>
              <button className="p-1 hover:bg-[#333] rounded-full transition-colors">
                <SwapIcon className="w-4 h-4 text-[#8a8a8a]" />
              </button>
            </div>
            <span className="text-[#6a6a6a] text-xs">{timezone}</span>
            <span className="text-orange-500 text-xs font-bold mt-1 tracking-wide">{nextEvent} {nextEventTime}</span>
          </motion.div>
        </div>
        <div className="relative mt-4">
          <div className="relative h-12 bg-[#252525] rounded-full overflow-hidden border border-[#333]">
            <motion.div
              className="absolute inset-y-0 left-0 rounded-full flex items-center justify-end pr-2"
              initial={{ width: "0%" }}
              animate={{ width: `${Math.max(progress, 15)}%` }}
              transition={{ duration: 1.5, ease: "circOut", delay: 0.5 }}
              style={{
                background: "linear-gradient(90deg, #7cb518 0%, #a4de02 50%, #b4f54e 100%)",
                boxShadow: "0 0 20px rgba(180, 245, 78, 0.6), 0 0 40px rgba(180, 245, 78, 0.3), inset 0 2px 4px rgba(255,255,255,0.2)",
              }}
            >
              <motion.div
                className="relative flex items-center justify-center w-8 h-8 rounded-full"
                animate={{ y: [0, -2, 0] }}
                transition={{ duration: 2, repeat: Infinity, ease: "easeInOut" }}
                style={{ background: "rgba(255,255,255,0.2)" }}
              >
                <PlaneIcon className="w-5 h-5 text-white transform rotate-45" />
              </motion.div>
            </motion.div>
            <motion.div
              className="absolute inset-y-0 left-0 rounded-full pointer-events-none"
              initial={{ width: "0%" }}
              animate={{ width: `${Math.max(progress, 15)}%` }}
              transition={{ duration: 1.5, ease: "circOut", delay: 0.5 }}
              style={{
                background: "radial-gradient(ellipse at right, rgba(180, 245, 78, 0.4) 0%, transparent 70%)",
                filter: "blur(8px)",
              }}
            />
          </div>
          <motion.div initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ delay: 1.2 }} className="absolute right-4 top-1/2 -translate-y-1/2 text-[#6a6a6a] text-sm font-mono font-medium">{remainingTime}</motion.div>
        </div>
      </div>
    </motion.div>
  )
}

function FlightStatusCardLight({
  departureCode = "YYZ",
  arrivalCode = "HND",
  departureCity = "Toronto",
  arrivalCity = "Tokyo",
  departureTime = "MON, 6:14 PM",
  arrivalTime = "TUE, 7:14 AM",
  eta = "ETA 2:15 PM",
  timezone = "Tokyo Time",
  nextEvent = "DINNER IN",
  nextEventTime = "2:34H",
  progress = 45,
  remainingTime = "-7H 01M",
  className,
}: FlightStatusCardProps) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.6, ease: "easeOut" }}
      className={cn(
        "relative w-full max-w-[480px] rounded-[28px] p-6 overflow-hidden",
        "bg-[#f8f8f8]",
        "shadow-[0_20px_60px_-10px_rgba(0,0,0,0.15),0_10px_30px_-5px_rgba(0,0,0,0.08),inset_0_1px_0_rgba(255,255,255,0.8)]",
        className
      )}
      style={{ background: "linear-gradient(145deg, #ffffff 0%, #f8f8f8 50%, #f0f0f0 100%)" }}
    >
      <HalftoneLightPattern className="opacity-40" />
      <div className="relative z-10">
        <div className="flex items-start justify-between mb-6">
          <div className="flex items-center gap-4">
            <div className="flex flex-col items-start">
              <DotMatrixText text={departureCode} dotSize={5} gap={2} charGap={6} activeColor="#2d7a2d" inactiveColor="rgba(45, 122, 45, 0.15)" />
              <motion.span initial={{ opacity: 0, x: -10 }} animate={{ opacity: 1, x: 0 }} transition={{ delay: 0.3 }} className="text-[#555] text-sm mt-2 font-medium">{departureCity}</motion.span>
              <motion.span initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ delay: 0.5 }} className="text-[#888] text-xs mt-0.5 uppercase tracking-wide">{departureTime}</motion.span>
            </div>
            <div className="flex items-center px-2 mt-1">
              <motion.svg width="24" height="24" viewBox="0 0 24 24" fill="none" className="text-orange-600" initial={{ scaleX: 0, opacity: 0 }} animate={{ scaleX: 1, opacity: 1 }} transition={{ delay: 0.4, type: "spring" }}>
                <path d="M5 12h14m0 0l-4-4m4 4l-4 4" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" />
              </motion.svg>
            </div>
            <div className="flex flex-col items-start">
              <DotMatrixText text={arrivalCode} dotSize={5} gap={2} charGap={6} activeColor="#2d7a2d" inactiveColor="rgba(45, 122, 45, 0.15)" />
              <motion.span initial={{ opacity: 0, x: 10 }} animate={{ opacity: 1, x: 0 }} transition={{ delay: 0.3 }} className="text-[#555] text-sm mt-2 font-medium">{arrivalCity}</motion.span>
              <motion.span initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ delay: 0.5 }} className="text-[#888] text-xs mt-0.5 uppercase tracking-wide">{arrivalTime}</motion.span>
            </div>
          </div>
          <motion.div initial={{ opacity: 0, scale: 0.9 }} animate={{ opacity: 1, scale: 1 }} transition={{ delay: 0.6 }} className="flex flex-col bg-white rounded-xl p-3 min-w-[130px] border border-[#e0e0e0] shadow-sm">
            <div className="flex items-center justify-between mb-1">
              <span className="text-[#222] text-sm font-semibold">{eta}</span>
              <button className="p-1 hover:bg-[#f0f0f0] rounded-full transition-colors">
                <SwapIcon className="w-4 h-4 text-[#666]" />
              </button>
            </div>
            <span className="text-[#888] text-xs">{timezone}</span>
            <span className="text-orange-600 text-xs font-bold mt-1 tracking-wide">{nextEvent} {nextEventTime}</span>
          </motion.div>
        </div>
        <div className="relative mt-4">
          <div className="relative h-12 bg-[#e8e8e8] rounded-full overflow-hidden border border-[#ddd]">
            <motion.div
              className="absolute inset-y-0 left-0 rounded-full flex items-center justify-end pr-2"
              initial={{ width: "0%" }}
              animate={{ width: `${Math.max(progress, 15)}%` }}
              transition={{ duration: 1.5, ease: "circOut", delay: 0.5 }}
              style={{
                background: "linear-gradient(90deg, #4a9c4a 0%, #5cb85c 50%, #7ed17e 100%)",
                boxShadow: "0 0 15px rgba(92, 184, 92, 0.4), inset 0 2px 4px rgba(255,255,255,0.3)",
              }}
            >
              <motion.div
                className="relative flex items-center justify-center w-8 h-8 rounded-full"
                animate={{ y: [0, -2, 0] }}
                transition={{ duration: 2, repeat: Infinity, ease: "easeInOut" }}
                style={{ background: "rgba(255,255,255,0.3)" }}
              >
                <PlaneIcon className="w-5 h-5 text-white transform rotate-45" />
              </motion.div>
            </motion.div>
          </div>
          <motion.div initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ delay: 1.2 }} className="absolute right-4 top-1/2 -translate-y-1/2 text-[#666] text-sm font-mono font-medium">{remainingTime}</motion.div>
        </div>
      </div>
    </motion.div>
  )
}

function HalftoneLightPattern({ className }: { className?: string }) {
  return (
    <svg className={cn("absolute inset-0 pointer-events-none", className)} width="100%" height="100%" xmlns="http://www.w3.org/2000/svg">
      <defs>
        <pattern id="halftone-light" x="0" y="0" width="8" height="8" patternUnits="userSpaceOnUse">
          <circle cx="2" cy="2" r="1.2" fill="rgba(45, 122, 45, 0.12)" />
        </pattern>
        <linearGradient id="halftone-fade-light" x1="0%" y1="0%" x2="100%" y2="0%">
          <stop offset="0%" stopColor="white" stopOpacity="1" />
          <stop offset="50%" stopColor="white" stopOpacity="0.5" />
          <stop offset="100%" stopColor="white" stopOpacity="0" />
        </linearGradient>
        <mask id="halftone-mask-light">
          <rect width="100%" height="100%" fill="url(#halftone-fade-light)" />
        </mask>
      </defs>
      <rect width="100%" height="100%" fill="url(#halftone-light)" mask="url(#halftone-mask-light)" />
    </svg>
  )
}

function FlightStatusCardAdaptive(props: FlightStatusCardProps) {
  return (
    <>
      <div className="hidden dark:block">
        <FlightStatusCard {...props} />
      </div>
      <div className="block dark:hidden">
        <FlightStatusCardLight {...props} />
      </div>
    </>
  )
}

export { FlightStatusCard, FlightStatusCardLight, FlightStatusCardAdaptive, DotMatrixText, DotMatrixChar }
```
---

## Github Calendar <a name="github-calendar"></a>

A premium, customizable visualization of GitHub contribution graphs with variants and scaling support.

- **Registry Key**: `github-calendar`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/github-calendar.json
```

#### Dependencies
- **NPM Packages**:
  - `framer-motion`

#### Component Source Code
##### File: `components/ui/github-calendar.tsx`
```tsx
"use client"

import * as React from "react"
import { motion, AnimatePresence } from "framer-motion"
import { cn } from "@/lib/utils"

interface ContributionDay {
    color: string
    contributionCount: number
    contributionLevel: "NONE" | "FIRST_QUARTILE" | "SECOND_QUARTILE" | "THIRD_QUARTILE" | "FOURTH_QUARTILE"
    date: string
}

interface GithubContributionData {
    contributions: ContributionDay[][]
    totalContributions: number
}

interface GithubCalendarProps {
    username: string
    variant?: "default" | "city-lights" | "minimal"
    shape?: "square" | "rounded" | "circle" | "squircle"
    glowIntensity?: number
    className?: string
    showTotal?: boolean
    colorSchema?: "green" | "blue" | "purple" | "orange" | "gray"
}

// Color schemas for custom styling
const colorSchemas = {
    gray: {
        level0: "bg-zinc-100 dark:bg-zinc-900",
        level1: "bg-zinc-300 dark:bg-zinc-800",
        level2: "bg-zinc-400 dark:bg-zinc-700",
        level3: "bg-zinc-600 dark:bg-zinc-500",
        level4: "bg-zinc-800 dark:bg-zinc-300",
    },
    green: {
        level0: "bg-zinc-100 dark:bg-zinc-900",
        level1: "bg-emerald-200 dark:bg-emerald-900",
        level2: "bg-emerald-300 dark:bg-emerald-700",
        level3: "bg-emerald-400 dark:bg-emerald-500",
        level4: "bg-emerald-500 dark:bg-emerald-400",
    },
    blue: {
        level0: "bg-zinc-100 dark:bg-zinc-900",
        level1: "bg-blue-200 dark:bg-blue-900",
        level2: "bg-blue-300 dark:bg-blue-700",
        level3: "bg-blue-400 dark:bg-blue-500",
        level4: "bg-blue-500 dark:bg-blue-400",
    },
    purple: {
        level0: "bg-zinc-100 dark:bg-zinc-900",
        level1: "bg-purple-200 dark:bg-purple-900",
        level2: "bg-purple-300 dark:bg-purple-700",
        level3: "bg-purple-400 dark:bg-purple-500",
        level4: "bg-purple-500 dark:bg-purple-400",
    },
    orange: {
        level0: "bg-zinc-100 dark:bg-zinc-900",
        level1: "bg-orange-200 dark:bg-orange-900",
        level2: "bg-orange-300 dark:bg-orange-700",
        level3: "bg-orange-400 dark:bg-orange-500",
        level4: "bg-orange-500 dark:bg-orange-400",
    },
}

function getLevelClass(level: string, schema: keyof typeof colorSchemas = "green") {
    const s = colorSchemas[schema]
    switch (level) {
        case "FIRST_QUARTILE":
            return s.level1
        case "SECOND_QUARTILE":
            return s.level2
        case "THIRD_QUARTILE":
            return s.level3
        case "FOURTH_QUARTILE":
            return s.level4
        case "NONE":
        default:
            return s.level0
    }
}

function getShapeClass(shape: string) {
    switch (shape) {
        case "circle":
            return "rounded-full"
        case "square":
            return "rounded-none"
        case "squircle":
            return "rounded-sm" // Approximation
        case "rounded":
        default:
            return "rounded-[2px]"
    }
}

export function GithubCalendar({
    username,
    variant = "default",
    shape = "rounded",
    glowIntensity = 5,
    className,
    showTotal = true,
    colorSchema = "green",
}: GithubCalendarProps) {
    const [data, setData] = React.useState<GithubContributionData | null>(null)
    const [loading, setLoading] = React.useState(true)
    const [error, setError] = React.useState<string | null>(null)
    const [hoveredDate, setHoveredDate] = React.useState<string | null>(null)
    const [hoveredCount, setHoveredCount] = React.useState<number | null>(null)
    const [mousePos, setMousePos] = React.useState({ x: 0, y: 0 })

    React.useEffect(() => {
        const fetchData = async () => {
            try {
                setLoading(true)
                const response = await fetch(
                    `https://github-contributions-api.deno.dev/${username}.json`
                )
                if (!response.ok) {
                    throw new Error("Failed to fetch GitHub data")
                }
                const jsonData = await response.json()
                setData(jsonData)
            } catch (err) {
                setError(err instanceof Error ? err.message : "An error occurred")
            } finally {
                setLoading(false)
            }
        }

        if (username) {
            fetchData()
        }
    }, [username])

    if (error) {
        return (
            <div className={cn("p-4 rounded-lg border border-red-200 bg-red-50 text-red-500 text-sm", className)}>
                Error: {error}
            </div>
        )
    }

    if (loading) {
        return (
            <div className={cn("w-full h-32 animate-pulse bg-zinc-100 dark:bg-zinc-800 rounded-xl", className)} />
        )
    }

    const weeks = data?.contributions || []

    return (
        <div className={cn("w-max max-w-full flex flex-col gap-4", className)}>
            {showTotal && (
                <div className="flex items-center justify-between">
                    <div className="flex items-center gap-2">
                        <svg height="16" aria-hidden="true" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" className="fill-current text-muted-foreground">
                            <path d="M8 0c4.42 0 8 3.58 8 8a8.013 8.013 0 0 1-5.45 7.59c-.4.08-.55-.17-.55-.38 0-.27.01-1.13.01-2.2 0-.75-.25-1.23-.54-1.48 1.78-.2 3.65-.88 3.65-3.95 0-.88-.31-1.59-.82-2.15.08-.2.36-1.02-.08-2.12 0 0-.67-.22-2.2.82-.64-.18-1.32-.27-2-.27-.68 0-1.36.09-2 .27-1.53-1.03-2.2-.82-2.2-.82-.44 1.1-.16 1.92-.08 2.12-.51.56-.82 1.28-.82 2.15 0 3.06 1.86 3.75 3.64 3.95-.23.2-.44.55-.51 1.07-.46.21-1.61.55-2.33-.66-.15-.24-.6-.83-1.23-.82-.67.01-.27.38.01.53.34.19.73.9.82 1.13.16.45.68 1.31 2.69.94 0 .67.01 1.3.01 1.49 0 .21-.15.45-.55.38A7.995 7.995 0 0 1 0 8c0-4.42 3.58-8 8-8Z"></path>
                        </svg>
                        <span className="font-semibold text-sm">@{username}</span>
                    </div>
                    <span className="text-sm text-muted-foreground">
                        {data?.totalContributions} contributions in the last year
                    </span>
                </div>
            )}

            <div
                className="relative flex flex-nowrap gap-[3px] w-max max-w-full"
                onMouseLeave={() => {
                    setHoveredDate(null)
                    setHoveredCount(null)
                }}
            >
                {/* Simple Tooltip */}
                <AnimatePresence>
                    {hoveredDate && (
                        <motion.div
                            initial={{ opacity: 0, y: 10, scale: 0.9 }}
                            animate={{ opacity: 1, y: 0, scale: 1 }}
                            exit={{ opacity: 0, y: 5, scale: 0.9 }}
                            transition={{ duration: 0.2 }}
                            className="absolute z-50 pointer-events-none px-3 py-1.5 bg-zinc-900 text-white dark:bg-white dark:text-zinc-900 text-xs rounded-md shadow-xl whitespace-nowrap"
                            style={{
                                left: mousePos.x,
                                top: mousePos.y - 40,
                                transform: "translateX(-50%)"
                            }}
                        >
                            <span className="font-bold mr-1">{hoveredCount}</span>
                            <span className="text-zinc-400 dark:text-zinc-500">contributions on {hoveredDate}</span>
                        </motion.div>
                    )}
                </AnimatePresence>

                {weeks.map((week, weekIndex) => (
                    <div key={weekIndex} className="flex flex-col gap-[3px] w-[14px]">
                        {week.map((day, dayIndex) => {
                            const isGlowing = variant === "city-lights" && day.contributionCount > 0;
                            const isMinimal = variant === "minimal";
                            const shapeClass = getShapeClass(shape);

                            return (
                                <motion.div
                                    key={day.date}
                                    initial={{ opacity: 0, scale: 0 }}
                                    animate={{ opacity: 1, scale: 1 }}
                                    transition={{
                                        delay: weekIndex * 0.01 + dayIndex * 0.01,
                                        type: "spring",
                                        stiffness: 260,
                                        damping: 20
                                    }}
                                    onMouseEnter={(e) => {
                                        setHoveredDate(day.date)
                                        setHoveredCount(day.contributionCount)
                                        const rect = e.currentTarget.getBoundingClientRect()
                                        const parentRect = e.currentTarget.offsetParent!.getBoundingClientRect()
                                        setMousePos({
                                            x: rect.left - parentRect.left + rect.width / 2,
                                            y: rect.top - parentRect.top
                                        })
                                    }}
                                    className={cn(
                                        "w-full aspect-square transition-colors duration-200",
                                        getLevelClass(day.contributionLevel, colorSchema),
                                        isGlowing && "z-10",
                                        shapeClass,
                                        isMinimal && "rounded-full scale-75",
                                    )}
                                    style={
                                        isGlowing ? {
                                            boxShadow: day.contributionLevel !== "NONE"
                                                ? `0 0 ${day.contributionCount > 3 ? `${glowIntensity * 1.5}px` : `${glowIntensity}px`} ${colorSchema === 'green' ? '#10b981' :
                                                    colorSchema === 'blue' ? '#3b82f6' :
                                                        colorSchema === 'purple' ? '#a855f7' :
                                                            '#f97316'
                                                }`
                                                : 'none'
                                        } : undefined
                                    }
                                />
                            )
                        })}
                    </div>
                ))}
            </div>
        </div>
    )
}
```
---

## Hero Geometric <a name="hero-geometric"></a>

A geometric hero section with a shader background and premium typography.

- **Registry Key**: `hero-geometric`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/hero-geometric.json
```

#### Dependencies
- **NPM Packages**:
  - `framer-motion`
  - `lucide-react`
  - `three`
  - `@react-three/fiber`
  - `@react-three/drei`

#### How to Use
```tsx
"use client";

import HeroGeometric from "@workspace/ui/components/hero-geometric";
import { cn } from "@/lib/utils";
import { Instrument_Serif } from "next/font/google";

const instrumentSerif = Instrument_Serif({
  subsets: ["latin"],
  weight: "400",
  style: ["normal", "italic"],
  variable: "--font-serif",
});

interface HeroGeometricPreviewProps {
  title1?: string;
  title2?: string;
  description?: string;
  color1?: string;
  color2?: string;
  speed?: number;
}

function HeroGeometricPreviewWrapper({
  title1 = "Elevate",
  title2 = "Your Brand",
  description,
  color1,
  color2,
  speed,
}: HeroGeometricPreviewProps) {
  return (
    <div className="relative h-full w-full">
      <div
        className={cn(
          "h-full w-full overflow-hidden relative group",
          instrumentSerif.variable,
        )}
      >
        <HeroGeometric
          title1={title1}
          title2={title2}
          description={description}
          color1={color1}
          color2={color2}
          speed={speed}
          className="!min-h-full h-full"
          style={{ minHeight: "100%" }}
        />
      </div>
    </div>
  );
}

export function HeroGeometricDemo() {
  return (
    <HeroGeometricPreviewWrapper
      title1="Elevate"
      title2="Your Brand"
    // description="Scale your SaaS in minutes"
    />
  );
}

export function HeroGeometricWarmDemo() {
  return (
    <HeroGeometricPreviewWrapper
      color1="#EA580C"
      color2="#FFF7ED"
      title1="Warm"
      title2="Palette"
    />
  );
}

export function HeroGeometricSpeedDemo() {
  return (
    <HeroGeometricPreviewWrapper speed={2} title1="High" title2="Velocity" />
  );
}
```
#### Component Source Code
##### File: `components/ui/hero-geometric.tsx`
```tsx
"use client";

/* eslint-disable react/no-unknown-property */
import { useRef, useMemo } from "react";
import { Canvas, useFrame, ThreeElements } from "@react-three/fiber";
import * as THREE from "three";
import { motion } from "framer-motion";

import { cn } from "../lib/utils";

/* eslint-disable @typescript-eslint/no-namespace */
declare module "react" {
    namespace JSX {
        // eslint-disable-next-line @typescript-eslint/no-empty-interface
        interface IntrinsicElements extends ThreeElements { }
    }
}
/* eslint-enable @typescript-eslint/no-namespace */

// --- Shader Code ---
const vertexShader = `
varying vec2 vUv;
void main() {
  vUv = uv;
  gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
}
`;

const fragmentShader = `
uniform float uTime;
uniform vec2 uResolution;
uniform vec3 uColor1;
uniform vec3 uColor2;
varying vec2 vUv;

vec3 permute(vec3 x) { return mod(((x*34.0)+1.0)*x, 289.0); }

float snoise(vec2 v){
  const vec4 C = vec4(0.211324865405187, 0.366025403784439,
           -0.577350269189626, 0.024390243902439);
  vec2 i  = floor(v + dot(v, C.yy) );
  vec2 x0 = v -   i + dot(i, C.xx);
  vec2 i1;
  i1 = (x0.x > x0.y) ? vec2(1.0, 0.0) : vec2(0.0, 1.0);
  vec4 x12 = x0.xyxy + C.xxzz;
  x12.xy -= i1;
  i = mod(i, 289.0);
  vec3 p = permute( permute( i.y + vec3(0.0, i1.y, 1.0 ))
  + i.x + vec3(0.0, i1.x, 1.0 ));
  vec3 m = max(0.5 - vec3(dot(x0,x0), dot(x12.xy,x12.xy), dot(x12.zw,x12.zw)), 0.0);
  m = m*m ;
  m = m*m ;
  vec3 x = 2.0 * fract(p * C.www) - 1.0;
  vec3 h = abs(x) - 0.5;
  vec3 ox = floor(x + 0.5);
  vec3 a0 = x - ox;
  m *= 1.79284291400159 - 0.85373472095314 * ( a0*a0 + h*h );
  vec3 g;
  g.x  = a0.x  * x0.x  + h.x  * x0.y;
  g.yz = a0.yz * x12.xz + h.yz * x12.yw;
  return 130.0 * dot(m, g);
}

float bayerDither4x4(vec2 uv) {
    int x = int(mod(uv.x, 4.0));
    int y = int(mod(uv.y, 4.0));
    
    int matrix[16];
    matrix[0] = 0; matrix[1] = 8; matrix[2] = 2; matrix[3] = 10;
    matrix[4] = 12; matrix[5] = 4; matrix[6] = 14; matrix[7] = 6;
    matrix[8] = 3; matrix[9] = 11; matrix[10] = 1; matrix[11] = 9;
    matrix[12] = 15; matrix[13] = 7; matrix[14] = 13; matrix[15] = 5;
    
    return float(matrix[y * 4 + x]) / 16.0;
}

void main() {
    vec2 uv = vUv;
    vec2 coord = gl_FragCoord.xy;
    
    // Enhanced noise with time
    float noise = snoise(uv * 1.5 + vec2(uTime * 0.05, uTime * 0.03)) * 0.25;
    
    // Diagonal gradient from bottom-left to top-right
    float diagonal = (uv.x + uv.y) * 0.5;
    
    // Combine for gradient - emphasize corners
    float gradient = diagonal * 1.2 + noise;
    
    // Interpolate colors based on gradient
    vec3 deepBlue = uColor1;
    vec3 paleBlue = uColor2;
    vec3 softBlue = mix(deepBlue, paleBlue, 0.33);
    vec3 lightBlue = mix(deepBlue, paleBlue, 0.66);
    
    // Map to colors with more distinct steps
    vec3 color;
    if (gradient < 0.3) {
        color = deepBlue;
    } else if (gradient < 0.55) {
        color = softBlue;
    } else if (gradient < 0.8) {
        color = lightBlue;
    } else {
        color = paleBlue;
    }
    
    // Enhanced dithering at boundaries
    float dither = bayerDither4x4(coord);
    float threshold = fract(gradient * 4.0);
    
    if (gradient < 0.3 && threshold > dither * 0.5) {
        color = softBlue;
    } else if (gradient >= 0.3 && gradient < 0.55 && threshold > dither * 0.5) {
        color = lightBlue;
    } else if (gradient >= 0.55 && gradient < 0.8 && threshold > dither * 0.5) {
        color = paleBlue;
    }
    
    // Softer fade to white - only at extreme bottom-left
    vec2 cornerDist = vec2(uv.x, uv.y);
    float fadeMask = smoothstep(0.0, 0.25, length(cornerDist));
    color = mix(vec3(1.0), color, fadeMask);
    
    // Add subtle vignette to emphasize corners
    float vignette = smoothstep(1.2, 0.3, length(uv - 0.5));
    color = mix(color, color * 0.95, (1.0 - vignette) * 0.3);
    
    gl_FragColor = vec4(color, 1.0);
}
`;

const GradientPlane = ({
    color1,
    color2,
    speed = 1
}: {
    color1: string;
    color2: string;
    speed?: number
}) => {
    const meshRef = useRef<THREE.Mesh>(null);
    const uniforms = useMemo(
        () => ({
            uTime: { value: 0 },
            uResolution: { value: new THREE.Vector2(1000, 1000) },
            uColor1: { value: new THREE.Color(color1) },
            uColor2: { value: new THREE.Color(color2) },
        }),
        [color1, color2]
    );

    useFrame((state) => {
        const { clock, size } = state;
        uniforms.uTime.value = clock.getElapsedTime() * speed;
        uniforms.uResolution.value.set(size.width, size.height);
        uniforms.uColor1.value.set(color1);
        uniforms.uColor2.value.set(color2);
    });

    return (
        <mesh ref={meshRef} scale={[2, 2, 1]}>
            <planeGeometry args={[2, 2]} />
            <shaderMaterial
                vertexShader={vertexShader}
                fragmentShader={fragmentShader}
                uniforms={uniforms}
                transparent={true}
                depthWrite={false}
                depthTest={false}
            />
        </mesh>
    );
};

// --- Main Component ---

interface HeroGeometricProps {
    title1?: string;
    title2?: string;
    description?: string;
    className?: string; // Explicitly included
    color1?: string;
    color2?: string;
    speed?: number;
}

export default function HeroGeometric({
    title1,
    title2,
    description,
    color1 = "#3B82F6", // Default soft blue
    color2 = "#F0F9FF", // Default pale blue
    speed = 1,
    className,
}: HeroGeometricProps) {
    return (
        <div
            className={cn("relative w-full min-h-screen flex flex-col items-center overflow-hidden bg-white text-black", className)}
            style={{ containerType: "size" }}
        >
            {/* Background Shader */}
            <div className="absolute top-0 left-0 w-full h-full z-0 pointer-events-none">
                <Canvas
                    camera={{ position: [0, 0, 1] }}
                    dpr={[1, 1]}
                    gl={{
                        antialias: false,
                        alpha: true,
                    }}
                >
                    <GradientPlane color1={color1} color2={color2} speed={speed} />
                </Canvas>
            </div>

            {/* Content */}
            {(title1 || title2 || description) && (
                <div className="relative z-10 w-full flex-1 flex flex-col items-center justify-center pt-8 pb-8 md:pt-20 md:pb-20">
                    <div className="w-full max-w-[1200px] px-6 flex flex-col items-center">
                        {/* Headline */}
                        <div className="flex flex-col items-center text-center gap-2 md:gap-4 mb-8 md:mb-12">
                            {title1 && (
                                <div className="overflow-hidden">
                                    <motion.h1
                                        initial={{ y: "100%", opacity: 0 }}
                                        animate={{ y: "0%", opacity: 1 }}
                                        transition={{ duration: 1, ease: [0.16, 1, 0.3, 1], delay: 0.2 }}
                                        className="text-[12cqi] md:text-[8cqi] lg:text-[6cqi] leading-[0.9] tracking-tighter text-[#131313]"
                                    >
                                        <span className="font-serif italic font-light text-[#1a1a1a]">
                                            {title1}
                                        </span>
                                    </motion.h1>
                                </div>
                            )}
                            {title2 && (
                                <div className="overflow-hidden">
                                    <motion.h1
                                        initial={{ y: "100%", opacity: 0 }}
                                        animate={{ y: "0%", opacity: 1 }}
                                        transition={{ duration: 1, ease: [0.16, 1, 0.3, 1], delay: 0.35 }}
                                        className="text-[12cqi] md:text-[8cqi] lg:text-[6cqi] leading-[0.9] tracking-tighter font-bold text-black"
                                    >
                                        {title2}
                                    </motion.h1>
                                </div>
                            )}
                        </div>

                        {/* Subheadline */}
                        {description && (
                            <div className="max-w-[480px] text-center mb-8">
                                <motion.p
                                    initial={{ opacity: 0, y: 20 }}
                                    animate={{ opacity: 1, y: 0 }}
                                    transition={{ duration: 0.8, delay: 0.6, ease: "easeOut" }}
                                    className="text-lg md:text-[1.35rem] leading-relaxed text-neutral-600 font-normal"
                                >
                                    {description}
                                </motion.p>
                            </div>
                        )}
                    </div>
                </div>
            )}
        </div>
    );
}
```
---

## Hyper Text <a name="hyper-text"></a>

A text effect that scrambles and cycles characters before revealing the final text, inspired by cyberpunk aesthetics.

- **Registry Key**: `hyper-text`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/hyper-text.json
```

#### Component Source Code
##### File: `components/ui/hyper-text.tsx`
```tsx
"use client";

import { cn } from "@workspace/ui/lib/utils";
import { useEffect, useRef, useState } from "react";

interface HyperTextProps {
    className?: string;
    duration?: number;
    text: string;
    animateOnLoad?: boolean;
}

const alphabets = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";

export const HyperText = ({
    className,
    duration = 800,
    text,
    animateOnLoad = true,
}: HyperTextProps) => {
    const [displayText, setDisplayText] = useState(text.split(""));
    const [trigger, setTrigger] = useState(false);
    const iterations = useRef(0);
    const isFirstRender = useRef(true);

    const triggerAnimation = () => {
        iterations.current = 0;
        setTrigger(true);
    };

    useEffect(() => {
        const interval = setInterval(() => {
            if (!animateOnLoad && isFirstRender.current) {
                clearInterval(interval);
                isFirstRender.current = false;
                return;
            }
            if (iterations.current < text.length) {
                setDisplayText((t) =>
                    t.map((l, i) =>
                        l === " "
                            ? l
                            : i <= iterations.current
                                ? text[i] ?? ""
                                : alphabets[Math.floor(Math.random() * alphabets.length)]
                    )
                );
                iterations.current = iterations.current + 0.1;
            } else {
                setTrigger(false);
                clearInterval(interval);
            }
        }, duration / (text.length * 10));
        // Clean up interval on unmount
        return () => clearInterval(interval);
    }, [text, duration, trigger, animateOnLoad]);

    return (
        <div
            className={cn("flex cursor-default overflow-hidden py-2 font-mono", className)}
            onMouseEnter={triggerAnimation}
        >
            {displayText.map((letter, i) => (
                <span key={i} className="min-w-[0.1em]">
                    {letter}
                </span>
            ))}
        </div>
    );
};
```
---

## Image Ripple Effect <a name="image-ripple-effect"></a>

A cursor-driven WebGL ripple distortion effect for image cards.

- **Registry Key**: `image-ripple-effect`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/image-ripple-effect.json
```

#### Dependencies
- **NPM Packages**:
  - `three`
  - `@types/three`
  - `@react-three/fiber`
  - `@react-three/drei`

#### How to Use
```tsx
"use client";

import { useEffect, useMemo } from "react";
import { create } from "zustand";
import { ImageRippleEffect } from "@workspace/ui/components/image-ripple-effect";
import { cn } from "@/lib/utils";
import { usePlaygroundStore } from "@/hooks/use-playground-store";

interface ImageRipplePlaygroundConfig {
  imageSrc: string;
  brushTextureUrl: string;
  imageX: number;
  imageY: number;
  imageWidthScale: number;
  imageHeightScale: number;
  distortionStrength: number;
  waveCount: number;
  waveSize: number;
  waveRotationSpeed: number;
  waveFadeMultiplier: number;
  waveGrowth: number;
  waveSpawnThreshold: number;
}

const IMAGE_RIPPLE_DEFAULT_CONFIG: ImageRipplePlaygroundConfig = {
  imageSrc:
    "https://images.unsplash.com/photo-1469474968028-56623f02e42e?auto=format&fit=crop&q=80&w=1400",
  brushTextureUrl: "/brush.png",
  imageX: 0,
  imageY: 0,
  imageWidthScale: 0.34,
  imageHeightScale: 0.42,
  distortionStrength: 0.075,
  waveCount: 100,
  waveSize: 60,
  waveRotationSpeed: 0.025,
  waveFadeMultiplier: 0.95,
  waveGrowth: 0.155,
  waveSpawnThreshold: 0.1,
};

const PRESETS: Array<{ name: string; config: ImageRipplePlaygroundConfig }> = [
  {
    name: "Default",
    config: IMAGE_RIPPLE_DEFAULT_CONFIG,
  },
  {
    name: "Gentle",
    config: {
      ...IMAGE_RIPPLE_DEFAULT_CONFIG,
      distortionStrength: 0.05,
      waveFadeMultiplier: 0.965,
      waveGrowth: 0.12,
      waveRotationSpeed: 0.016,
    },
  },
  {
    name: "Punchy",
    config: {
      ...IMAGE_RIPPLE_DEFAULT_CONFIG,
      distortionStrength: 0.1,
      waveCount: 140,
      waveSize: 72,
      waveFadeMultiplier: 0.93,
      waveGrowth: 0.18,
      waveRotationSpeed: 0.034,
    },
  },
  {
    name: "Dense",
    config: {
      ...IMAGE_RIPPLE_DEFAULT_CONFIG,
      waveCount: 180,
      waveSize: 54,
      waveSpawnThreshold: 0.05,
      waveFadeMultiplier: 0.955,
    },
  },
];

interface ImageRipplePlaygroundStore {
  config: ImageRipplePlaygroundConfig;
  renderVersion: number;
  activePreset: string;
  setConfig: (config: ImageRipplePlaygroundConfig) => void;
  updateConfig: (updates: Partial<ImageRipplePlaygroundConfig>) => void;
  setActivePreset: (preset: string) => void;
  resetPreview: () => void;
  resetConfig: () => void;
}

const useImageRipplePlaygroundStore = create<ImageRipplePlaygroundStore>((set) => ({
  config: IMAGE_RIPPLE_DEFAULT_CONFIG,
  renderVersion: 0,
  activePreset: "Default",
  setConfig: (config) => set({ config }),
  updateConfig: (updates) =>
    set((state) => ({
      config: { ...state.config, ...updates },
    })),
  setActivePreset: (preset) => set({ activePreset: preset }),
  resetPreview: () =>
    set((state) => ({
      renderVersion: state.renderVersion + 1,
    })),
  resetConfig: () =>
    set((state) => ({
      config: IMAGE_RIPPLE_DEFAULT_CONFIG,
      activePreset: "Default",
      renderVersion: state.renderVersion + 1,
    })),
}));

const SectionTitle = ({ children }: { children: React.ReactNode }) => (
  <div className="mb-3 text-[10px] font-mono uppercase tracking-widest text-muted-foreground">{children}</div>
);

const Slider = ({
  value,
  min,
  max,
  step,
  onChange,
  label,
}: {
  value: number;
  min: number;
  max: number;
  step: number;
  onChange: (val: number) => void;
  label: string;
}) => (
  <div className="space-y-2">
    <div className="flex items-center justify-between text-sm">
      <span className="text-foreground/90">{label}</span>
      <span className="font-mono text-muted-foreground">
        {Number(value).toFixed(step < 0.1 ? 3 : 1)}
      </span>
    </div>
    <input
      type="range"
      min={min}
      max={max}
      step={step}
      value={value}
      onChange={(e) => onChange(parseFloat(e.target.value))}
      className="h-2 w-full cursor-pointer appearance-none rounded-full bg-zinc-300/70 dark:bg-zinc-700/70
      [&::-webkit-slider-thumb]:h-4 [&::-webkit-slider-thumb]:w-4 [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:rounded-full [&::-webkit-slider-thumb]:border [&::-webkit-slider-thumb]:border-border [&::-webkit-slider-thumb]:bg-white dark:[&::-webkit-slider-thumb]:bg-zinc-100"
    />
  </div>
);

function generateCode(config: ImageRipplePlaygroundConfig) {
  return `import { ImageRippleEffect } from "@/components/ui/image-ripple-effect"

const images = [
  {
    src: "${config.imageSrc}",
    x: ${config.imageX},
    y: ${config.imageY},
    widthScale: ${config.imageWidthScale},
    heightScale: ${config.imageHeightScale},
  },
]

<ImageRippleEffect
  images={images}
  brushTextureUrl="${config.brushTextureUrl}"
  distortionStrength={${config.distortionStrength}}
  waveCount={${config.waveCount}}
  waveSize={${config.waveSize}}
  waveRotationSpeed={${config.waveRotationSpeed}}
  waveFadeMultiplier={${config.waveFadeMultiplier}}
  waveGrowth={${config.waveGrowth}}
  waveSpawnThreshold={${config.waveSpawnThreshold}}
/>`;
}

export function ImageRippleEffectPlayground() {
  const config = useImageRipplePlaygroundStore((state) => state.config);
  const renderVersion = useImageRipplePlaygroundStore((state) => state.renderVersion);

  useEffect(() => {
    const timeoutId = setTimeout(() => {
      usePlaygroundStore.getState().setCode(generateCode(config));
    }, 250);

    return () => clearTimeout(timeoutId);
  }, [config]);

  return (
    <div className="relative h-full w-full">
      <ImageRippleEffect
        key={renderVersion}
        className="h-full w-full !min-h-full"
        brushTextureUrl={config.brushTextureUrl}
        images={[
          {
            src: config.imageSrc,
            x: config.imageX,
            y: config.imageY,
            widthScale: config.imageWidthScale,
            heightScale: config.imageHeightScale,
          },
        ]}
        distortionStrength={config.distortionStrength}
        waveCount={config.waveCount}
        waveSize={config.waveSize}
        waveRotationSpeed={config.waveRotationSpeed}
        waveFadeMultiplier={config.waveFadeMultiplier}
        waveGrowth={config.waveGrowth}
        waveSpawnThreshold={config.waveSpawnThreshold}
      />
    </div>
  );
}

export function ImageRippleEffectPersonalizePanel() {
  const config = useImageRipplePlaygroundStore((state) => state.config);
  const activePreset = useImageRipplePlaygroundStore((state) => state.activePreset);
  const setConfig = useImageRipplePlaygroundStore((state) => state.setConfig);
  const updateConfig = useImageRipplePlaygroundStore((state) => state.updateConfig);
  const setActivePreset = useImageRipplePlaygroundStore((state) => state.setActivePreset);
  const resetPreview = useImageRipplePlaygroundStore((state) => state.resetPreview);
  const resetConfig = useImageRipplePlaygroundStore((state) => state.resetConfig);

  const selectedPresetConfig = useMemo(
    () => PRESETS.find((preset) => preset.name === activePreset)?.config,
    [activePreset],
  );

  const handleChange = <K extends keyof ImageRipplePlaygroundConfig>(
    key: K,
    value: ImageRipplePlaygroundConfig[K],
  ) => {
    updateConfig({ [key]: value } as Partial<ImageRipplePlaygroundConfig>);

    if (activePreset === "Custom") {
      return;
    }

    const matchedPreset = PRESETS.find(
      (preset) => JSON.stringify(preset.config) === JSON.stringify({ ...config, [key]: value }),
    );
    setActivePreset(matchedPreset?.name ?? "Custom");
  };

  const handlePresetChange = (presetName: string) => {
    const preset = PRESETS.find((item) => item.name === presetName);
    if (!preset) {
      return;
    }

    setConfig({ ...preset.config });
    setActivePreset(presetName);
    resetPreview();
  };

  return (
    <div className="h-full overflow-auto [&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]">
      <div className="space-y-6 px-4 pb-10 pt-20">
        <header className="space-y-2">
          <h2 className="text-2xl font-bold tracking-tighter text-foreground">Personalize</h2>
          <p className="text-sm leading-relaxed text-muted-foreground/90">
            Tune ripple intensity, wave behavior, and image placement in real-time.
          </p>
        </header>

        <div className="flex items-center justify-between">
          <div className="text-sm text-muted-foreground/90">
            Current preset: <span className="font-mono text-foreground">{activePreset}</span>
          </div>
          <button
            type="button"
            onClick={resetConfig}
            className="rounded-md border border-border/40 bg-white/50 px-3 py-1.5 text-[10px] font-mono uppercase tracking-widest text-muted-foreground transition-colors hover:text-foreground dark:bg-white/[0.03]"
          >
            Reset
          </button>
        </div>

        <div>
          <SectionTitle>Presets</SectionTitle>
          <div className="grid grid-cols-4 gap-1.5">
            {PRESETS.map((preset) => {
              const isActive =
                activePreset === preset.name ||
                (activePreset !== "Custom" && selectedPresetConfig && preset.name === activePreset);

              return (
                <button
                  key={preset.name}
                  type="button"
                  onClick={() => handlePresetChange(preset.name)}
                  className={cn(
                    "rounded-md border p-2 text-center text-[10px] font-mono uppercase tracking-widest transition-colors",
                    isActive
                      ? "border-zinc-500/70 bg-zinc-100 dark:border-zinc-500/80 dark:bg-zinc-900/70"
                      : "border-border/70 bg-transparent",
                  )}
                >
                  {preset.name}
                </button>
              );
            })}
          </div>
        </div>

        <div className="space-y-3">
          <SectionTitle>Ripple</SectionTitle>
          <Slider
            label="Distortion Strength"
            min={0.01}
            max={0.2}
            step={0.001}
            value={config.distortionStrength}
            onChange={(val) => handleChange("distortionStrength", val)}
          />
          <Slider
            label="Wave Count"
            min={20}
            max={220}
            step={1}
            value={config.waveCount}
            onChange={(val) => handleChange("waveCount", val)}
          />
          <Slider
            label="Wave Size"
            min={20}
            max={120}
            step={1}
            value={config.waveSize}
            onChange={(val) => handleChange("waveSize", val)}
          />
          <Slider
            label="Rotation Speed"
            min={0.005}
            max={0.06}
            step={0.001}
            value={config.waveRotationSpeed}
            onChange={(val) => handleChange("waveRotationSpeed", val)}
          />
          <Slider
            label="Fade Multiplier"
            min={0.85}
            max={0.995}
            step={0.001}
            value={config.waveFadeMultiplier}
            onChange={(val) => handleChange("waveFadeMultiplier", val)}
          />
          <Slider
            label="Wave Growth"
            min={0.05}
            max={0.3}
            step={0.001}
            value={config.waveGrowth}
            onChange={(val) => handleChange("waveGrowth", val)}
          />
          <Slider
            label="Spawn Threshold"
            min={0.01}
            max={1}
            step={0.01}
            value={config.waveSpawnThreshold}
            onChange={(val) => handleChange("waveSpawnThreshold", val)}
          />
        </div>
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/image-ripple-effect.tsx`
```tsx
"use client";

import { OrthographicCamera, useFBO, useTexture } from "@react-three/drei";
import { Canvas, useFrame, useThree } from "@react-three/fiber";
import { cn } from "@/lib/utils";
import * as React from "react";
import * as THREE from "three";
import { WebGLErrorBoundary, WebGLFallback } from "./webgl-error-boundary";

const fragmentShader = `
uniform sampler2D uTexture;
uniform sampler2D uDisplacement;
uniform vec2 winResolution;
uniform float uStrength;

const float PI = 3.141592653589793238;

void main() {
  vec2 vUvScreen = gl_FragCoord.xy / winResolution.xy;
  vec4 displacement = texture2D(uDisplacement, vUvScreen);
  float theta = displacement.r * 2.0 * PI;

  vec2 dir = vec2(sin(theta), cos(theta));
  vec2 uv = vUvScreen + dir * displacement.r * uStrength;
  vec4 color = texture2D(uTexture, uv);

  gl_FragColor = color;
}
`;

const vertexShader = `
varying vec2 vUv;

void main() {
  vUv = uv;
  gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
}
`;

const BRUSH_DATA_URI = `data:image/svg+xml;utf8,${encodeURIComponent(`
  <svg xmlns="http://www.w3.org/2000/svg" width="128" height="128">
    <defs>
      <radialGradient id="g" cx="50%" cy="50%" r="50%">
        <stop offset="0%" stop-color="white" stop-opacity="1"/>
        <stop offset="65%" stop-color="white" stop-opacity="0.55"/>
        <stop offset="100%" stop-color="white" stop-opacity="0"/>
      </radialGradient>
    </defs>
    <rect width="128" height="128" fill="url(#g)"/>
  </svg>
`)}`;

function createDemoImage(title: string, colorA: string, colorB: string) {
  const svg = `
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 1000">
      <defs>
        <linearGradient id="g" x1="0" y1="0" x2="1" y2="1">
          <stop offset="0%" stop-color="${colorA}" />
          <stop offset="100%" stop-color="${colorB}" />
        </linearGradient>
      </defs>
      <rect width="800" height="1000" fill="url(#g)"/>
      <circle cx="610" cy="180" r="130" fill="white" fill-opacity="0.12"/>
      <circle cx="180" cy="760" r="190" fill="white" fill-opacity="0.12"/>
      <text x="64" y="900" fill="white" font-size="64" font-family="system-ui, sans-serif" opacity="0.9">
        ${title}
      </text>
    </svg>
  `;
  return `data:image/svg+xml;utf8,${encodeURIComponent(svg)}`;
}

const DEFAULT_IMAGE_URLS = [createDemoImage("Aurora", "#0f172a", "#155e75")];

export interface RippleImageItem {
  src: string;
  x?: number;
  y?: number;
  widthScale?: number;
  heightScale?: number;
}

export interface ImageRippleEffectProps
  extends Omit<React.HTMLAttributes<HTMLDivElement>, "children"> {
  className?: string;
  images?: RippleImageItem[];
  brushTextureUrl?: string;
  distortionStrength?: number;
  waveCount?: number;
  waveSize?: number;
  waveRotationSpeed?: number;
  waveFadeMultiplier?: number;
  waveGrowth?: number;
  waveSpawnThreshold?: number;
  children?: React.ReactNode;
}

type ViewportDimensions = {
  width: number;
  height: number;
  pixelRatio: number;
};

function useContainerDimensions(
  ref: React.RefObject<HTMLElement | null>,
): ViewportDimensions {
  const [dimensions, setDimensions] = React.useState<ViewportDimensions>({
    width: 0,
    height: 0,
    pixelRatio: 1,
  });

  React.useEffect(() => {
    const element = ref.current;
    if (!element) {
      return;
    }

    const updateSize = () => {
      const rect = element.getBoundingClientRect();
      setDimensions({
        width: Math.round(rect.width),
        height: Math.round(rect.height),
        pixelRatio:
          typeof window !== "undefined" ? Math.min(window.devicePixelRatio, 2) : 1,
      });
    };

    updateSize();
    const observer = new ResizeObserver(updateSize);
    observer.observe(element);
    window.addEventListener("resize", updateSize);

    return () => {
      observer.disconnect();
      window.removeEventListener("resize", updateSize);
    };
  }, [ref]);

  return dimensions;
}

interface RippleSceneProps {
  width: number;
  height: number;
  pixelRatio: number;
  pointerRef: React.MutableRefObject<{ x: number; y: number }>;
  images: RippleImageItem[];
  brushTextureUrl: string;
  distortionStrength: number;
  waveCount: number;
  waveSize: number;
  waveRotationSpeed: number;
  waveFadeMultiplier: number;
  waveGrowth: number;
  waveSpawnThreshold: number;
}

function RippleScene({
  width,
  height,
  pixelRatio,
  pointerRef,
  images,
  brushTextureUrl,
  distortionStrength,
  waveCount,
  waveSize,
  waveRotationSpeed,
  waveFadeMultiplier,
  waveGrowth,
  waveSpawnThreshold,
}: RippleSceneProps) {
  const { viewport } = useThree();
  const { gl, camera } = useThree();

  const brushTexture = useTexture(brushTextureUrl);
  const imageTextures = useTexture(images.map((item) => item.src));
  const rippleScene = React.useMemo(() => new THREE.Scene(), []);
  const imageScene = React.useMemo(() => new THREE.Scene(), []);

  const waveMeshesRef = React.useRef<THREE.Mesh[]>([]);
  const prevMouseRef = React.useRef({ x: 0, y: 0 });
  const currentWaveRef = React.useRef(0);

  const uniformsRef = React.useRef({
    uDisplacement: { value: null as THREE.Texture | null },
    uTexture: { value: null as THREE.Texture | null },
    winResolution: { value: new THREE.Vector2(1, 1) },
    uStrength: { value: distortionStrength },
  });

  const fboBase = useFBO(Math.max(width, 1), Math.max(height, 1), {
    depthBuffer: false,
    stencilBuffer: false,
  });
  const fboTexture = useFBO(Math.max(width, 1), Math.max(height, 1), {
    depthBuffer: false,
    stencilBuffer: false,
  });

  const imageCamera = React.useMemo(
    () =>
      new THREE.OrthographicCamera(
        viewport.width / -2,
        viewport.width / 2,
        viewport.height / 2,
        viewport.height / -2,
        -1000,
        1000,
      ),
    [viewport.height, viewport.width],
  );

  React.useEffect(() => {
    imageCamera.position.z = 2;
  }, [imageCamera]);

  React.useEffect(() => {
    uniformsRef.current.uStrength.value = distortionStrength;
  }, [distortionStrength]);

  React.useEffect(() => {
    brushTexture.minFilter = THREE.LinearFilter;
    brushTexture.magFilter = THREE.LinearFilter;
    brushTexture.needsUpdate = true;
  }, [brushTexture]);

  React.useEffect(() => {
    waveMeshesRef.current.forEach((mesh) => {
      rippleScene.remove(mesh);
      mesh.geometry.dispose();
      if (Array.isArray(mesh.material)) {
        mesh.material.forEach((material) => material.dispose());
      } else {
        mesh.material.dispose();
      }
    });

    const meshes: THREE.Mesh[] = [];
    for (let i = 0; i < waveCount; i += 1) {
      const geometry = new THREE.PlaneGeometry(waveSize, waveSize, 1, 1);
      const material = new THREE.MeshBasicMaterial({
        transparent: true,
        map: brushTexture,
        depthWrite: false,
      });
      const mesh = new THREE.Mesh(geometry, material);
      mesh.visible = false;
      mesh.rotation.z = Math.random();
      rippleScene.add(mesh);
      meshes.push(mesh);
    }

    waveMeshesRef.current = meshes;
    currentWaveRef.current = 0;

    return () => {
      meshes.forEach((mesh) => {
        rippleScene.remove(mesh);
        mesh.geometry.dispose();
        if (Array.isArray(mesh.material)) {
          mesh.material.forEach((material) => material.dispose());
        } else {
          mesh.material.dispose();
        }
      });
    };
  }, [brushTexture, rippleScene, waveCount, waveSize]);

  React.useEffect(() => {
    while (imageScene.children.length > 0) {
      imageScene.remove(imageScene.children[0] as THREE.Object3D);
    }

    imageScene.add(imageCamera);
    const geometry = new THREE.PlaneGeometry(1, 1);
    const group = new THREE.Group();

    images.forEach((item, index) => {
      const texture = imageTextures[index];
      if (!texture) {
        return;
      }
      texture.minFilter = THREE.LinearFilter;
      texture.magFilter = THREE.LinearFilter;
      texture.needsUpdate = true;

      const mesh = new THREE.Mesh(
        geometry,
        new THREE.MeshBasicMaterial({ map: texture }),
      );
      mesh.position.x = (item.x ?? (index - (images.length - 1) / 2) * 0.25) * viewport.width;
      mesh.position.y = (item.y ?? 0) * viewport.height;
      mesh.position.z = 1;
      mesh.scale.x = viewport.width * (item.widthScale ?? 0.22);
      mesh.scale.y = viewport.width * (item.heightScale ?? 0.28);
      group.add(mesh);
    });

    imageScene.add(group);

    return () => {
      geometry.dispose();
      group.children.forEach((child) => {
        const mesh = child as THREE.Mesh;
        mesh.geometry?.dispose?.();
        if (Array.isArray(mesh.material)) {
          mesh.material.forEach((material) => material.dispose());
        } else {
          mesh.material?.dispose?.();
        }
      });
      imageScene.remove(group);
    };
  }, [imageCamera, imageScene, imageTextures, images, viewport.height, viewport.width]);

  useFrame(() => {
    const x = pointerRef.current.x - width / 2;
    const y = -pointerRef.current.y + height / 2;
    const prev = prevMouseRef.current;
    const moved =
      Math.abs(x - prev.x) > waveSpawnThreshold ||
      Math.abs(y - prev.y) > waveSpawnThreshold;

    if (moved && waveMeshesRef.current.length > 0) {
      const waveIndex = currentWaveRef.current % waveMeshesRef.current.length;
      const mesh = waveMeshesRef.current[waveIndex];
      if (mesh) {
        mesh.position.x = x;
        mesh.position.y = y;
        mesh.visible = true;
        mesh.scale.set(1.75, 1.75, 1);
        mesh.rotation.z = Math.random() * Math.PI;
        const material = mesh.material as THREE.MeshBasicMaterial;
        material.opacity = 1;
      }
      currentWaveRef.current = (waveIndex + 1) % waveMeshesRef.current.length;
    }
    prevMouseRef.current = { x, y };

    waveMeshesRef.current.forEach((mesh) => {
      if (!mesh.visible) {
        return;
      }
      mesh.rotation.z += waveRotationSpeed;
      mesh.scale.x = 0.98 * mesh.scale.x + waveGrowth;
      mesh.scale.y = 0.98 * mesh.scale.y + waveGrowth;
      const material = mesh.material as THREE.MeshBasicMaterial;
      material.opacity *= waveFadeMultiplier;
      if (material.opacity <= 0.01) {
        mesh.visible = false;
      }
    });

    uniformsRef.current.uTexture.value = fboTexture.texture;
    uniformsRef.current.uDisplacement.value = fboBase.texture;
    uniformsRef.current.winResolution.value
      .set(width, height)
      .multiplyScalar(pixelRatio);

    gl.setRenderTarget(fboBase);
    gl.clear();
    gl.render(rippleScene, camera);

    gl.setRenderTarget(fboTexture);
    gl.clear();
    gl.render(imageScene, imageCamera);

    gl.setRenderTarget(null);
  });

  return (
    <mesh>
      <planeGeometry args={[Math.max(width, 1), Math.max(height, 1), 1, 1]} />
      <shaderMaterial
        vertexShader={vertexShader}
        fragmentShader={fragmentShader}
        transparent
        uniforms={uniformsRef.current}
      />
    </mesh>
  );
}

export function ImageRippleEffect({
  className,
  images = DEFAULT_IMAGE_URLS.map((src) => ({ src })),
  brushTextureUrl = BRUSH_DATA_URI,
  distortionStrength = 0.075,
  waveCount = 100,
  waveSize = 60,
  waveRotationSpeed = 0.025,
  waveFadeMultiplier = 0.95,
  waveGrowth = 0.155,
  waveSpawnThreshold = 0.1,
  children,
  ...props
}: ImageRippleEffectProps) {
  const containerRef = React.useRef<HTMLDivElement>(null);
  const { width, height, pixelRatio } = useContainerDimensions(containerRef);
  const pointerRef = React.useRef({ x: 0, y: 0 });

  const handlePointerMove = React.useCallback(
    (event: React.PointerEvent<HTMLDivElement>) => {
      const rect = containerRef.current?.getBoundingClientRect();
      if (!rect) {
        return;
      }
      pointerRef.current = {
        x: event.clientX - rect.left,
        y: event.clientY - rect.top,
      };
    },
    [],
  );

  const frustumSize = height;
  const aspect = width > 0 && height > 0 ? width / height : 1;

  return (
    <div
      ref={containerRef}
      onPointerMove={handlePointerMove}
      className={cn(
        "relative h-[560px] w-full overflow-hidden text-white",
        className,
      )}
      {...props}
    >
      {width > 0 && height > 0 && (
        <WebGLErrorBoundary fallback={<WebGLFallback className="absolute inset-0 h-full w-full" />}>
          <Canvas>
            <OrthographicCamera
              makeDefault
              args={[
                (frustumSize * aspect) / -2,
                (frustumSize * aspect) / 2,
                frustumSize / 2,
                frustumSize / -2,
                -1000,
                1000,
              ]}
              position={[0, 0, 2]}
            />
            <RippleScene
              width={width}
              height={height}
              pixelRatio={pixelRatio}
              pointerRef={pointerRef}
              images={images}
              brushTextureUrl={brushTextureUrl}
              distortionStrength={distortionStrength}
              waveCount={waveCount}
              waveSize={waveSize}
              waveRotationSpeed={waveRotationSpeed}
              waveFadeMultiplier={waveFadeMultiplier}
              waveGrowth={waveGrowth}
              waveSpawnThreshold={waveSpawnThreshold}
            />
          </Canvas>
        </WebGLErrorBoundary>
      )}
      {children ? (
        <div className="pointer-events-none absolute inset-0 z-10">{children}</div>
      ) : null}
    </div>
  );
}
```
##### File: `components/ui/webgl-error-boundary.tsx`
```tsx
"use client";

import { cn } from "@/lib/utils";
import * as React from "react";

interface WebGLErrorBoundaryProps {
  children: React.ReactNode;
  fallback?: React.ReactNode;
  onError?: (error: Error, errorInfo: React.ErrorInfo) => void;
}

interface WebGLErrorBoundaryState {
  hasError: boolean;
}

export class WebGLErrorBoundary extends React.Component<
  WebGLErrorBoundaryProps,
  WebGLErrorBoundaryState
> {
  public state: WebGLErrorBoundaryState = { hasError: false };

  static getDerivedStateFromError(): WebGLErrorBoundaryState {
    return { hasError: true };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    this.props.onError?.(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback ?? <WebGLFallback />;
    }
    return this.props.children;
  }
}

interface WebGLFallbackProps {
  className?: string;
  message?: string;
}

export function WebGLFallback({
  className,
  message = "Interactive WebGL content is unavailable on this device/browser.",
}: WebGLFallbackProps) {
  return (
    <div
      className={cn(
        "flex h-full w-full items-center justify-center bg-gradient-to-br from-zinc-950 via-slate-900 to-zinc-900 px-4 text-center text-sm text-white/75",
        className,
      )}
      role="status"
      aria-live="polite"
    >
      <p>{message}</p>
    </div>
  );
}
```
---

## Image Trail <a name="image-trail"></a>

Component for image-trail

- **Registry Key**: `image-trail`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/image-trail.json
```

#### How to Use
```tsx
"use client";

import { ImageTrail } from "@workspace/ui/components/image-trail";

const defaultScientificImages = [
    "https://images.unsplash.com/photo-1532094349884-543bc11b234d?auto=format&fit=crop&q=80&w=800", // Flasks
    "https://images.unsplash.com/photo-1507413245164-6160d8298b31?auto=format&fit=crop&q=80&w=800", // Lab
    "https://images.unsplash.com/photo-1517976487492-5750f3195933?auto=format&fit=crop&q=80&w=800", // Rocket
    "https://images.unsplash.com/photo-1581093450021-4a7360e9a6b5?auto=format&fit=crop&q=80&w=800", // Microscope
    "https://images.unsplash.com/photo-1530026405186-ed1f139313f8?auto=format&fit=crop&q=80&w=800", // DNA/Abstract
    "https://images.unsplash.com/photo-1507668077129-56e32842fceb?auto=format&fit=crop&q=80&w=800", // Earth/Space
    "https://images.unsplash.com/photo-1518770660439-4636190af475?auto=format&fit=crop&q=80&w=800", // Circuit/Technology
    "https://images.unsplash.com/photo-1633167606207-d840b5070fc2?auto=format&fit=crop&q=80&w=800", // Physics/Lasers
];

export function ImageTrailDemo() {
    return (
        <div className="w-full h-full min-h-[500px] flex rounded-lg overflow-hidden relative bg-black">
            <div className="absolute inset-0 z-10 flex flex-col items-center justify-center p-8 pointer-events-none mix-blend-difference">
                <h2 className="text-4xl md:text-5xl font-bold tracking-tight text-white mb-2">Scientific Discovery</h2>
                <p className="text-zinc-200 text-sm max-w-[280px] text-center">
                    Move your cursor across the space to reveal the hidden knowledge stream.
                </p>
            </div>
            <ImageTrail
                images={defaultScientificImages}
                imageWidth={250}
                imageHeight={320}
                duration={1.6}
                threshold={50}
            />
        </div>
    );
}
```
#### Component Source Code
##### File: `components/ui/image-trail.tsx`
```tsx
"use client"

import React, { useEffect, useRef } from "react"
import { Expo, gsap, Power1, Quint } from "gsap"

interface ImageTrailProps {
    images: string[]
    imageWidth?: number
    imageHeight?: number
    threshold?: number
    duration?: number
}

export function ImageTrail({
    images = [],
    imageWidth = 200,
    imageHeight = 200,
    threshold = 50,
    duration = 1.6,
}: ImageTrailProps) {
    const contentRef = useRef<HTMLDivElement | null>(null)
    const imagesRef = useRef<HTMLImageElement[]>([])
    const mousePos = useRef({ x: 0, y: 0 })
    const cacheMousePos = useRef({ x: 0, y: 0 })
    const lastMousePos = useRef({ x: 0, y: 0 })
    const zIndexVal = useRef(1)
    const imgPosition = useRef(0)
    const parentSize = useRef({ width: 0, height: 0 })

    useEffect(() => {
        if (contentRef.current) {
            imagesRef.current = Array.from(contentRef.current.querySelectorAll("img"))
        }

        const handleMouseMove = (e: MouseEvent) => {
            const rect = contentRef.current?.getBoundingClientRect()
            if (rect) {
                mousePos.current = {
                    x: e.clientX - rect.left,
                    y: e.clientY - rect.top,
                }
            }
        }

        calcParentSize()
        if (imagesRef.current.length === 0) {
            return
        }

        window.addEventListener("mousemove", handleMouseMove)
        window.addEventListener("resize", calcParentSize)

        requestAnimationFrame(renderImages)

        return () => {
            window.removeEventListener("mousemove", handleMouseMove)
            window.removeEventListener("resize", calcParentSize)
        }
        // eslint-disable-next-line react-hooks/exhaustive-deps
    }, [])

    const calcParentSize = () => {
        const rect = contentRef.current?.getBoundingClientRect()
        if (rect) {
            parentSize.current = { width: rect.width, height: rect.height }
        }
    }

    const lerp = (a: number, b: number, n: number) => (1 - n) * a + n * b

    const getMouseDistance = () => {
        const dx = mousePos.current.x - lastMousePos.current.x
        const dy = mousePos.current.y - lastMousePos.current.y
        return Math.hypot(dx, dy)
    }

    const renderImages = () => {
        const distance = getMouseDistance()

        cacheMousePos.current.x = lerp(
            cacheMousePos.current.x,
            mousePos.current.x,
            0.1
        )
        cacheMousePos.current.y = lerp(
            cacheMousePos.current.y,
            mousePos.current.y,
            0.1
        )

        if (distance > threshold) {
            showNextImage()
            zIndexVal.current += 1
            imgPosition.current = (imgPosition.current + 1) % imagesRef.current.length
            lastMousePos.current = { ...mousePos.current }
        }

        requestAnimationFrame(renderImages)
    }

    const showNextImage = () => {
        const img = imagesRef.current[imgPosition.current]
        if (!img) return

        const rect = img.getBoundingClientRect()
        gsap.killTweensOf(img)

        gsap
            .timeline()
            .set(img, {
                startAt: { opacity: 0 },
                opacity: 1,
                zIndex: zIndexVal.current,
                x: cacheMousePos.current.x - rect.width / 2,
                y: cacheMousePos.current.y - rect.height / 2,
            })
            .to(img, {
                duration: duration,
                ease: Expo.easeOut,
                x: mousePos.current.x - rect.width / 2,
                y: mousePos.current.y - rect.height / 2,
            })
            .to(
                img,
                {
                    duration: 1,
                    ease: Power1.easeOut,
                    opacity: 0,
                },
                duration - 1 > 0 ? duration - 1 : 0.4
            )
            .to(
                img,
                {
                    duration: 1,
                    ease: Quint.easeInOut,
                    y: `+=${parentSize.current.height + rect.height / 2}`,
                },
                duration - 1 > 0 ? duration - 1 : 0.4
            )
    }

    return (
        <div
            style={
                {
                    "--image-width": `${imageWidth}px`,
                    "--image-height": `${imageHeight}px`,
                } as React.CSSProperties
            }
            className="relative isolate z-0 flex h-full w-full items-center justify-center overflow-hidden"
            ref={contentRef}
        >
            {images.map((url, index) => (
                <img
                    key={index}
                    className="pointer-events-none absolute top-0 left-0 h-[var(--image-height)] w-[var(--image-width)] object-cover opacity-0 will-change-transform"
                    src={url}
                    alt={`trail element ${index + 1}`}
                />
            ))}
        </div>
    )
}
```
---

## Infinite Image Field <a name="infinite-image-field"></a>

An endless, cursor-driven photo canvas. Images tile infinitely in all directions — move your cursor to pan the field, and it glides fluidly toward where you point.

- **Registry Key**: `infinite-image-field`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/infinite-image-field.json
```

#### How to Use
```tsx
"use client";

import { useEffect } from "react";
import { InfiniteImageField } from "@workspace/ui/components/infinite-image-field";
import { usePlaygroundStore } from "@/hooks/use-playground-store";
import { create } from "zustand";

interface InfiniteImageFieldConfig {
  gap: number;
  maxSpeed: number;
  smoothing: number;
  borderRadius: number;
}

const DEFAULT_CONFIG: InfiniteImageFieldConfig = {
  gap: 28,
  maxSpeed: 5,
  smoothing: 0.07,
  borderRadius: 0,
};

interface InfiniteImageFieldStore {
  config: InfiniteImageFieldConfig;
  renderVersion: number;
  updateConfig: (updates: Partial<InfiniteImageFieldConfig>) => void;
  resetConfig: () => void;
  resetPreview: () => void;
}

const useInfiniteImageFieldStore = create<InfiniteImageFieldStore>((set) => ({
  config: DEFAULT_CONFIG,
  renderVersion: 0,
  updateConfig: (updates) =>
    set((state) => ({ config: { ...state.config, ...updates } })),
  resetConfig: () =>
    set((state) => ({
      config: DEFAULT_CONFIG,
      renderVersion: state.renderVersion + 1,
    })),
  resetPreview: () =>
    set((state) => ({ renderVersion: state.renderVersion + 1 })),
}));

function generateCode(config: InfiniteImageFieldConfig): string {
  const lines: string[] = [];
  lines.push(`<InfiniteImageField`);
  lines.push(`  maxSpeed={${config.maxSpeed}}`);
  lines.push(`  smoothing={${config.smoothing}}`);
  lines.push(`  gap={${config.gap}}`);
  lines.push(`  borderRadius={${config.borderRadius}}`);
  lines.push(`  className="w-full h-screen"`);
  lines.push(`/>`);
  return lines.join("\n");
}

export function InfiniteImageFieldPlayground() {
  const { config, renderVersion } = useInfiniteImageFieldStore();
  usePlaygroundStore();

  useEffect(() => {
    const code = generateCode(config);
    const id = setTimeout(() => {
      usePlaygroundStore.getState().setCode(code);
    }, 250);
    return () => clearTimeout(id);
  }, [config]);

  return (
    <InfiniteImageField
      key={renderVersion}
      gap={config.gap}
      maxSpeed={config.maxSpeed}
      smoothing={config.smoothing}
      borderRadius={config.borderRadius}
      className="w-full h-full"
    />
  );
}

// ─── Shared primitives ────────────────────────────────────────────────────────

const SectionTitle = ({ children }: { children: React.ReactNode }) => (
  <div className="mb-3 text-[10px] font-mono uppercase tracking-widest text-muted-foreground">
    {children}
  </div>
);

const Slider = ({
  value,
  min,
  max,
  step,
  onChange,
  label,
  unit = "",
}: {
  value: number;
  min: number;
  max: number;
  step: number;
  onChange: (val: number) => void;
  label: string;
  unit?: string;
}) => (
  <div className="space-y-2">
    <div className="flex items-center justify-between text-sm">
      <span className="text-foreground/90">{label}</span>
      <span className="font-mono text-muted-foreground">
        {Number(value).toFixed(step < 0.1 ? 2 : 1)}
        {unit}
      </span>
    </div>
    <input
      type="range"
      min={min}
      max={max}
      step={step}
      value={value}
      onChange={(e) => onChange(parseFloat(e.target.value))}
      className="h-2 w-full cursor-pointer appearance-none rounded-full bg-zinc-300/70 dark:bg-zinc-700/70
      [&::-webkit-slider-thumb]:h-4 [&::-webkit-slider-thumb]:w-4 [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:rounded-full [&::-webkit-slider-thumb]:border [&::-webkit-slider-thumb]:border-border [&::-webkit-slider-thumb]:bg-white dark:[&::-webkit-slider-thumb]:bg-zinc-100"
    />
  </div>
);

// ─── Personalize panel ────────────────────────────────────────────────────────

export function InfiniteImageFieldPersonalizePanel() {
  const { config, updateConfig, resetConfig } = useInfiniteImageFieldStore();

  return (
    <div className="h-full overflow-auto bg-[#f3f4f6] dark:bg-[#080808] [&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]">
      <div className="space-y-6 px-4 pb-10 pt-20">
        <header className="space-y-2">
          <h2 className="text-2xl font-bold tracking-tighter text-foreground">
            Personalize
          </h2>
          <p className="text-sm leading-relaxed text-muted-foreground/90">
            Tune the speed, layout, and feel of the infinite image field.
          </p>
        </header>

        <div className="flex items-center justify-end">
          <button
            type="button"
            onClick={resetConfig}
            className="rounded-md border border-border/40 bg-white/50 px-3 py-1.5 text-[10px] font-mono uppercase tracking-widest text-muted-foreground transition-colors hover:text-foreground dark:bg-white/[0.03]"
          >
            Reset
          </button>
        </div>

        {/* Motion */}
        <div>
          <SectionTitle>Motion</SectionTitle>
          <div className="grid grid-cols-1 gap-3">
            <Slider
              label="Max Speed"
              min={1}
              max={20}
              step={0.5}
              value={config.maxSpeed}
              onChange={(v) => updateConfig({ maxSpeed: v })}
              unit="px"
            />
            <Slider
              label="Smoothing"
              min={0.01}
              max={0.3}
              step={0.01}
              value={config.smoothing}
              onChange={(v) => updateConfig({ smoothing: v })}
            />
          </div>
        </div>

        {/* Layout */}
        <div>
          <SectionTitle>Layout</SectionTitle>
          <div className="grid grid-cols-1 gap-3">
            <Slider
              label="Gap"
              min={4}
              max={80}
              step={2}
              value={config.gap}
              onChange={(v) => updateConfig({ gap: v })}
              unit="px"
            />
          </div>
        </div>

        {/* Style */}
        <div>
          <SectionTitle>Style</SectionTitle>
          <div className="grid grid-cols-1 gap-3">
            <Slider
              label="Border Radius"
              min={0}
              max={40}
              step={1}
              value={config.borderRadius}
              onChange={(v) => updateConfig({ borderRadius: v })}
              unit="px"
            />
          </div>
        </div>
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/infinite-image-field.tsx`
```tsx
"use client";

import { cn } from "@/lib/utils";
import { useCallback, useEffect, useRef } from "react";

export const INFINITE_IMAGE_FIELD_IMAGES: string[] = [
  "https://plus.unsplash.com/premium_photo-1665311515452-a9f54c4266c9?w=400&h=560&fit=crop&q=80",
  "https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=400&h=560&fit=crop&q=80",
  "https://images.unsplash.com/photo-1469474968028-56623f02e42e?w=400&h=560&fit=crop&q=80",
  "https://images.unsplash.com/photo-1433086966358-54859d0ed716?w=400&h=560&fit=crop&q=80",
  "https://images.unsplash.com/photo-1501854140801-50d01698950b?w=400&h=560&fit=crop&q=80",
  "https://images.unsplash.com/photo-1464822759023-fed622ff2c3b?w=400&h=560&fit=crop&q=80",
  "https://images.unsplash.com/photo-1500534314209-a25ddb2bd429?w=400&h=560&fit=crop&q=80",
  "https://images.unsplash.com/photo-1440342359743-84fcb8c21f21?w=400&h=560&fit=crop&q=80",
  "https://images.unsplash.com/photo-1511884642898-4c92249e20b6?w=400&h=560&fit=crop&q=80",
  "https://images.unsplash.com/photo-1519681393784-d120267933ba?w=400&h=560&fit=crop&q=80",
  "https://images.unsplash.com/photo-1448375240586-882707db888b?w=400&h=560&fit=crop&q=80",
  "https://images.unsplash.com/photo-1542273917363-3b1817f69a2d?w=400&h=560&fit=crop&q=80",
];

export interface InfiniteImageFieldProps
  extends Omit<React.HTMLAttributes<HTMLDivElement>, "children"> {
  className?: string;
  images?: string[];
  imageWidth?: number;
  imageHeight?: number;
  gap?: number;
  maxSpeed?: number;
  smoothing?: number;
  borderRadius?: number;
}

function drawRoundedRect(
  ctx: CanvasRenderingContext2D,
  x: number,
  y: number,
  w: number,
  h: number,
  r: number
) {
  const clampedR = Math.min(r, w / 2, h / 2);
  ctx.beginPath();
  ctx.moveTo(x + clampedR, y);
  ctx.lineTo(x + w - clampedR, y);
  ctx.quadraticCurveTo(x + w, y, x + w, y + clampedR);
  ctx.lineTo(x + w, y + h - clampedR);
  ctx.quadraticCurveTo(x + w, y + h, x + w - clampedR, y + h);
  ctx.lineTo(x + clampedR, y + h);
  ctx.quadraticCurveTo(x, y + h, x, y + h - clampedR);
  ctx.lineTo(x, y + clampedR);
  ctx.quadraticCurveTo(x, y, x + clampedR, y);
  ctx.closePath();
}

export function InfiniteImageField({
  className,
  images = INFINITE_IMAGE_FIELD_IMAGES,
  imageWidth = 200,
  imageHeight = 280,
  gap = 28,
  maxSpeed = 5,
  smoothing = 0.07,
  borderRadius = 0,
  ...rest
}: InfiniteImageFieldProps) {
  const canvasRef = useRef<HTMLCanvasElement>(null);
  const loadedImagesRef = useRef<HTMLImageElement[]>([]);
  const dimsRef = useRef({ w: 0, h: 0 });
  const camRef = useRef({ x: 0, y: 0 });
  const velRef = useRef({ x: 0, y: 0 });
  const mouseRef = useRef({ x: 0.5, y: 0.5 });
  const isInsideRef = useRef(false);
  const rafRef = useRef<number>(0);

  // Pre-load images
  useEffect(() => {
    const imgs = images.map((src) => {
      const img = new Image();
      img.crossOrigin = "anonymous";
      img.src = src;
      return img;
    });
    loadedImagesRef.current = imgs;
  }, [images]);

  const draw = useCallback(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    const ctx = canvas.getContext("2d");
    if (!ctx) return;

    const { w: W, h: H } = dimsRef.current;
    if (W === 0 || H === 0) {
      rafRef.current = requestAnimationFrame(draw);
      return;
    }

    const dpr = window.devicePixelRatio || 1;
    ctx.setTransform(dpr, 0, 0, dpr, 0, 0);

    const cellW = imageWidth + gap;
    const cellH = imageHeight + gap;
    const imgs = loadedImagesRef.current;
    const numImages = imgs.length;

    // Physics — cursor offset from center drives velocity
    const tx = isInsideRef.current
      ? (mouseRef.current.x - 0.5) * 2 * maxSpeed
      : 0;
    const ty = isInsideRef.current
      ? (mouseRef.current.y - 0.5) * 2 * maxSpeed
      : 0;

    velRef.current.x += (tx - velRef.current.x) * smoothing;
    velRef.current.y += (ty - velRef.current.y) * smoothing;

    camRef.current.x += velRef.current.x;
    camRef.current.y += velRef.current.y;

    const camX = camRef.current.x;
    const camY = camRef.current.y;

    ctx.clearRect(0, 0, W, H);

    // Compute visible cell range
    const colMin = Math.floor((camX - W / 2) / cellW) - 1;
    const colMax = Math.ceil((camX + W / 2) / cellW) + 1;
    const rowMin = Math.floor((camY - H / 2) / cellH) - 1;
    const rowMax = Math.ceil((camY + H / 2) / cellH) + 1;

    for (let row = rowMin; row <= rowMax; row++) {
      for (let col = colMin; col <= colMax; col++) {
        // Top-left corner in screen space
        const sx = col * cellW - camX + W / 2 - imageWidth / 2;
        const sy = row * cellH - camY + H / 2 - imageHeight / 2;

        // Deterministic image assignment — same cell always gets same image
        const imgIdx =
          Math.abs(col * 7 + row * 13 + ((col * row * 3) | 0)) % numImages;
        const img = imgs[imgIdx];

        ctx.save();
        drawRoundedRect(ctx, sx, sy, imageWidth, imageHeight, borderRadius);
        ctx.clip();

        if (img && img.complete && img.naturalWidth > 0) {
          ctx.drawImage(img, sx, sy, imageWidth, imageHeight);
        } else {
          // Placeholder while loading — semi-transparent so background shows
          ctx.fillStyle = "rgba(0,0,0,0.15)";
          ctx.fillRect(sx, sy, imageWidth, imageHeight);
        }
        ctx.restore();

        // Subtle border overlay for glass-panel effect
        ctx.save();
        drawRoundedRect(ctx, sx, sy, imageWidth, imageHeight, borderRadius);
        ctx.strokeStyle = "rgba(255,255,255,0.06)";
        ctx.lineWidth = 1;
        ctx.stroke();
        ctx.restore();
      }
    }

    rafRef.current = requestAnimationFrame(draw);
  }, [imageWidth, imageHeight, gap, maxSpeed, smoothing, borderRadius]);

  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;

    const resize = () => {
      const dpr = window.devicePixelRatio || 1;
      const rect = canvas.getBoundingClientRect();
      dimsRef.current = { w: rect.width, h: rect.height };
      canvas.width = rect.width * dpr;
      canvas.height = rect.height * dpr;
    };

    resize();

    const ro = new ResizeObserver(resize);
    ro.observe(canvas);

    const onMove = (e: MouseEvent) => {
      const rect = canvas.getBoundingClientRect();
      mouseRef.current = {
        x: (e.clientX - rect.left) / rect.width,
        y: (e.clientY - rect.top) / rect.height,
      };
    };

    const onEnter = () => {
      isInsideRef.current = true;
    };
    const onLeave = () => {
      isInsideRef.current = false;
    };

    canvas.addEventListener("mousemove", onMove);
    canvas.addEventListener("mouseenter", onEnter);
    canvas.addEventListener("mouseleave", onLeave);

    rafRef.current = requestAnimationFrame(draw);

    return () => {
      cancelAnimationFrame(rafRef.current);
      ro.disconnect();
      canvas.removeEventListener("mousemove", onMove);
      canvas.removeEventListener("mouseenter", onEnter);
      canvas.removeEventListener("mouseleave", onLeave);
    };
  }, [draw]);

  return (
    <div
      {...rest}
      className={cn("relative w-full h-full overflow-hidden", className)}
    >
      <canvas ref={canvasRef} className="block w-full h-full bg-transparent" />
    </div>
  );
}
```
---

## Interactive Hover Button <a name="interactive-hover-button"></a>

A button that reveals text and an arrow icon on hover.

- **Registry Key**: `interactive-hover-button`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/interactive-hover-button.json
```

#### Dependencies
- **NPM Packages**:
  - `lucide-react`

#### Component Source Code
##### File: `components/ui/interactive-hover-button.tsx`
```tsx
"use client"

import React from "react"
import { ArrowRight } from "lucide-react"
import { cn } from "@/lib/utils"

interface InteractiveHoverButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  text?: string
}

const InteractiveHoverButton = React.forwardRef<
  HTMLButtonElement,
  InteractiveHoverButtonProps
>(({ text = "Button", className, ...props }, ref) => {
  return (
    <button
      ref={ref}
      className={cn(
        "group relative w-32 cursor-pointer overflow-hidden rounded-full border bg-background p-2 text-center font-semibold",
        className,
      )}
      {...props}
    >
      <span className="inline-block translate-x-1 transition-all duration-300 group-hover:translate-x-12 group-hover:opacity-0">
        {text}
      </span>
      <div className="absolute top-0 z-10 flex h-full w-full translate-x-12 items-center justify-center gap-2 text-primary-foreground opacity-0 transition-all duration-300 group-hover:-translate-x-1 group-hover:opacity-100">
        <span>{text}</span>
        <ArrowRight />
      </div>
      <div className="absolute left-[20%] top-[40%] h-2 w-2 scale-[1] rounded-lg bg-primary transition-all duration-300 group-hover:left-[0%] group-hover:top-[0%] group-hover:h-full group-hover:w-full group-hover:scale-[1.8] group-hover:bg-primary"></div>
    </button>
  )
})

InteractiveHoverButton.displayName = "InteractiveHoverButton"

export { InteractiveHoverButton }
```
---

## Layered Stack <a name="layered-stack"></a>

Component for layered-stack

- **Registry Key**: `layered-stack`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/layered-stack.json
```

#### Component Source Code
##### File: `components/ui/layered-stack.tsx`
```tsx
"use client";

import { ComponentProps, useCallback, useEffect, useRef } from "react";
import gsap from "gsap";
import { cn } from "@/lib/utils";

export type LayeredStackProps = ComponentProps<"div">;

export const LayeredStack = ({ children, className, ...props }: LayeredStackProps) => {
    const containerRef = useRef<HTMLDivElement>(null);
    const isStacked = useRef(true);

    const stackCards = useCallback((animate = true) => {
        const container = containerRef.current;
        if (!container) return;

        isStacked.current = true;
        const cards = Array.from(container.children) as HTMLElement[];

        // First clear all transforms so offsetLeft/offsetTop reflect true grid positions
        cards.forEach((card) => {
            gsap.killTweensOf(card);
        });

        if (!animate) {
            // Instant reposition: clear transforms, recalculate, set immediately
            cards.forEach((card) => gsap.set(card, { x: 0, y: 0, rotate: 0 }));
            // Wait a frame so the browser recalculates layout
            requestAnimationFrame(() => {
                const container = containerRef.current;
                if (!container || !isStacked.current) return;
                const cards = Array.from(container.children) as HTMLElement[];
                cards.forEach((card, i) => {
                    const offsetX = container.clientWidth / 2 - card.offsetWidth / 2 - card.offsetLeft;
                    const offsetY = container.clientHeight / 2 - card.offsetHeight / 2 - card.offsetTop;
                    gsap.set(card, {
                        x: offsetX,
                        y: offsetY,
                        rotate: gsap.utils.random(-10, 10),
                        zIndex: 100 - i,
                    });
                });
            });
            return;
        }

        // Animated: first clear, then after layout recalc, animate to center
        cards.forEach((card) => gsap.set(card, { x: 0, y: 0, rotate: 0 }));
        requestAnimationFrame(() => {
            const container = containerRef.current;
            if (!container || !isStacked.current) return;
            const cards = Array.from(container.children) as HTMLElement[];
            cards.forEach((card, i) => {
                const offsetX = container.clientWidth / 2 - card.offsetWidth / 2 - card.offsetLeft;
                const offsetY = container.clientHeight / 2 - card.offsetHeight / 2 - card.offsetTop;
                gsap.to(card, {
                    x: offsetX,
                    y: offsetY,
                    rotate: gsap.utils.random(-10, 10),
                    zIndex: 100 - i,
                    duration: 0.8,
                    ease: "expo.out",
                    overwrite: true,
                });
            });
        });
    }, []);

    const resetCards = useCallback(() => {
        const container = containerRef.current;
        if (!container) return;

        isStacked.current = false;
        const cards = Array.from(container.children) as HTMLElement[];

        gsap.to(cards, {
            x: 0,
            y: 0,
            zIndex: (i: number) => 100 - i,
            duration: 0.8,
            rotate: 0,
            ease: "expo.out",
            stagger: {
                amount: 0.1,
                from: "start",
            },
            overwrite: true,
        });
    }, []);

    useEffect(() => {
        const container = containerRef.current;
        if (!container) return;

        // Initial z-index setup
        const cards = Array.from(container.children) as HTMLElement[];
        cards.forEach((card, i) => {
            gsap.set(card, { zIndex: 100 - i });
        });

        stackCards();

        // Re-run current state on resize
        const ro = new ResizeObserver(() => {
            if (isStacked.current) {
                stackCards(false);
            }
            // If expanded (reset), transforms are already 0 so nothing to fix
        });
        ro.observe(container);

        return () => ro.disconnect();
    }, [stackCards]);

    return (
        <div
            ref={containerRef}
            onMouseEnter={resetCards}
            onMouseLeave={() => stackCards(true)}
            className={cn("relative", className)}
            {...props}>
            {children}
        </div>
    );
};
```
---

## Letter Cascade <a name="letter-cascade"></a>

Component for letter-cascade

- **Registry Key**: `letter-cascade`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/letter-cascade.json
```

#### Component Source Code
##### File: `components/ui/letter-cascade.tsx`
```tsx
"use client";

import { cn } from "@workspace/ui/lib/utils";
import {
    type AnimationOptions,
    motion,
    stagger,
    useAnimate,
} from "framer-motion";
import { useCallback, useState } from "react";

interface LetterCascadeProps {
    /** The text to animate */
    text: string;
    /** Additional CSS classes for the container */
    className?: string;
    /** CSS classes applied to each individual letter */
    letterClassName?: string;
    /** Stagger delay between each letter in seconds */
    staggerDuration?: number;
    /** Where the stagger wave originates */
    staggerFrom?: "first" | "last" | "center" | number;
    /** Spring stiffness — higher = snappier */
    stiffness?: number;
    /** Spring damping — lower = bouncier */
    damping?: number;
    /** Trigger the animation on click instead of hover */
    triggerOnClick?: boolean;
    /** Callback when the full animation cycle completes */
    onComplete?: () => void;
}

export function LetterCascade({
    text,
    className,
    letterClassName,
    staggerDuration = 0.04,
    staggerFrom = "first",
    stiffness = 220,
    damping = 16,
    triggerOnClick = false,
    onComplete,
}: LetterCascadeProps) {
    const [scope, animate] = useAnimate();
    const [blocked, setBlocked] = useState(false);

    const trigger = useCallback(() => {
        if (blocked) return;
        setBlocked(true);

        const merge = (base: AnimationOptions): AnimationOptions => ({
            ...base,
            delay: stagger(staggerDuration, { from: staggerFrom }),
        });

        const spring: AnimationOptions = {
            type: "spring",
            stiffness,
            damping,
        };

        // ── Phase 1: front tilts back, echo flips in from below ──
        animate(
            ".cascade-front",
            {
                rotateX: 90,
                opacity: 0,
                y: -6,
                filter: "blur(4px)",
            },
            merge(spring)
        ).then(() => {
            // Instantly reset front
            animate(
                ".cascade-front",
                { rotateX: 0, opacity: 1, y: 0, filter: "blur(0px)" },
                { duration: 0 }
            ).then(() => {
                setBlocked(false);
                onComplete?.();
            });
        });

        animate(
            ".cascade-echo",
            {
                rotateX: 0,
                opacity: 1,
                y: 0,
                scale: 1,
                filter: "blur(0px)",
            },
            merge(spring)
        ).then(() => {
            // Instantly reset echo
            animate(
                ".cascade-echo",
                {
                    rotateX: -90,
                    opacity: 0,
                    y: 6,
                    scale: 0.8,
                    filter: "blur(4px)",
                },
                { duration: 0 }
            );
        });
    }, [
        blocked,
        animate,
        staggerDuration,
        staggerFrom,
        stiffness,
        damping,
        onComplete,
    ]);

    return (
        <span
            ref={scope}
            className={cn(
                "inline-flex cursor-pointer select-none items-center justify-center",
                className
            )}
            {...(triggerOnClick ? { onClick: trigger } : { onMouseEnter: trigger })}
            aria-label={text}
        >
            {text.split("").map((letter, i) => (
                <span
                    key={i}
                    className="relative inline-flex whitespace-pre"
                    style={{ perspective: "500px" }}
                >
                    {/* Front face — visible by default, tilts backward on trigger */}
                    <motion.span
                        className={cn("cascade-front inline-block", letterClassName)}
                        style={{
                            rotateX: 0,
                            y: 0,
                            transformOrigin: "bottom center",
                            backfaceVisibility: "hidden",
                        }}
                    >
                        {letter}
                    </motion.span>

                    {/* Echo face — hidden below, flips up into view on trigger */}
                    <motion.span
                        className={cn(
                            "cascade-echo absolute inset-0 inline-block",
                            letterClassName
                        )}
                        style={{
                            rotateX: -90,
                            opacity: 0,
                            y: 6,
                            scale: 0.8,
                            filter: "blur(4px)",
                            transformOrigin: "top center",
                            backfaceVisibility: "hidden",
                        }}
                    >
                        {letter}
                    </motion.span>
                </span>
            ))}
        </span>
    );
}
```
---

## Liquid Blob <a name="liquid-blob"></a>

Animated liquid morphing blob shapes with mouse interaction that create a beautiful organic background effect.

- **Registry Key**: `liquid-blob`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/liquid-blob.json
```

#### Dependencies
- **NPM Packages**:
  - `framer-motion`

#### How to Use
```tsx
"use client";

import { LiquidBlob } from "@workspace/ui/components/liquid-blob";

export function LiquidBlobDemo() {
  return (
    <div className="relative h-full w-full overflow-hidden bg-white dark:bg-[#121212]">
      <LiquidBlob interactive />
      <div className="relative z-10 flex h-full items-center justify-center pointer-events-none">
        <h3 className="text-2xl font-bold text-foreground">Liquid Blob</h3>
      </div>
    </div>
  );
}

export function LiquidBlobTealDemo() {
  return (
    <div className="relative h-full w-full overflow-hidden bg-white dark:bg-[#121212]">
      <LiquidBlob
        color="#10b981"
        secondaryColor="#06b6d4"
        size={350}
        blur={80}
        interactive
      />
      <div className="relative z-10 flex h-full items-center justify-center pointer-events-none">
        <h3 className="text-2xl font-bold text-foreground">Teal Theme</h3>
      </div>
    </div>
  );
}

export function LiquidBlobFastDemo() {
  return (
    <div className="relative h-full w-full overflow-hidden bg-white dark:bg-[#121212]">
      <LiquidBlob
        color="#f97316"
        secondaryColor="#ef4444"
        size={250}
        speed={4}
        opacity={0.8}
        interactive
      />
      <div className="relative z-10 flex h-full items-center justify-center pointer-events-none">
        <h3 className="text-2xl font-bold text-foreground">Fast Animation</h3>
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/liquid-blob.tsx`
```tsx
"use client"

import { motion, useMotionValue, useSpring } from "framer-motion"
import { useEffect, useRef } from "react"
import { cn } from "@/lib/utils"

interface LiquidBlobProps {
  className?: string
  color?: string
  secondaryColor?: string
  size?: number
  blur?: number
  speed?: number
  opacity?: number
  interactive?: boolean
}

export function LiquidBlob({
  className,
  color = "#8b5cf6",
  secondaryColor = "#ec4899",
  size = 300,
  blur = 60,
  speed = 8,
  opacity = 0.7,
  interactive = true,
}: LiquidBlobProps) {
  const containerRef = useRef<HTMLDivElement>(null)
  const mouseX = useMotionValue(0)
  const mouseY = useMotionValue(0)
  
  const springConfig = { damping: 25, stiffness: 150 }
  const smoothX = useSpring(mouseX, springConfig)
  const smoothY = useSpring(mouseY, springConfig)

  useEffect(() => {
    if (!interactive) return
    
    const container = containerRef.current
    if (!container) return

    const handleMouseMove = (e: MouseEvent) => {
      const rect = container.getBoundingClientRect()
      const x = e.clientX - rect.left - rect.width / 2
      const y = e.clientY - rect.top - rect.height / 2
      mouseX.set(x * 0.15)
      mouseY.set(y * 0.15)
    }

    const handleMouseLeave = () => {
      mouseX.set(0)
      mouseY.set(0)
    }

    container.addEventListener("mousemove", handleMouseMove)
    container.addEventListener("mouseleave", handleMouseLeave)

    return () => {
      container.removeEventListener("mousemove", handleMouseMove)
      container.removeEventListener("mouseleave", handleMouseLeave)
    }
  }, [interactive, mouseX, mouseY])

  return (
    <div 
      ref={containerRef}
      className={cn("pointer-events-auto absolute inset-0 overflow-hidden", className)}
    >
      <motion.div
        className="absolute rounded-full"
        style={{
          width: size,
          height: size,
          background: `radial-gradient(circle, ${color} 0%, ${secondaryColor} 100%)`,
          filter: `blur(${blur}px)`,
          opacity,
          left: "20%",
          top: "20%",
          x: smoothX,
          y: smoothY,
        }}
        animate={{
          scale: [1, 1.2, 0.9, 1.1, 1],
          borderRadius: [
            "60% 40% 30% 70% / 60% 30% 70% 40%",
            "30% 60% 70% 40% / 50% 60% 30% 60%",
            "60% 40% 30% 70% / 60% 30% 70% 40%",
            "40% 60% 60% 40% / 70% 30% 50% 60%",
            "60% 40% 30% 70% / 60% 30% 70% 40%",
          ],
        }}
        transition={{
          duration: speed,
          repeat: Infinity,
          ease: "easeInOut",
        }}
      />
      <motion.div
        className="absolute rounded-full"
        style={{
          width: size * 0.8,
          height: size * 0.8,
          background: `radial-gradient(circle, ${secondaryColor} 0%, ${color} 100%)`,
          filter: `blur(${blur}px)`,
          opacity: opacity * 0.8,
          right: "20%",
          bottom: "20%",
          x: useSpring(mouseX, { damping: 30, stiffness: 120 }),
          y: useSpring(mouseY, { damping: 30, stiffness: 120 }),
        }}
        animate={{
          scale: [1, 0.9, 1.2, 1, 1],
          borderRadius: [
            "40% 60% 60% 40% / 70% 30% 50% 60%",
            "60% 40% 30% 70% / 60% 30% 70% 40%",
            "30% 60% 70% 40% / 50% 60% 30% 60%",
            "60% 40% 30% 70% / 60% 30% 70% 40%",
            "40% 60% 60% 40% / 70% 30% 50% 60%",
          ],
        }}
        transition={{
          duration: speed * 1.2,
          repeat: Infinity,
          ease: "easeInOut",
        }}
      />
      <motion.div
        className="absolute rounded-full"
        style={{
          width: size * 0.6,
          height: size * 0.6,
          background: `radial-gradient(circle, ${color} 30%, transparent 100%)`,
          filter: `blur(${blur * 0.8}px)`,
          opacity: opacity * 0.6,
          left: "50%",
          top: "50%",
          marginLeft: -size * 0.3,
          marginTop: -size * 0.3,
          x: useSpring(mouseX, { damping: 20, stiffness: 100 }),
          y: useSpring(mouseY, { damping: 20, stiffness: 100 }),
        }}
        animate={{
          scale: [1, 1.3, 0.8, 1.1, 1],
          borderRadius: [
            "60% 40% 30% 70% / 60% 30% 70% 40%",
            "40% 60% 60% 40% / 70% 30% 50% 60%",
            "60% 40% 30% 70% / 60% 30% 70% 40%",
            "30% 60% 70% 40% / 50% 60% 30% 60%",
            "60% 40% 30% 70% / 60% 30% 70% 40%",
          ],
        }}
        transition={{
          duration: speed * 0.9,
          repeat: Infinity,
          ease: "easeInOut",
        }}
      />
    </div>
  )
}
```
---

## Mac Keyboard <a name="mac-keyboard"></a>

Component for mac-keyboard

- **Registry Key**: `mac-keyboard`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/mac-keyboard.json
```

#### Component Source Code
##### File: `components/ui/mac-keyboard.tsx`
```tsx
"use client";

import { cn } from "@/lib/utils";
import {
  ArrowLeft,
  ArrowRight,
  ArrowDown,
  ArrowUp,
  ChevronUp,
  Command,
  Globe,
  LayoutGrid,
  Lock,
  Mic,
  Moon,
  Option,
  Play,
  Search,
  SkipBack,
  SkipForward,
  Sun,
  Volume1,
  Volume2,
  VolumeX,
} from "lucide-react";
import * as React from "react";

// Context to share active keys state
const KeyboardContext = React.createContext<{
  activeKeys: Set<string>;
}>({
  activeKeys: new Set(),
});

export interface MacKeyProps extends React.HTMLAttributes<HTMLDivElement> {
  label?: React.ReactNode;
  subLabel?: React.ReactNode;
  icon?: React.ReactNode;
  iconLabel?: string;
  width?: number;
  keyCode?: string | string[]; // Can be a single code or array of codes
  noAspectRatio?: boolean; // Skip aspect-ratio on wrapper (for arrow half-height keys)
}

interface MacKeyboardProps extends React.HTMLAttributes<HTMLDivElement> {
  // Optional prop to provide a custom sound URL
  soundSrc?: string;
}

export function MacKeyboard({ className, soundSrc = "/audio/key-press.wav", ...props }: MacKeyboardProps) {
  const [activeKeys, setActiveKeys] = React.useState<Set<string>>(new Set());
  const audioCtxRef = React.useRef<AudioContext | null>(null);
  const audioBufferRef = React.useRef<AudioBuffer | null>(null);

  React.useEffect(() => {
    if (!soundSrc) return;
    // Use Web Audio API for low-latency, short key click sounds
    const ctx = new (window.AudioContext || (window as unknown as { webkitAudioContext: typeof AudioContext }).webkitAudioContext)();
    audioCtxRef.current = ctx;

    fetch(soundSrc)
      .then((res) => res.arrayBuffer())
      .then((buf) => ctx.decodeAudioData(buf))
      .then((decoded) => { audioBufferRef.current = decoded; })
      .catch(() => {}); // Silently fail if audio can't load

    return () => { ctx.close().catch(() => {}); };
  }, [soundSrc]);

  const playClick = React.useCallback(() => {
    const ctx = audioCtxRef.current;
    const buffer = audioBufferRef.current;
    if (!ctx || !buffer) return;

    const play = () => {
      const source = ctx.createBufferSource();
      source.buffer = buffer;

      const gain = ctx.createGain();
      gain.gain.value = 0.15; // Set volume very low (15%) so it's a subtle click

      source.connect(gain);
      gain.connect(ctx.destination);
      source.start(0);
    };

    if (ctx.state === "suspended") {
      ctx.resume().then(play).catch(() => {});
    } else {
      play();
    }
  }, []);

  React.useEffect(() => {
    const handleKeyDown = (e: KeyboardEvent) => {
      if (e.repeat) return;

      setActiveKeys((prev) => {
        const newSet = new Set(prev);
        newSet.add(e.code);
        return newSet;
      });

      playClick();
    };

    const handleKeyUp = (e: KeyboardEvent) => {
      setActiveKeys((prev) => {
        const newSet = new Set(prev);
        newSet.delete(e.code);
        return newSet;
      });
    };

    window.addEventListener("keydown", handleKeyDown);
    window.addEventListener("keyup", handleKeyUp);
    return () => {
      window.removeEventListener("keydown", handleKeyDown);
      window.removeEventListener("keyup", handleKeyUp);
    };
  }, [playClick]);

  return (
    <KeyboardContext.Provider value={{ activeKeys }}>
      {props.children ? (
        <div 
          className={cn(
            "inline-flex items-center gap-1.5 rounded-[1.5rem] bg-neutral-200 p-3 shadow-2xl dark:bg-neutral-900 border border-neutral-300 dark:border-neutral-800", 
            className
          )} 
          {...props}
        >
          {props.children}
        </div>
      ) : (
        <div
          className={cn(
            "flex w-full min-w-[800px] max-w-5xl shrink-0 flex-col gap-2 rounded-[1.5rem] bg-neutral-200 p-3 shadow-2xl dark:bg-neutral-900 border border-neutral-300 dark:border-neutral-800",
            className
          )}
          style={{ minWidth: "800px" }}
          {...props}
        >
          {/* Row 1: Esc, F1-F12, Touch ID */}
          <Row>
            <MacKey width={1.5} keyCode="Escape" className="text-[10px] items-start pl-3">esc</MacKey>
            <MacKey width={1} keyCode="F1" icon={<Sun className="h-4 w-4" />} iconLabel="F1" />
            <MacKey width={1} keyCode="F2" icon={<Sun className="h-4 w-4" />} iconLabel="F2" />
            <MacKey width={1} keyCode="F3" icon={<LayoutGrid className="h-4 w-4" />} iconLabel="F3" />
            <MacKey width={1} keyCode="F4" icon={<Search className="h-4 w-4" />} iconLabel="F4" />
            <MacKey width={1} keyCode="F5" icon={<Mic className="h-4 w-4" />} iconLabel="F5" />
            <MacKey width={1} keyCode="F6" icon={<Moon className="h-4 w-4" />} iconLabel="F6" />
            <MacKey width={1} keyCode="F7" icon={<SkipBack className="h-4 w-4 fill-current" />} iconLabel="F7" />
            <MacKey width={1} keyCode="F8" icon={<Play className="h-4 w-4 fill-current" />} iconLabel="F8" />
            <MacKey width={1} keyCode="F9" icon={<SkipForward className="h-4 w-4 fill-current" />} iconLabel="F9" />
            <MacKey width={1} keyCode="F10" icon={<VolumeX className="h-4 w-4" />} iconLabel="F10" />
            <MacKey width={1} keyCode="F11" icon={<Volume1 className="h-4 w-4" />} iconLabel="F11" />
            <MacKey width={1} keyCode="F12" icon={<Volume2 className="h-4 w-4" />} iconLabel="F12" />
            <MacKey width={1} icon={<Lock className="h-4 w-4" />} />
          </Row>

      {/* Row 2: Numbers */}
      <Row>
        <MacKey width={1} label="`" subLabel="~" />
        <MacKey width={1} label="1" subLabel="!" />
        <MacKey width={1} label="2" subLabel="@" />
        <MacKey width={1} label="3" subLabel="#" />
        <MacKey width={1} label="4" subLabel="$" />
        <MacKey width={1} label="5" subLabel="%" />
        <MacKey width={1} label="6" subLabel="^" />
        <MacKey width={1} label="7" subLabel="&" />
        <MacKey width={1} label="8" subLabel="*" />
        <MacKey width={1} label="9" subLabel="(" />
        <MacKey width={1} label="0" subLabel=")" />
        <MacKey width={1} label="-" subLabel="_" />
        <MacKey width={1} label="=" subLabel="+" />
        <MacKey width={1.5} className="items-end pr-2 text-xs" label="delete" />
      </Row>

      {/* Row 3: Tab */}
      <Row>
        <MacKey width={1.5} className="items-start pl-3 text-xs" label="tab" />
        <MacKey width={1} label="Q" />
        <MacKey width={1} label="W" />
        <MacKey width={1} label="E" />
        <MacKey width={1} label="R" />
        <MacKey width={1} label="T" />
        <MacKey width={1} label="Y" />
        <MacKey width={1} label="U" />
        <MacKey width={1} label="I" />
        <MacKey width={1} label="O" />
        <MacKey width={1} label="P" />
        <MacKey width={1} label="[" subLabel="{" />
        <MacKey width={1} label="]" subLabel="}" />
        <MacKey width={1} label="\" subLabel="|" />
      </Row>

      {/* Row 4: Caps */}
      <Row>
        <MacKey width={1.75} className="items-start pl-3 text-xs relative group" label="">
           <span className="z-10 mt-0.5">caps lock</span>
           <div className="absolute left-2 top-2 h-1.5 w-1.5 rounded-full bg-neutral-900/20 group-active:bg-green-500 transition-colors dark:bg-white/20" />
        </MacKey>
        <MacKey width={1} label="A" />
        <MacKey width={1} label="S" />
        <MacKey width={1} label="D" />
        <MacKey width={1} label="F" />
        <MacKey width={1} label="G" />
        <MacKey width={1} label="H" />
        <MacKey width={1} label="J" />
        <MacKey width={1} label="K" />
        <MacKey width={1} label="L" />
        <MacKey width={1} label=";" subLabel=":" />
        <MacKey width={1} label="'" subLabel='"' />
        <MacKey width={1.75} className="items-end pr-2 text-xs" label="return" />
      </Row>

      {/* Row 5: Shift */}
      <Row>
        <MacKey width={2.25} keyCode="ShiftLeft" className="items-start pl-3 text-xs" label="shift" />
        <MacKey width={1} label="Z" />
        <MacKey width={1} label="X" />
        <MacKey width={1} label="C" />
        <MacKey width={1} label="V" />
        <MacKey width={1} label="B" />
        <MacKey width={1} label="N" />
        <MacKey width={1} label="M" />
        <MacKey width={1} label="," subLabel="<" />
        <MacKey width={1} label="." subLabel=">" />
        <MacKey width={1} label="/" subLabel="?" />
        <MacKey width={2.25} keyCode="ShiftRight" className="items-end pr-2 text-xs" label="shift" />
      </Row>

      {/* Row 6: Bottom */}
      <Row>
        <MacKey
          width={1}
          className="items-end pr-1 pb-1"
          label={<span className="text-[10px] font-normal">fn</span>}
        >
          <Globe className="absolute top-2 left-2 h-4 w-4" />
        </MacKey>
        <MacKey
          width={1}
          keyCode="ControlLeft"
          className="items-end pr-1 pb-1"
          label={<span className="text-[10px] font-normal">control</span>}
        >
          <ChevronUp className="absolute top-2 left-2 h-4 w-4" />
        </MacKey>
        <MacKey
          width={1.25}
          keyCode="AltLeft"
          className="items-end pr-1 pb-1"
          label={<span className="text-[10px] font-normal">option</span>}
        >
          <Option className="absolute top-2 left-2 h-4 w-4" />
        </MacKey>
        <MacKey
          width={1.5}
          keyCode="MetaLeft"
          className="items-end pr-1 pb-1"
          label={<span className="text-[10px] font-bold">command</span>}
        >
          <Command className="absolute top-2 left-2 h-4 w-4" />
        </MacKey>
        {/* Spacebar */}
        <MacKey width={4} keyCode="Space" /> 
        <MacKey
          width={1.5}
          keyCode="MetaRight"
          className="items-end pl-1 pb-1"
          label={<span className="text-[10px] font-bold">command</span>}
        >
           <Command className="absolute top-2 right-2 h-4 w-4" />
        </MacKey>
        <MacKey
          width={1.25}
          keyCode="AltRight"
          className="items-end pl-1 pb-1"
          label={<span className="text-[10px] font-normal">option</span>}
        >
           <Option className="absolute top-2 right-2 h-4 w-4" />
        </MacKey>
        
        {/* Arrow keys */}
        <div style={{ flex: 3 }} className="grid h-full grid-cols-3 items-end gap-1.5 pl-0.5">
          <MacKey width={1} keyCode="ArrowLeft" className="h-full">
            <ArrowLeft className="h-4 w-4" />
          </MacKey>
          <div className="flex h-full min-h-0 flex-col gap-1.5">
            <MacKey
              width={1}
              noAspectRatio
              keyCode="ArrowUp"
              style={{ flex: 1 }}
              className="!min-h-0 items-center justify-center p-0 !rounded-[4px] shadow-[0_1px_0_1px_rgba(0,0,0,0.1)]"
            >
              <ArrowUp className="h-3 w-3" />
            </MacKey>
            <MacKey
              width={1}
              noAspectRatio
              keyCode="ArrowDown"
              style={{ flex: 1 }}
              className="!min-h-0 items-center justify-center p-0 !rounded-[4px] shadow-[0_1px_0_1px_rgba(0,0,0,0.1)]"
            >
              <ArrowDown className="h-3 w-3" />
            </MacKey>
          </div>
          <MacKey width={1} keyCode="ArrowRight" className="h-full">
            <ArrowRight className="h-4 w-4" />
          </MacKey>
        </div>
      </Row>
        </div>
      )}
    </KeyboardContext.Provider>
  );
}

function Row({ children }: { children: React.ReactNode }) {
  return <div className="flex w-full gap-1.5">{children}</div>;
}

export function MacKey({
  className,
  label,
  subLabel,
  icon,
  iconLabel,
  width = 1,
  children,
  keyCode,
  noAspectRatio,
  ...props
}: MacKeyProps) {
  const { activeKeys } = React.useContext(KeyboardContext);

  const isActive = React.useMemo(() => {
    // 1. Check explicit keyCode prop
    if (keyCode) {
      if (Array.isArray(keyCode)) {
        return keyCode.some((code) => activeKeys.has(code));
      }
      return activeKeys.has(keyCode);
    }

    // 2. Try to infer from label (simple cases)
    if (typeof label === "string") {
      const l = label.toLowerCase();
      
      // Numbers
      if (/^[0-9]$/.test(l)) return activeKeys.has(`Digit${l}`);
      
      // Letters
      if (/^[a-z]$/.test(l)) return activeKeys.has(`Key${l.toUpperCase()}`);

      // Common symbols
      const symbolMap: Record<string, string> = {
        "-": "Minus",
        "=": "Equal",
        "[": "BracketLeft",
        "]": "BracketRight",
        "\\": "Backslash",
        ";": "Semicolon",
        "'": "Quote",
        ",": "Comma",
        ".": "Period",
        "/": "Slash",
        "`": "Backquote",
        "delete": "Backspace",
        "tab": "Tab", 
        "caps lock": "CapsLock",
        "return": "Enter",
        "space": "Space"
      };
      
      if (symbolMap[l]) return activeKeys.has(symbolMap[l]);
    }
    
    return false;
  }, [activeKeys, keyCode, label]);

  // For width=1 keys, use aspect-ratio to define the row height.
  // For wider keys, omit aspect-ratio so they stretch to the row height via align-items: stretch.
  const applyAspectRatio = !noAspectRatio;
  return (
    <div
      style={{
        flex: width,
        ...(applyAspectRatio ? { aspectRatio: `${width}/1` } : {}),
      }}
      className="min-w-0 self-stretch"
    >
      <div
        className={cn(
          "group relative flex h-full w-full min-w-0 select-none flex-col items-center justify-center overflow-hidden rounded-lg bg-white",
          // Light mode shadows
          "shadow-[inset_0_-2px_1px_rgba(0,0,0,0.05),inset_0_1px_1px_rgba(255,255,255,1),0_1px_1px_rgba(0,0,0,0.1)]",
          // Dark mode overrides
          "dark:bg-neutral-800 dark:shadow-[inset_0_-2px_1px_rgba(0,0,0,0.4),inset_0_1px_1px_rgba(255,255,255,0.05),0_1px_1px_rgba(0,0,0,0.5)]",
          // Active state styles - mimicking the active: pseudo-class but triggered by state
          isActive && "translate-y-[1px] shadow-none scale-[0.98]",
          "active:translate-y-[1px] active:shadow-none transition-all duration-100 active:scale-[0.98]",
          className
        )}
        {...props}
      >
        {/* Icon only keys (F-keys) */}
        {icon && !label && !subLabel && !children && (
           <div className="flex flex-col items-center justify-between h-full py-2">
              <span className="text-[15px]">{icon}</span>
              {iconLabel && <span className="text-[7px] leading-none opacity-60 font-medium">{iconLabel}</span>}
           </div>
        )}

        {/* Number/Symbol keys */}
        {subLabel && (
          <div className="flex flex-col items-center justify-between h-full py-2">
             <span className="text-xs font-normal opacity-60">{subLabel}</span>
             <span className="text-sm font-medium">{label}</span>
          </div>
        )}
        
        {/* Letter keys */}
        {!subLabel && !icon && typeof label === "string" && label.length === 1 && (
          <span className="text-lg font-medium">{label}</span>
        )}

        {/* Modifier keys with text label */}
        {!subLabel && !icon && (typeof label !== "string" || label.length > 1) && !children && (
          <span className="font-medium text-xs">{label}</span>
        )}
        
        {children}
      </div>
    </div>
  );
}
```
---

## Magnet Lines <a name="magnet-lines"></a>

A grid of lines that rotate to face the cursor, creating a magnetic field effect.

- **Registry Key**: `magnet-lines`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/magnet-lines.json
```

#### Dependencies
- **NPM Packages**:
  - `framer-motion`

#### How to Use
```tsx
"use client";

import { MagnetLines } from "@workspace/ui/components/magnet-lines";

export function MagnetLinesDemo() {
  return (
    <div className="flex h-full w-full items-center justify-center">
      <MagnetLines
        rows={9}
        columns={9}
        containerSize="60vmin"
        lineColor="currentColor"
        lineWidth="0.8vmin"
        lineHeight="5vmin"
        baseAngle={0}
        className="text-foreground"
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/magnet-lines.tsx`
```tsx
"use client";

import React, { useRef, useState, useEffect } from "react";
import { motion } from "framer-motion";

interface MagnetLinesProps {
    rows?: number;
    columns?: number;
    containerSize?: string;
    lineColor?: string;
    lineWidth?: string;
    lineHeight?: string;
    baseAngle?: number;
    className?: string;
    style?: React.CSSProperties;
}

export function MagnetLines({
    rows = 9,
    columns = 9,
    containerSize = "80vmin",
    lineColor = "#efefef",
    lineWidth = "1vmin",
    lineHeight = "6vmin",
    baseAngle = 0,
    className = "",
    style = {},
}: MagnetLinesProps) {
    const containerRef = useRef<HTMLDivElement>(null);

    // Spans the grid
    const total = rows * columns;
    const spans = Array.from({ length: total }, (_, i) => i);

    return (
        <div
            ref={containerRef}
            className={`relative grid place-items-center ${className}`}
            style={{
                gridTemplateColumns: `repeat(${columns}, 1fr)`,
                gridTemplateRows: `repeat(${rows}, 1fr)`,
                width: containerSize,
                height: containerSize,
                ...style,
            }}
        >
            {spans.map((i) => (
                <Line
                    key={i}
                    containerRef={containerRef}
                    lineColor={lineColor}
                    lineWidth={lineWidth}
                    lineHeight={lineHeight}
                    baseAngle={baseAngle}
                />
            ))}
        </div>
    );
}

function Line({
    containerRef,
    lineColor,
    lineWidth,
    lineHeight,
    baseAngle,
}: {
    containerRef: React.RefObject<HTMLDivElement | null>;
    lineColor: string;
    lineWidth: string;
    lineHeight: string;
    baseAngle: number;
}) {
    const lineRef = useRef<HTMLDivElement>(null);
    const [rotate, setRotate] = useState(baseAngle);

    useEffect(() => {
        const updateRotation = (e: MouseEvent) => {
            if (!lineRef.current || !containerRef.current) return;

            const rect = lineRef.current.getBoundingClientRect();
            const centerX = rect.left + rect.width / 2;
            const centerY = rect.top + rect.height / 2;

            const mouseX = e.clientX;
            const mouseY = e.clientY;

            const distanceX = mouseX - centerX;
            const distanceY = mouseY - centerY;

            const angle = (Math.atan2(distanceY, distanceX) * 180) / Math.PI;

            setRotate(angle + baseAngle);
        };

        window.addEventListener("mousemove", updateRotation);
        return () => window.removeEventListener("mousemove", updateRotation);
    }, [baseAngle, containerRef]);

    return (
        <motion.div
            ref={lineRef}
            animate={{ rotate }}
            transition={{ type: "spring", damping: 20, stiffness: 300 }}
            style={{
                width: lineWidth,
                height: lineHeight,
                backgroundColor: lineColor,
            }}
        />
    );
}
```
---

## Magnetic Dock <a name="magnetic-dock"></a>

A macOS-style magnetic dock with smooth scaling animations, spring physics, and premium micro-interactions. Supports both light and dark modes.

- **Registry Key**: `magnetic-dock`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/magnetic-dock.json
```

#### Dependencies
- **NPM Packages**:
  - `framer-motion`

#### How to Use
```tsx
"use client";

import React from "react";
import {
  MagneticDock,
  DockIconHome,
  DockIconSearch,
  DockIconFolder,
  DockIconMail,
  DockIconMusic,
  DockIconSettings,
  DockIconTrash,
} from "@workspace/ui/components/magnetic-dock";

const defaultItems = [
  { id: "home", label: "Home", icon: <DockIconHome />, isActive: true },
  { id: "search", label: "Search", icon: <DockIconSearch /> },
  { id: "folder", label: "Finder", icon: <DockIconFolder /> },
  { id: "mail", label: "Mail", icon: <DockIconMail />, badge: 3 },
  { id: "music", label: "Music", icon: <DockIconMusic /> },
  { id: "settings", label: "Settings", icon: <DockIconSettings /> },
  { id: "trash", label: "Trash", icon: <DockIconTrash /> },
];

const minimalItems = [
  { id: "home", label: "Home", icon: <DockIconHome /> },
  { id: "search", label: "Search", icon: <DockIconSearch /> },
  { id: "folder", label: "Finder", icon: <DockIconFolder /> },
  { id: "settings", label: "Settings", icon: <DockIconSettings /> },
];

export function MagneticDockDefaultPreview() {
  return (
    <div className="flex items-center justify-center w-full h-full p-8">
      <MagneticDock items={defaultItems} />
    </div>
  );
}

export function MagneticDockSolidPreview() {
  return (
    <div className="flex items-center justify-center w-full h-full p-8">
      <MagneticDock items={minimalItems} variant="solid" />
    </div>
  );
}

export function MagneticDockLargeScalePreview() {
  return (
    <div className="flex items-center justify-center w-full h-full p-8">
      <MagneticDock
        items={minimalItems}
        iconSize={48}
        maxScale={2}
        magneticDistance={200}
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/magnetic-dock.tsx`
```tsx
"use client"

import * as React from "react"
import { motion, useMotionValue, useSpring, useTransform, AnimatePresence, type MotionValue } from "framer-motion"
import { cn } from "@/lib/utils"

interface MagneticDockProps {
    /** Array of dock items */
    items: DockItemData[]
    /** Size of icons in pixels */
    iconSize?: number
    /** Maximum scale on hover */
    maxScale?: number
    /** Distance of magnetic effect in pixels */
    magneticDistance?: number
    /** Show labels on hover */
    showLabels?: boolean
    /** Dock position */
    position?: "bottom" | "top" | "left" | "right"
    /** Background style */
    variant?: "glass" | "solid" | "transparent"
    /** Custom class name */
    className?: string
}

interface DockItemData {
    /** Unique identifier */
    id: string
    /** Display label */
    label: string
    /** Icon component or image URL */
    icon: React.ReactNode
    /** Click handler */
    onClick?: () => void
    /** Whether item is active */
    isActive?: boolean
    /** Badge count */
    badge?: number
}

interface DockItemProps {
    item: DockItemData
    mouseX: MotionValue<number>
    iconSize: number
    maxScale: number
    magneticDistance: number
    showLabels: boolean
    isVertical: boolean
}

function DockItem({
    item,
    mouseX,
    iconSize,
    maxScale,
    magneticDistance,
    showLabels,
    isVertical,
}: DockItemProps) {
    const ref = React.useRef<HTMLButtonElement>(null)
    const [isHovered, setIsHovered] = React.useState(false)

    // Calculate distance from mouse to center of item
    const distance = useTransform(mouseX, (val: number) => {
        if (!ref.current) return magneticDistance + 1
        const rect = ref.current.getBoundingClientRect()
        const center = isVertical
            ? rect.top + rect.height / 2
            : rect.left + rect.width / 2
        return val - center
    })

    // Scale based on distance - closer = larger
    const scale = useTransform(distance, [-magneticDistance, 0, magneticDistance], [1, maxScale, 1])

    // Apply spring physics for smooth animation
    const springConfig = { damping: 20, stiffness: 300, mass: 0.5 }
    const smoothScale = useSpring(scale, springConfig)

    // Calculate the size based on scale
    const size = useTransform(smoothScale, (s) => s * iconSize)

    // Floating effect
    const y = useTransform(smoothScale, (s) => (s - 1) * -10)
    const smoothY = useSpring(y, springConfig)

    return (
        <motion.button
            ref={ref}
            onClick={item.onClick}
            onMouseEnter={() => setIsHovered(true)}
            onMouseLeave={() => setIsHovered(false)}
            className={cn(
                "relative flex items-center justify-center",
                "rounded-2xl transition-colors duration-200",
                "focus:outline-none focus-visible:ring-2 focus-visible:ring-neutral-400 dark:focus-visible:ring-white/50",
                item.isActive && "bg-neutral-200/50 dark:bg-white/10"
            )}
            style={{
                width: size,
                height: size,
                y: isVertical ? 0 : smoothY,
                x: isVertical ? smoothY : 0,
            }}
            whileTap={{ scale: 0.9 }}
        >
            {/* Icon Container */}
            <motion.div
                className={cn(
                    "relative w-full h-full rounded-2xl overflow-hidden",
                    "bg-gradient-to-b from-neutral-100 to-neutral-50",
                    "dark:from-neutral-800 dark:to-neutral-900",
                    "backdrop-blur-sm",
                    "border border-neutral-300 dark:border-neutral-700",
                    "shadow-lg shadow-black/10 dark:shadow-black/30",
                    "flex items-center justify-center",
                    "transition-all duration-200"
                )}
                style={{
                    boxShadow: isHovered
                        ? "0 8px 32px rgba(0,0,0,0.15), inset 0 1px 0 rgba(255,255,255,0.5)"
                        : "0 4px 12px rgba(0,0,0,0.08), inset 0 1px 0 rgba(255,255,255,0.3)",
                }}
            >
                {/* Icon */}
                <div className="w-[60%] h-[60%] flex items-center justify-center text-neutral-700 dark:text-white">
                    {item.icon}
                </div>

                {/* Shine effect */}
                <motion.div
                    className="absolute inset-0 pointer-events-none"
                    style={{
                        background:
                            "linear-gradient(135deg, rgba(255,255,255,0.6) 0%, transparent 50%, transparent 100%)",
                        opacity: isHovered ? 0.9 : 0.5,
                    }}
                />
            </motion.div>

            {/* Badge */}
            <AnimatePresence>
                {item.badge !== undefined && item.badge > 0 && (
                    <motion.div
                        initial={{ scale: 0, opacity: 0 }}
                        animate={{ scale: 1, opacity: 1 }}
                        exit={{ scale: 0, opacity: 0 }}
                        className={cn(
                            "absolute -top-1 -right-1",
                            "min-w-[20px] h-5 px-1.5",
                            "rounded-full",
                            "bg-red-500",
                            "text-white text-xs font-semibold",
                            "flex items-center justify-center",
                            "border-2 border-white dark:border-neutral-950",
                            "shadow-lg"
                        )}
                    >
                        {item.badge > 99 ? "99+" : item.badge}
                    </motion.div>
                )}
            </AnimatePresence>

            {/* Active Indicator */}
            <AnimatePresence>
                {item.isActive && (
                    <motion.div
                        initial={{ scale: 0, opacity: 0 }}
                        animate={{ scale: 1, opacity: 1 }}
                        exit={{ scale: 0, opacity: 0 }}
                        className={cn(
                            "absolute -bottom-2",
                            "w-1.5 h-1.5 rounded-full",
                            "bg-neutral-600 dark:bg-white/80"
                        )}
                    />
                )}
            </AnimatePresence>

            {/* Tooltip */}
            <AnimatePresence>
                {showLabels && isHovered && (
                    <motion.div
                        initial={{ opacity: 0, y: 8, scale: 0.9 }}
                        animate={{ opacity: 1, y: 0, scale: 1 }}
                        exit={{ opacity: 0, y: 8, scale: 0.9 }}
                        transition={{ duration: 0.15, ease: "easeOut" }}
                        className={cn(
                            "absolute -top-10 left-1/2 -translate-x-1/2",
                            "px-3 py-1.5 rounded-lg",
                            "bg-white dark:bg-neutral-900/95",
                            "backdrop-blur-sm",
                            "text-neutral-800 dark:text-white text-sm font-medium whitespace-nowrap",
                            "border border-neutral-200 dark:border-white/10",
                            "shadow-xl shadow-black/10 dark:shadow-black/20",
                            "pointer-events-none z-50"
                        )}
                    >
                        {item.label}
                        {/* Tooltip arrow */}
                        <div
                            className={cn(
                                "absolute left-1/2 -translate-x-1/2 -bottom-1",
                                "w-2 h-2 rotate-45",
                                "bg-white dark:bg-neutral-900/95",
                                "border-r border-b border-neutral-200 dark:border-white/10"
                            )}
                        />
                    </motion.div>
                )}
            </AnimatePresence>

            {/* Hover glow */}
            <motion.div
                className="absolute inset-0 rounded-2xl pointer-events-none"
                animate={{
                    boxShadow: isHovered
                        ? "0 0 30px rgba(255,255,255,0.15)"
                        : "0 0 0px rgba(255,255,255,0)",
                }}
                transition={{ duration: 0.3 }}
            />
        </motion.button>
    )
}

function MagneticDock({
    items,
    iconSize = 56,
    maxScale = 1.5,
    magneticDistance = 150,
    showLabels = true,
    position = "bottom",
    variant = "glass",
    className,
}: MagneticDockProps) {
    const mousePosition = useMotionValue(Infinity)
    const isVertical = position === "left" || position === "right"

    const handleMouseMove = React.useCallback(
        (e: React.MouseEvent) => {
            if (isVertical) {
                mousePosition.set(e.clientY)
            } else {
                mousePosition.set(e.clientX)
            }
        },
        [mousePosition, isVertical]
    )

    const handleMouseLeave = () => {
        mousePosition.set(Infinity)
    }

    const variantStyles = {
        glass: cn(
            "bg-white/80 dark:bg-neutral-900/80",
            "backdrop-blur-xl backdrop-saturate-150",
            "border border-neutral-200 dark:border-neutral-700"
        ),
        solid: cn(
            "bg-neutral-100 dark:bg-neutral-900",
            "border border-neutral-300 dark:border-neutral-700"
        ),
        transparent: "bg-transparent border-0",
    }

    const positionStyles = {
        bottom: "flex-row",
        top: "flex-row",
        left: "flex-col",
        right: "flex-col",
    }

    return (
        <motion.div
            onMouseMove={handleMouseMove}
            onMouseLeave={handleMouseLeave}
            className={cn(
                "inline-flex items-end gap-2 p-3 rounded-3xl",
                variantStyles[variant],
                positionStyles[position],
                "shadow-xl shadow-black/10 dark:shadow-black/30",
                className
            )}
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.5, ease: [0.25, 0.46, 0.45, 0.94] }}
        >
            {items.map((item) => (
                <DockItem
                    key={item.id}
                    item={item}
                    mouseX={mousePosition}
                    iconSize={iconSize}
                    maxScale={maxScale}
                    magneticDistance={magneticDistance}
                    showLabels={showLabels}
                    isVertical={isVertical}
                />
            ))}
        </motion.div>
    )
}

// Preset icons for common use cases
function DockIconHome({ className }: { className?: string }) {
    return (
        <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            strokeWidth="2"
            strokeLinecap="round"
            strokeLinejoin="round"
            className={cn("w-full h-full", className)}
        >
            <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z" />
            <polyline points="9 22 9 12 15 12 15 22" />
        </svg>
    )
}

function DockIconSearch({ className }: { className?: string }) {
    return (
        <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            strokeWidth="2"
            strokeLinecap="round"
            strokeLinejoin="round"
            className={cn("w-full h-full", className)}
        >
            <circle cx="11" cy="11" r="8" />
            <line x1="21" y1="21" x2="16.65" y2="16.65" />
        </svg>
    )
}

function DockIconFolder({ className }: { className?: string }) {
    return (
        <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            strokeWidth="2"
            strokeLinecap="round"
            strokeLinejoin="round"
            className={cn("w-full h-full", className)}
        >
            <path d="M22 19a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h5l2 3h9a2 2 0 0 1 2 2z" />
        </svg>
    )
}

function DockIconMail({ className }: { className?: string }) {
    return (
        <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            strokeWidth="2"
            strokeLinecap="round"
            strokeLinejoin="round"
            className={cn("w-full h-full", className)}
        >
            <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z" />
            <polyline points="22,6 12,13 2,6" />
        </svg>
    )
}

function DockIconMusic({ className }: { className?: string }) {
    return (
        <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            strokeWidth="2"
            strokeLinecap="round"
            strokeLinejoin="round"
            className={cn("w-full h-full", className)}
        >
            <path d="M9 18V5l12-2v13" />
            <circle cx="6" cy="18" r="3" />
            <circle cx="18" cy="16" r="3" />
        </svg>
    )
}

function DockIconSettings({ className }: { className?: string }) {
    return (
        <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            strokeWidth="2"
            strokeLinecap="round"
            strokeLinejoin="round"
            className={cn("w-full h-full", className)}
        >
            <circle cx="12" cy="12" r="3" />
            <path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z" />
        </svg>
    )
}

function DockIconTrash({ className }: { className?: string }) {
    return (
        <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            strokeWidth="2"
            strokeLinecap="round"
            strokeLinejoin="round"
            className={cn("w-full h-full", className)}
        >
            <polyline points="3 6 5 6 21 6" />
            <path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2" />
        </svg>
    )
}

export {
    MagneticDock,
    DockIconHome,
    DockIconSearch,
    DockIconFolder,
    DockIconMail,
    DockIconMusic,
    DockIconSettings,
    DockIconTrash,
    type MagneticDockProps,
    type DockItemData,
}
```
---

## Matrix Rain <a name="matrix-rain"></a>

- **Registry Key**: `matrix-rain`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/matrix-rain.json
```

#### How to Use
```tsx
"use client";

import { MatrixRain } from "@workspace/ui/components/matrix-rain";

export function MatrixRainDemo() {
  return (
    <div className="relative h-full w-full bg-background overflow-hidden">
      <MatrixRain transparent />
      <div className="absolute inset-0 flex items-center justify-center pointer-events-none">
        <h1 className="text-4xl font-bold text-foreground tracking-wider">
          MATRIX
        </h1>
      </div>
    </div>
  );
}

export function MatrixRainRainbowDemo() {
  return (
    <div className="relative h-full w-full bg-background overflow-hidden">
      <MatrixRain variant="rainbow" transparent />
      <div className="absolute inset-0 flex items-center justify-center pointer-events-none">
        <h1 className="text-4xl font-bold text-foreground tracking-widest">
          RGB
        </h1>
      </div>
    </div>
  );
}

export function MatrixRainCustomDemo() {
  return (
    <div className="relative h-full w-full bg-background overflow-hidden">
      <MatrixRain fixedColor="#ec4899" speed={80} fontSize={20} transparent />
      <div className="absolute inset-0 flex items-center justify-center pointer-events-none">
        <h1 className="text-4xl font-bold text-foreground">CYBERPUNK</h1>
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/matrix-rain.tsx`
```tsx
"use client"

import { cn } from "@/lib/utils"
import { useEffect, useRef } from "react"

interface MatrixRainProps {
    className?: string
    variant?: "default" | "cyan" | "rainbow"
    width?: number
    height?: number
    fontSize?: number
    speed?: number
    fixedColor?: string
}

export function MatrixRain({
    className,
    variant = "default",
    width,
    height,
    fontSize = 16,
    speed = 50,
    fixedColor,
}: MatrixRainProps) {
    const canvasRef = useRef<HTMLCanvasElement>(null)

    useEffect(() => {
        const canvas = canvasRef.current
        if (!canvas) return

        const ctx = canvas.getContext("2d")
        if (!ctx) return

        // If specific dimensions are provided, use them. Otherwise observe parent.
        const resizeObserver = new ResizeObserver(() => {
            if (!width && !height) {
                canvas.width = canvas.offsetWidth
                canvas.height = canvas.offsetHeight
            }
        })
        resizeObserver.observe(canvas)

        // Initial size setup
        if (width) canvas.width = width
        if (height) canvas.height = height
        if (!width && !height) {
            canvas.width = canvas.offsetWidth
            canvas.height = canvas.offsetHeight
        }

        const w = canvas.width
        const h = canvas.height

        // Columns config
        const columns = Math.floor(w / fontSize)
        const drops = new Array(columns).fill(1)

        // Character set: Katakana + Numbers
        const chars = "ｦｧｨｩｪｫｬｭｮｯｰｱｲｳｴｵｶｷｸｹｺｻｼｽｾｿﾀﾁﾂﾃﾄﾅﾆﾇﾈﾉﾊﾋﾌﾍﾎﾏﾐﾑﾒﾓﾔﾕﾖﾗﾘﾙﾚﾛﾜﾝ1234567890"

        let isDark = document.documentElement.classList.contains("dark")

        // Set default background based on theme immediately to avoid delay
        const bg = isDark ? "#000000" : "#ffffff"

        ctx.fillStyle = bg
        ctx.fillRect(0, 0, w, h)

        const draw = () => {
            // Check theme state on every frame
            const currentIsDark = document.documentElement.classList.contains("dark")

            // If theme changed, reset the background immediately
            if (currentIsDark !== isDark) {
                isDark = currentIsDark
                ctx.fillStyle = isDark ? "#000000" : "#ffffff"
                ctx.fillRect(0, 0, canvas.width, canvas.height)
            }

            // Semi-transparent color for trail effect
            // Use black for dark mode, white for light mode
            ctx.fillStyle = isDark ? "rgba(0, 0, 0, 0.05)" : "rgba(255, 255, 255, 0.05)"
            ctx.fillRect(0, 0, canvas.width, canvas.height)

            ctx.font = `${fontSize}px monospace`

            for (let i = 0; i < drops.length; i++) {
                const text = chars[Math.floor(Math.random() * chars.length)] || ""

                // Color selection
                if (variant === "rainbow" && !fixedColor) {
                    const hue = (Date.now() / 20 + i * 10) % 360
                    ctx.fillStyle = `hsl(${hue}, 100%, 50%)`
                } else if (fixedColor) {
                    ctx.fillStyle = fixedColor
                } else {
                    // Adaptive colors based on background
                    if (variant === "cyan") {
                        ctx.fillStyle = isDark ? "#0FF" : "#0e7490" // Cyan-700
                    } else {
                        // Default to green
                        ctx.fillStyle = isDark ? "#0F0" : "#15803d" // Green-700
                    }
                }

                const x = i * fontSize
                const y = drops[i] * fontSize

                ctx.fillText(text, x, y)

                if (y > canvas.height && Math.random() > 0.975) {
                    drops[i] = 0
                }

                drops[i]++
            }
        }

        const interval = setInterval(draw, speed)

        return () => {
            clearInterval(interval)
            resizeObserver.disconnect()
        }
    }, [variant, fontSize, speed, fixedColor, width, height])

    return (
        <canvas
            ref={canvasRef}
            className={cn("size-full bg-background block rounded-[inherit]", className)}
            style={{ width, height }}
        />
    )
}
```
---

## Music Player <a name="music-player"></a>

Component for music-player

- **Registry Key**: `music-player`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/music-player.json
```

#### How to Use
```tsx
"use client";

import * as React from "react";
import { MusicPlayer } from "@workspace/ui/components/music-player";

export function MusicPlayerPreview() {
  return (
    <div className="flex flex-col items-center justify-center p-8 gap-12">
      <MusicPlayer
        src="https://www.youtube.com/watch?v=dQw4w9WgXcQ"
        coverArt="https://i.scdn.co/image/ab67616d0000b27315ebbedaacef61af244262a8"
        className="w-full h-full max-w-sm"
      />
      <div className="text-center space-y-2">
        <h3 className="text-xl font-bold tracking-tight">Click the Record to Play</h3>
        <p className="text-sm text-muted-foreground w-64">
          Features a spinning record animation and a tonearm overlay.
        </p>
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/music-player.tsx`
```tsx
"use client";

import React, { useRef, useState, useEffect } from "react";
import { cn } from "@/lib/utils";
import { motion } from "framer-motion";

export interface MusicPlayerProps extends React.HTMLAttributes<HTMLDivElement> {
  /** The source URL of the audio file or YouTube video */
  src: string;
  /** The URL of the album cover image */
  coverArt: string;
  /** Whether to auto-play the audio when loaded */
  autoPlay?: boolean;
}

export function MusicPlayer({
  className,
  src,
  coverArt,
  autoPlay = false,
  ...props
}: MusicPlayerProps) {
  const [isPlaying, setIsPlaying] = useState(autoPlay);
  const audioRef = useRef<HTMLAudioElement | null>(null);
  const iframeRef = useRef<HTMLIFrameElement | null>(null);

  // Extract YouTube ID if it's a YouTube URL
  const getYoutubeId = (url: string) => {
    const match = url.match(
      /(?:youtu\.be\/|youtube\.com\/(?:embed\/|v\/|watch\?v=|watch\?.+&v=))([^&?]+)/
    );
    return match ? match[1] : null;
  };
  
  const youtubeId = src ? getYoutubeId(src) : null;

  useEffect(() => {
    if (isPlaying) {
      if (youtubeId && iframeRef.current?.contentWindow) {
        iframeRef.current.contentWindow.postMessage(
          JSON.stringify({ event: "command", func: "playVideo", args: [] }),
          "*"
        );
      } else {
        audioRef.current?.play().catch(() => setIsPlaying(false));
      }
    } else {
      if (youtubeId && iframeRef.current?.contentWindow) {
        iframeRef.current.contentWindow.postMessage(
          JSON.stringify({ event: "command", func: "pauseVideo", args: [] }),
          "*"
        );
      } else {
        audioRef.current?.pause();
      }
    }
  }, [isPlaying, youtubeId]);

  const togglePlay = () => {
    setIsPlaying(!isPlaying);
  };

  return (
    <div
      className={cn("relative inline-flex flex-col items-center", className)}
      {...props}
    >
      {youtubeId ? (
        <iframe
          ref={iframeRef}
          className="hidden"
          src={`https://www.youtube.com/embed/${youtubeId}?enablejsapi=1&autoplay=${
            autoPlay ? 1 : 0
          }&controls=0`}
          allow="autoplay"
        />
      ) : (
        <audio
          ref={audioRef}
          src={src}
          onEnded={() => setIsPlaying(false)}
          className="hidden"
        />
      )}

      <div
        className="relative cursor-pointer select-none h-64 w-64 md:h-80 md:w-80"
        onClick={togglePlay}
        title={isPlaying ? "Pause" : "Play"}
      >
        {/* Tonearm */}
        <motion.div
          className="absolute z-20 top-[-5%] right-[-10%] sm:top-[-8%] sm:right-[-15%] origin-top-right w-[60%] h-[15%] pointer-events-none"
          initial={{ rotate: 10 }}
          animate={{ rotate: isPlaying ? -20 : 10 }}
          transition={{ duration: 0.5, ease: "easeInOut" }}
        >
          {/* Tonearm base */}
          <div className="absolute top-0 right-0 w-8 h-8 md:w-10 md:h-10 rounded-full bg-zinc-400 dark:bg-zinc-600 shadow-md transform translate-x-1/2 -translate-y-1/2 border-4 border-zinc-200 dark:border-zinc-800 z-10" />
          {/* Tonearm stick & Needle */}
          <div className="absolute top-0 right-[10px] sm:right-[15px] w-[90%] h-2 md:h-3 bg-zinc-400 dark:bg-zinc-500 rounded-full origin-right -rotate-12 shadow-sm flex items-center justify-start">
            {/* Needle */}
            <div className="w-4 h-4 md:w-5 md:h-5 bg-zinc-800 dark:bg-zinc-300 rounded-full shadow-md transform -translate-x-1/2" />
          </div>
        </motion.div>

        {/* Record Disc */}
        <div
          className={cn(
            "relative w-full h-full rounded-full border-4 sm:border-8 border-black/10 dark:border-white/10 shadow-xl overflow-hidden shadow-black/30 bg-black animate-spin"
          )}
          style={{
            animationDuration: "4s",
            animationPlayState: isPlaying ? "running" : "paused",
          }}
        >
          {/* Album Cover Background */}
          <div
            className="absolute inset-0 bg-cover bg-center opacity-90 transition-opacity"
            style={{ backgroundImage: `url(${coverArt})` }}
          />

          {/* Grooves Overlay (Multiple dark gradient rings) */}
          <div
            className="absolute inset-0 rounded-full border border-black/20"
            style={{
              background:
                "radial-gradient(circle, transparent 20%, rgba(0,0,0,0.4) 21%, transparent 22%, transparent 35%, rgba(0,0,0,0.5) 36%, transparent 37%, transparent 50%, rgba(0,0,0,0.3) 51%, transparent 52%, transparent 65%, rgba(0,0,0,0.6) 66%, transparent 67%, transparent 80%, rgba(0,0,0,0.4) 81%, transparent 82%)",
            }}
          />

          {/* Glare effect */}
          <div
            className="absolute inset-0 rounded-full pointer-events-none"
            style={{
              background:
                "linear-gradient(135deg, rgba(255,255,255,0.4) 0%, transparent 40%, transparent 60%, rgba(255,255,255,0.2) 100%)",
            }}
          />

          {/* Center Hole and Label Area */}
          <div className="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-1/3 h-1/3 rounded-full bg-zinc-900 border border-zinc-700 shadow-inner flex items-center justify-center">
            {/* The very center pin hole */}
            <div className="w-3 h-3 md:w-4 md:h-4 bg-zinc-300 dark:bg-zinc-600 rounded-full shadow-inner border border-black/40" />
          </div>
        </div>
      </div>
    </div>
  );
}
```
---

## Noise Texture <a name="noise-texture"></a>

An animated noise/grain texture overlay effect with customizable grain size and blend modes.

- **Registry Key**: `noise-texture`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/noise-texture.json
```

#### How to Use
```tsx
"use client";

import { NoiseTexture } from "@workspace/ui/components/noise-texture";

export function NoiseTextureDemo() {
  return (
    <div className="relative h-full w-full overflow-hidden bg-gradient-to-br from-violet-500 to-pink-500">
      <NoiseTexture />
      <div className="relative z-10 flex h-full items-center justify-center">
        <h3 className="text-2xl font-bold text-white">Noise Texture</h3>
      </div>
    </div>
  );
}

export function NoiseTextureCoarseDemo() {
  return (
    <div className="relative h-full w-full overflow-hidden bg-gradient-to-br from-emerald-500 to-cyan-500">
      <NoiseTexture grain="coarse" opacity={0.2} />
      <div className="relative z-10 flex h-full items-center justify-center">
        <h3 className="text-2xl font-bold text-white">Coarse Grain</h3>
      </div>
    </div>
  );
}

export function NoiseTextureStaticDemo() {
  return (
    <div className="relative h-full w-full overflow-hidden bg-gradient-to-br from-orange-500 to-red-500">
      <NoiseTexture grain="fine" opacity={0.25} animate={false} />
      <div className="relative z-10 flex h-full items-center justify-center">
        <h3 className="text-2xl font-bold text-white">Static Noise</h3>
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/noise-texture.tsx`
```tsx
"use client"

import { useEffect, useRef } from "react"
import { cn } from "@/lib/utils"

interface NoiseTextureProps {
  className?: string
  opacity?: number
  speed?: number
  grain?: "fine" | "medium" | "coarse"
  blend?: "overlay" | "soft-light" | "multiply" | "screen" | "normal"
  animate?: boolean
}

export function NoiseTexture({
  className,
  opacity = 0.4,
  speed = 10,
  grain = "medium",
  blend = "normal",
  animate = true,
}: NoiseTextureProps) {
  const canvasRef = useRef<HTMLCanvasElement>(null)
  const animationRef = useRef<number | NodeJS.Timeout>(0)

  useEffect(() => {
    const canvas = canvasRef.current
    if (!canvas) return

    const ctx = canvas.getContext("2d")
    if (!ctx) return

    const grainSizes: Record<string, number> = {
      fine: 1,
      medium: 2,
      coarse: 3,
    }
    const grainSize = grainSizes[grain] ?? 2

    const resize = () => {
      const rect = canvas.getBoundingClientRect()
      const dpr = Math.min(window.devicePixelRatio || 1, 2)
      canvas.width = Math.ceil(rect.width / grainSize) * dpr
      canvas.height = Math.ceil(rect.height / grainSize) * dpr
      canvas.style.width = `${rect.width}px`
      canvas.style.height = `${rect.height}px`
    }

    resize()
    window.addEventListener("resize", resize)

    const renderNoise = () => {
      const { width, height } = canvas
      if (width === 0 || height === 0) return
      
      const imageData = ctx.createImageData(width, height)
      const data = imageData.data

      for (let i = 0; i < data.length; i += 4) {
        const value = Math.random() * 255
        data[i] = value
        data[i + 1] = value
        data[i + 2] = value
        data[i + 3] = 255
      }

      ctx.putImageData(imageData, 0, 0)
    }

    renderNoise()

    if (animate) {
      const animateNoise = () => {
        renderNoise()
        animationRef.current = setTimeout(() => {
          requestAnimationFrame(animateNoise)
        }, 1000 / speed)
      }
      animateNoise()
    }

    return () => {
      window.removeEventListener("resize", resize)
      if (animationRef.current) {
        clearTimeout(animationRef.current as NodeJS.Timeout)
      }
    }
  }, [grain, speed, animate])

  const grainSizes: Record<string, number> = {
    fine: 1,
    medium: 2,
    coarse: 3,
  }

  return (
    <canvas
      ref={canvasRef}
      className={cn("pointer-events-none absolute inset-0", className)}
      style={{
        opacity,
        mixBlendMode: blend,
        imageRendering: "pixelated",
        width: "100%",
        height: "100%",
        transform: `scale(${grainSizes[grain] ?? 2})`,
        transformOrigin: "top left",
      }}
    />
  )
}
```
---

## Particle Galaxy <a name="particle-galaxy"></a>

- **Registry Key**: `particle-galaxy`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/particle-galaxy.json
```

#### Dependencies
- **NPM Packages**:
  - `three`

#### How to Use
```tsx
"use client";

import { useState, useEffect } from "react";
import { useTheme } from "next-themes";
import { ParticleGalaxy } from "@workspace/ui/components/particle-galaxy";

interface ParticleGalaxyPreviewProps {
  particleCount?: number;
  particleSize?: number;
  rotationSpeed?: number;
  spiralArms?: number;
  colors?: [string, string, string];
  mouseInfluence?: number;
  autoRotate?: boolean;
  blendMode?: "additive" | "normal";
  spread?: number;
  density?: number;
  glow?: number;
  cameraMovement?: boolean;
  centerConcentration?: number;
  pulsate?: boolean;
  pulsateSpeed?: number;
}

function ParticleGalaxyWrapper(props: ParticleGalaxyPreviewProps) {
  const { resolvedTheme } = useTheme();
  const [mounted, setMounted] = useState(false);

  useEffect(() => {
    setMounted(true);
  }, []);

  const effectiveBlendMode =
    props.blendMode ||
    (mounted && resolvedTheme === "light" ? "normal" : "additive");

  return (
    <div className="relative w-full h-full">
      <ParticleGalaxy
        {...props}
        blendMode={effectiveBlendMode}
        className="h-full"
      />
    </div>
  );
}

export function ParticleGalaxyDemo() {
  return <ParticleGalaxyWrapper />;
}

export function ParticleGalaxyCustomDemo() {
  return (
    <ParticleGalaxyWrapper
      colors={["#10b981", "#06b6d4", "#3b82f6"]}
      spiralArms={5}
      particleCount={15000}
      spread={3.5}
    />
  );
}

export function ParticleGalaxyDenseDemo() {
  return (
    <ParticleGalaxyWrapper
      colors={["#f97316", "#ef4444", "#ec4899"]}
      particleCount={20000}
      particleSize={0.025}
      centerConcentration={0.8}
      density={0.9}
      glow={80}
      spread={2}
    />
  );
}

export function ParticleGalaxySlowDemo() {
  return (
    <ParticleGalaxyWrapper
      colors={["#8b5cf6", "#ec4899", "#f97316"]}
      rotationSpeed={0.0005}
      mouseInfluence={0.8}
      cameraMovement={false}
      pulsate={false}
    />
  );
}
```
#### Component Source Code
##### File: `components/ui/particle-galaxy.tsx`
```tsx
"use client"

import { cn } from "@/lib/utils"
import { useEffect, useRef, useState } from "react"
import * as THREE from "three"
import { WebGLErrorBoundary, WebGLFallback } from "./webgl-error-boundary"

interface ParticleGalaxyProps {
    className?: string
    /** Number of particles in the galaxy (affects performance) */
    particleCount?: number
    /** Base size of particles */
    particleSize?: number
    /** Speed of automatic rotation */
    rotationSpeed?: number
    /** Number of spiral arms */
    spiralArms?: number
    /** Array of 3 color values for the galaxy */
    colors?: [string, string, string]
    /** Strength of mouse interaction (0-1) */
    mouseInfluence?: number
    /** Enable automatic rotation */
    autoRotate?: boolean
    /** Blend mode: 'additive' for dark backgrounds, 'normal' for light backgrounds */
    blendMode?: "additive" | "normal"
    /** How spread out the galaxy is (1-5) */
    spread?: number
    /** Particle density/opacity (0-1) */
    density?: number
    /** Glow intensity (0-100) */
    glow?: number
    /** Enable gentle camera movement */
    cameraMovement?: boolean
    /** Concentration of particles toward center (0-1) */
    centerConcentration?: number
    /** Enable particle pulsation animation */
    pulsate?: boolean
    /** Speed of particle pulsation (0.1-2) */
    pulsateSpeed?: number
    /** Enable mouse wheel zoom */
    enableZoom?: boolean
    /** Enable click and drag rotation */
    enableDrag?: boolean
    /** Enable touch gestures on mobile */
    enableTouch?: boolean
    /** Rotation damping factor (0-1) - higher = more responsive */
    damping?: number
    /** Min zoom distance */
    minZoom?: number
    /** Max zoom distance */
    maxZoom?: number
}

export function ParticleGalaxy({
    className,
    particleCount = 10000,
    particleSize = 0.02,
    rotationSpeed = 0.001,
    spiralArms = 3,
    colors: colorProp = ["#4f46e5", "#8b5cf6", "#ec4899"],
    mouseInfluence = 0.5,
    autoRotate = true,
    blendMode = "additive",
    spread = 2.5,
    density = 0.7,
    glow = 60,
    cameraMovement = true,
    centerConcentration = 0.5,
    pulsate = true,
    pulsateSpeed = 1,
    enableZoom = true,
    enableDrag = true,
    enableTouch = true,
    damping = 0.1,
    minZoom = 1.5,
    maxZoom = 10,
}: ParticleGalaxyProps) {
    const containerRef = useRef<HTMLDivElement>(null)
    const canvasRef = useRef<HTMLCanvasElement>(null)
    const rendererRef = useRef<THREE.WebGLRenderer | null>(null)
    const sceneRef = useRef<THREE.Scene | null>(null)
    const cameraRef = useRef<THREE.PerspectiveCamera | null>(null)
    const particlesRef = useRef<THREE.Points | null>(null)
    const mouseRef = useRef({ x: 0, y: 0 })
    const frameRef = useRef<number | undefined>(undefined)
    const [dimensions, setDimensions] = useState({ width: 0, height: 0 })
    const [hasWebGLError, setHasWebGLError] = useState(false)

    // 3D interaction state
    const isDraggingRef = useRef(false)
    const previousMouseRef = useRef({ x: 0, y: 0 })
    const targetRotationRef = useRef({ x: 0, y: 0 })
    const currentRotationRef = useRef({ x: 0, y: 0 })
    const currentTiltRef = useRef({ x: 0, y: 0 })
    const targetZoomRef = useRef(3)
    const currentZoomRef = useRef(3)

    useEffect(() => {
        if (!containerRef.current || !canvasRef.current) return

        const container = containerRef.current

        // Get initial dimensions
        const updateDimensions = () => {
            const rect = container.getBoundingClientRect()
            setDimensions({ width: rect.width, height: rect.height })
        }

        updateDimensions()

        // Setup ResizeObserver
        const resizeObserver = new ResizeObserver(updateDimensions)
        resizeObserver.observe(container)

        return () => {
            resizeObserver.disconnect()
        }
    }, [])

    useEffect(() => {
        if (hasWebGLError) return
        if (!canvasRef.current || !containerRef.current || dimensions.width === 0 || dimensions.height === 0)
            return

        try {
            const canvas = canvasRef.current
            const container = containerRef.current

            // Scene
            const scene = new THREE.Scene()
            sceneRef.current = scene

            // Camera
            const camera = new THREE.PerspectiveCamera(
                75,
                dimensions.width / dimensions.height,
                0.1,
                1000
            )
            camera.position.z = 3
            cameraRef.current = camera

            // Renderer with transparent background
            const renderer = new THREE.WebGLRenderer({
                canvas,
                alpha: true, // Transparent background
                antialias: true,
            })
            renderer.setSize(dimensions.width, dimensions.height)
            renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
            rendererRef.current = renderer

            // Create galaxy particles
            const geometry = new THREE.BufferGeometry()
            const positions = new Float32Array(particleCount * 3)
            const colors = new Float32Array(particleCount * 3)
            const scales = new Float32Array(particleCount)

            // Convert hex colors to RGB
            const colorPalette = [
                new THREE.Color(colorProp[0]),
                new THREE.Color(colorProp[1]),
                new THREE.Color(colorProp[2]),
            ]

            for (let i = 0; i < particleCount; i++) {
            const i3 = i * 3

            // Spiral galaxy distribution with customizable spread
            const radius = Math.pow(Math.random(), centerConcentration) * spread
            const spinAngle = radius * spiralArms
            const branchAngle =
                ((i % spiralArms) / spiralArms) * Math.PI * 2 + spinAngle

            // Randomness for organic feel
            const randomnessStrength = 0.3 * (spread / 2.5)
            const randomX =
                Math.pow(Math.random(), 3) *
                (Math.random() < 0.5 ? 1 : -1) *
                randomnessStrength
            const randomY =
                Math.pow(Math.random(), 3) *
                (Math.random() < 0.5 ? 1 : -1) *
                randomnessStrength
            const randomZ =
                Math.pow(Math.random(), 3) *
                (Math.random() < 0.5 ? 1 : -1) *
                randomnessStrength

            positions[i3] = Math.cos(branchAngle) * radius + randomX
            positions[i3 + 1] = randomY
            positions[i3 + 2] = Math.sin(branchAngle) * radius + randomZ

            // Colors - mix based on distance from center
            const mixedColor = colorPalette[i % 3]!.clone()
            const centerDistance = radius / spread

            // For normal blend mode (light backgrounds), darken colors
            if (blendMode === "normal") {
                mixedColor.lerp(new THREE.Color("#000000"), centerDistance * 0.5)
            } else {
                // For additive blend mode (dark backgrounds), brighten toward center
                mixedColor.lerp(new THREE.Color("#ffffff"), 1 - centerDistance)
            }

            colors[i3] = mixedColor.r
            colors[i3 + 1] = mixedColor.g
            colors[i3 + 2] = mixedColor.b

            // Variable scale for depth
            scales[i] = Math.random()
        }

            geometry.setAttribute("position", new THREE.BufferAttribute(positions, 3))
            geometry.setAttribute("color", new THREE.BufferAttribute(colors, 3))
            geometry.setAttribute("scale", new THREE.BufferAttribute(scales, 1))

            // Custom shader material for better particle rendering
            const material = new THREE.ShaderMaterial({
            uniforms: {
                uTime: { value: 0 },
                uSize: { value: particleSize * 100 },
                uGlow: { value: glow / 100 },
                uDensity: { value: density },
                uPulsate: { value: pulsate ? 1.0 : 0.0 },
                uPulsateSpeed: { value: pulsateSpeed },
            },
            vertexShader: `
        uniform float uTime;
        uniform float uSize;
        uniform float uPulsate;
        uniform float uPulsateSpeed;
        attribute float scale;
        varying vec3 vColor;
        
        void main() {
          vColor = color;
          vec4 modelPosition = modelMatrix * vec4(position, 1.0);
          
          // Pulsate particles if enabled
          if (uPulsate > 0.5) {
            float pulsateValue = sin(uTime * uPulsateSpeed + position.x * 10.0) * 0.5 + 0.5;
            modelPosition.y += pulsateValue * 0.02;
          }
          
          vec4 viewPosition = viewMatrix * modelPosition;
          vec4 projectedPosition = projectionMatrix * viewPosition;
          gl_Position = projectedPosition;
          
          gl_PointSize = uSize * scale * (1.0 / -viewPosition.z);
        }
      `,
            fragmentShader: `
        varying vec3 vColor;
        uniform float uGlow;
        uniform float uDensity;
        
        void main() {
          // Circular particles with customizable glow
          float distanceToCenter = distance(gl_PointCoord, vec2(0.5));
          float strength = uGlow / distanceToCenter - (uGlow * 2.0);
          strength = max(0.0, strength);
          
          gl_FragColor = vec4(vColor, strength * uDensity);
        }
      `,
            transparent: true,
            blending:
                blendMode === "additive" ? THREE.AdditiveBlending : THREE.NormalBlending,
            depthWrite: false,
            vertexColors: true,
        })

            const particles = new THREE.Points(geometry, material)
            scene.add(particles)
            particlesRef.current = particles

            // ===== 3D INTERACTION CONTROLS =====

            // Mouse hover for subtle tilt (only when not dragging)
            const handleMouseMove = (event: MouseEvent) => {
            if (isDraggingRef.current) return
            const rect = container.getBoundingClientRect()
            mouseRef.current.x = ((event.clientX - rect.left) / rect.width) * 2 - 1
            mouseRef.current.y = -(((event.clientY - rect.top) / rect.height) * 2 - 1)
        }

            const handleMouseLeave = () => {
            if (!isDraggingRef.current) {
                mouseRef.current.x = 0
                mouseRef.current.y = 0
            }
        }

            // Drag to rotate
            const handleMouseDown = (event: MouseEvent) => {
            if (!enableDrag) return
            isDraggingRef.current = true
            previousMouseRef.current = { x: event.clientX, y: event.clientY }
            container.style.cursor = "grabbing"
        }

            const handleMouseMoveGlobal = (event: MouseEvent) => {
            if (!isDraggingRef.current || !enableDrag) return

            const deltaX = event.clientX - previousMouseRef.current.x
            const deltaY = event.clientY - previousMouseRef.current.y

            targetRotationRef.current.y += deltaX * 0.005
            targetRotationRef.current.x += deltaY * 0.005

            // Clamp vertical rotation
            targetRotationRef.current.x = Math.max(-Math.PI / 2, Math.min(Math.PI / 2, targetRotationRef.current.x))

            previousMouseRef.current = { x: event.clientX, y: event.clientY }
        }

            const handleMouseUp = () => {
            if (!enableDrag) return
            isDraggingRef.current = false
            container.style.cursor = enableDrag ? "grab" : "default"
        }

            // Mouse wheel zoom
            const handleWheel = (event: WheelEvent) => {
            if (!enableZoom) return
            event.preventDefault()

            const zoomSpeed = 0.001
            targetZoomRef.current += event.deltaY * zoomSpeed
            targetZoomRef.current = Math.max(minZoom, Math.min(maxZoom, targetZoomRef.current))
        }

            // Touch support
            let touchStartDistance = 0
            let touchStartZoom = 3

            const getTouchDistance = (touches: TouchList) => {
            if (touches.length < 2) return 0
            const dx = touches[0]!.clientX - touches[1]!.clientX
            const dy = touches[0]!.clientY - touches[1]!.clientY
            return Math.sqrt(dx * dx + dy * dy)
        }

            const handleTouchStart = (event: TouchEvent) => {
            if (!enableTouch) return

            if (event.touches.length === 2) {
                // Pinch zoom
                touchStartDistance = getTouchDistance(event.touches)
                touchStartZoom = targetZoomRef.current
            } else if (event.touches.length === 1 && enableDrag) {
                // Single touch drag
                isDraggingRef.current = true
                previousMouseRef.current = {
                    x: event.touches[0]!.clientX,
                    y: event.touches[0]!.clientY,
                }
            }
        }

            const handleTouchMove = (event: TouchEvent) => {
            if (!enableTouch) return

            if (event.touches.length === 2 && enableZoom) {
                // Pinch to zoom
                event.preventDefault()
                const currentDistance = getTouchDistance(event.touches)
                const zoomFactor = touchStartDistance / currentDistance
                targetZoomRef.current = touchStartZoom * zoomFactor
                targetZoomRef.current = Math.max(minZoom, Math.min(maxZoom, targetZoomRef.current))
            } else if (event.touches.length === 1 && isDraggingRef.current && enableDrag) {
                // Drag to rotate
                const deltaX = event.touches[0]!.clientX - previousMouseRef.current.x
                const deltaY = event.touches[0]!.clientY - previousMouseRef.current.y

                targetRotationRef.current.y += deltaX * 0.005
                targetRotationRef.current.x += deltaY * 0.005
                targetRotationRef.current.x = Math.max(-Math.PI / 2, Math.min(Math.PI / 2, targetRotationRef.current.x))

                previousMouseRef.current = {
                    x: event.touches[0]!.clientX,
                    y: event.touches[0]!.clientY,
                }
            }
        }

            const handleTouchEnd = () => {
            if (!enableTouch) return
            isDraggingRef.current = false
            touchStartDistance = 0
        }

            // Add event listeners
            container.addEventListener("mousemove", handleMouseMove)
            container.addEventListener("mouseleave", handleMouseLeave)
            container.addEventListener("mousedown", handleMouseDown)
            document.addEventListener("mousemove", handleMouseMoveGlobal)
            document.addEventListener("mouseup", handleMouseUp)
            container.addEventListener("wheel", handleWheel, { passive: false })
            container.addEventListener("touchstart", handleTouchStart, { passive: false })
            container.addEventListener("touchmove", handleTouchMove, { passive: false })
            container.addEventListener("touchend", handleTouchEnd)

            // Set cursor style
            container.style.cursor = enableDrag ? "grab" : "default"

            // Animation
            const clock = new THREE.Clock()
            const animate = () => {
            const elapsedTime = clock.getElapsedTime()

            // Update material uniforms
            if (material.uniforms && material.uniforms.uTime) {
                material.uniforms.uTime.value = elapsedTime
            }

            if (particles) {
                // Smooth damping for rotation
                currentRotationRef.current.x += (targetRotationRef.current.x - currentRotationRef.current.x) * damping
                currentRotationRef.current.y += (targetRotationRef.current.y - currentRotationRef.current.y) * damping

                // Auto-rotate when enabled and not being dragged
                if (autoRotate && !isDraggingRef.current) {
                    targetRotationRef.current.y += rotationSpeed
                }

                // Calculate target tilt based on mouse position
                const targetTiltX = (!isDraggingRef.current && mouseInfluence > 0) ? mouseRef.current.y * mouseInfluence * 0.3 : 0
                const targetTiltY = (!isDraggingRef.current && mouseInfluence > 0) ? mouseRef.current.x * mouseInfluence * 0.3 : 0

                // Smoothly interpolate current tilt
                const tiltDamping = 0.05
                currentTiltRef.current.x += (targetTiltX - currentTiltRef.current.x) * tiltDamping
                currentTiltRef.current.y += (targetTiltY - currentTiltRef.current.y) * tiltDamping

                // Apply rotation + tilt
                particles.rotation.x = currentRotationRef.current.x + currentTiltRef.current.x
                particles.rotation.y = currentRotationRef.current.y + currentTiltRef.current.y
            }

            // Smooth zoom damping
            currentZoomRef.current += (targetZoomRef.current - currentZoomRef.current) * damping
            camera.position.z = currentZoomRef.current

            // Optional gentle camera movement
            if (cameraMovement && !isDraggingRef.current) {
                camera.position.x = Math.sin(elapsedTime * 0.1) * 0.2
                camera.position.y = Math.cos(elapsedTime * 0.15) * 0.2
            } else if (isDraggingRef.current) {
                // Smoothly return camera to center when dragging
                camera.position.x += (0 - camera.position.x) * 0.1
                camera.position.y += (0 - camera.position.y) * 0.1
            }

            camera.lookAt(scene.position)

            renderer.render(scene, camera)
            frameRef.current = requestAnimationFrame(animate)
        }

            animate()

            // Cleanup
            return () => {
            container.removeEventListener("mousemove", handleMouseMove)
            container.removeEventListener("mouseleave", handleMouseLeave)
            container.removeEventListener("mousedown", handleMouseDown)
            document.removeEventListener("mousemove", handleMouseMoveGlobal)
            document.removeEventListener("mouseup", handleMouseUp)
            container.removeEventListener("wheel", handleWheel)
            container.removeEventListener("touchstart", handleTouchStart)
            container.removeEventListener("touchmove", handleTouchMove)
            container.removeEventListener("touchend", handleTouchEnd)
            if (frameRef.current) {
                cancelAnimationFrame(frameRef.current)
            }
            geometry.dispose()
            material.dispose()
            renderer.dispose()
            }
        } catch {
            setHasWebGLError(true)
            return
        }
    }, [
        hasWebGLError,
        dimensions,
        particleCount,
        particleSize,
        rotationSpeed,
        spiralArms,
        colorProp,
        mouseInfluence,
        autoRotate,
        blendMode,
        spread,
        density,
        glow,
        cameraMovement,
        centerConcentration,
        pulsate,
        pulsateSpeed,
        enableZoom,
        enableDrag,
        enableTouch,
        damping,
        minZoom,
        maxZoom,
    ])

    if (hasWebGLError) {
        return (
            <div ref={containerRef} className={cn("relative w-full h-full", className)}>
                <WebGLFallback className="absolute inset-0 h-full w-full" />
            </div>
        )
    }

    return (
        <WebGLErrorBoundary fallback={<WebGLFallback className="absolute inset-0 h-full w-full" />}>
            <div ref={containerRef} className={cn("relative w-full h-full", className)}>
                <canvas ref={canvasRef} className="w-full h-full" />
            </div>
        </WebGLErrorBoundary>
    )
}
```
##### File: `components/ui/webgl-error-boundary.tsx`
```tsx
"use client";

import { cn } from "@/lib/utils";
import * as React from "react";

interface WebGLErrorBoundaryProps {
  children: React.ReactNode;
  fallback?: React.ReactNode;
  onError?: (error: Error, errorInfo: React.ErrorInfo) => void;
}

interface WebGLErrorBoundaryState {
  hasError: boolean;
}

export class WebGLErrorBoundary extends React.Component<
  WebGLErrorBoundaryProps,
  WebGLErrorBoundaryState
> {
  public state: WebGLErrorBoundaryState = { hasError: false };

  static getDerivedStateFromError(): WebGLErrorBoundaryState {
    return { hasError: true };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    this.props.onError?.(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback ?? <WebGLFallback />;
    }
    return this.props.children;
  }
}

interface WebGLFallbackProps {
  className?: string;
  message?: string;
}

export function WebGLFallback({
  className,
  message = "Interactive WebGL content is unavailable on this device/browser.",
}: WebGLFallbackProps) {
  return (
    <div
      className={cn(
        "flex h-full w-full items-center justify-center bg-gradient-to-br from-zinc-950 via-slate-900 to-zinc-900 px-4 text-center text-sm text-white/75",
        className,
      )}
      role="status"
      aria-live="polite"
    >
      <p>{message}</p>
    </div>
  );
}
```
---

## Pixel Canvas <a name="pixel-canvas"></a>

An interactive pixel grid with smooth trailing effects that lights up on hover and decays over time.

- **Registry Key**: `pixel-canvas`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/pixel-canvas.json
```

#### How to Use
```tsx
"use client";

import { PixelCanvas } from "@workspace/ui/components/pixel-canvas";

function CursorHint() {
  return (
    <div className="pointer-events-none absolute inset-0 flex items-center justify-center">
      <span className="text-2xl font-medium tracking-tight text-foreground/50">
        Move your cursor
      </span>
    </div>
  );
}

export function PixelCanvasDemo() {
  return (
    <div className="relative h-full w-full overflow-hidden bg-white dark:bg-[#121212]">
      <PixelCanvas
        colors={["#e879f9", "#a78bfa", "#38bdf8", "#22d3ee"]}
        speed={0.02}
      />
      <CursorHint />
    </div>
  );
}

export function PixelCanvasTrailDemo() {
  return (
    <div className="relative h-full w-full overflow-hidden bg-white dark:bg-[#121212]">
      <PixelCanvas
        variant="trail"
        colors={["#f97316", "#fb923c", "#fbbf24", "#facc15"]}
        gap={8}
        speed={0.015}
      />
      <CursorHint />
    </div>
  );
}

export function PixelCanvasGlowDemo() {
  return (
    <div className="relative h-full w-full overflow-hidden bg-white dark:bg-[#121212]">
      <PixelCanvas
        variant="glow"
        colors={["#22c55e", "#10b981", "#14b8a6", "#06b6d4"]}
        gap={10}
        speed={0.01}
      />
      <CursorHint />
    </div>
  );
}

export function PixelCanvasSubtleDemo() {
  return (
    <div className="relative h-full w-full overflow-hidden bg-white dark:bg-[#121212]">
      <PixelCanvas
        colors={["#525252", "#a3a3a3", "#737373"]}
        gap={5}
        speed={0.03}
      />
      <CursorHint />
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/pixel-canvas.tsx`
```tsx
"use client";

import React, { useEffect, useRef, useCallback } from "react";
import { cn } from "@/lib/utils";

interface PixelCanvasProps extends React.HTMLAttributes<HTMLDivElement> {
    /** Size of each pixel cell in pixels */
    gap?: number;
    /** Speed of the trailing decay (higher = faster fade) */
    speed?: number;
    /** Array of colors for pixels - will interpolate through them as trail fades */
    colors?: string[];
    /** Disable mouse tracking */
    noFocus?: boolean;
    /** Variant style */
    variant?: "default" | "trail" | "glow";
}

interface Pixel {
    x: number;
    y: number;
    size: number;
    intensity: number;
    targetIntensity: number;
    colorPhase: number;
}

// Helper to interpolate between two hex colors
function lerpColor(color1: string, color2: string, t: number): string {
    const c1 = hexToRgb(color1);
    const c2 = hexToRgb(color2);
    if (!c1 || !c2) return color1;

    const r = Math.round(c1.r + (c2.r - c1.r) * t);
    const g = Math.round(c1.g + (c2.g - c1.g) * t);
    const b = Math.round(c1.b + (c2.b - c1.b) * t);

    return `rgb(${r}, ${g}, ${b})`;
}

function hexToRgb(hex: string): { r: number; g: number; b: number } | null {
    const result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
    return result
        ? {
            r: parseInt(result[1]!, 16),
            g: parseInt(result[2]!, 16),
            b: parseInt(result[3]!, 16),
        }
        : null;
}

export function PixelCanvas({
    className,
    gap = 6,
    speed = 0.02,
    colors = ["#e879f9", "#a78bfa", "#38bdf8", "#22d3ee"],
    noFocus = false,
    variant = "default",
    ...props
}: PixelCanvasProps) {
    const canvasRef = useRef<HTMLCanvasElement>(null);
    const containerRef = useRef<HTMLDivElement>(null);
    const pixelsRef = useRef<Pixel[][]>([]);
    const mouseRef = useRef({ x: -1000, y: -1000 });
    const animationRef = useRef<number>(0);
    const lastTimeRef = useRef<number>(0);

    const getColorFromIntensity = useCallback((intensity: number, phase: number) => {
        if (colors.length === 0) return "#ffffff";
        if (colors.length === 1) return colors[0]!;

        // Use phase + intensity to create a shifting color effect
        const t = (phase + intensity) % 1;
        const index = Math.floor(t * (colors.length - 1));
        const nextIndex = Math.min(index + 1, colors.length - 1);
        const localT = (t * (colors.length - 1)) % 1;

        const color1 = colors[index];
        const color2 = colors[nextIndex];

        if (!color1) return "#ffffff";
        if (!color2) return color1!;

        return lerpColor(color1, color2, localT);
    }, [colors]);

    useEffect(() => {
        const canvas = canvasRef.current;
        const container = containerRef.current;
        if (!canvas || !container) return;

        const ctx = canvas.getContext("2d", { alpha: true });
        if (!ctx) return;

        let cols = 0;
        let rows = 0;
        const pixelSize = Math.max(gap, 4);

        const initPixels = () => {
            const rect = container.getBoundingClientRect();
            const dpr = window.devicePixelRatio || 1;

            canvas.width = rect.width * dpr;
            canvas.height = rect.height * dpr;
            canvas.style.width = `${rect.width}px`;
            canvas.style.height = `${rect.height}px`;
            ctx.scale(dpr, dpr);

            cols = Math.ceil(rect.width / pixelSize);
            rows = Math.ceil(rect.height / pixelSize);

            const newPixels: Pixel[][] = [];
            for (let i = 0; i < cols; i++) {
                const row: Pixel[] = [];
                for (let j = 0; j < rows; j++) {
                    // Preserve existing intensity if pixel exists
                    const existing = pixelsRef.current[i]?.[j];
                    row.push({
                        x: i * pixelSize,
                        y: j * pixelSize,
                        size: pixelSize - 1,
                        intensity: existing?.intensity ?? 0,
                        targetIntensity: 0,
                        colorPhase: Math.random(), // Random starting phase for color variety
                    });
                }
                newPixels.push(row);
            }
            pixelsRef.current = newPixels;
        };

        const draw = (timestamp: number) => {
            const deltaTime = timestamp - lastTimeRef.current;
            lastTimeRef.current = timestamp;

            const rect = container.getBoundingClientRect();
            ctx.clearRect(0, 0, rect.width, rect.height);

            const { x: mouseX, y: mouseY } = mouseRef.current;
            const pixels = pixelsRef.current;

            // Influence radius based on variant
            const radius = variant === "glow" ? 120 : 80;
            const glowPasses = variant === "glow" ? 2 : 1;

            // Update pixel states
            for (let i = 0; i < cols; i++) {
                const col = pixels[i];
                if (!col) continue;

                for (let j = 0; j < rows; j++) {
                    const pixel = col[j];
                    if (!pixel) continue;

                    // Calculate distance from mouse
                    const centerX = pixel.x + pixel.size / 2;
                    const centerY = pixel.y + pixel.size / 2;
                    const dx = mouseX - centerX;
                    const dy = mouseY - centerY;
                    const distance = Math.sqrt(dx * dx + dy * dy);

                    // Set target intensity based on distance
                    if (distance < radius) {
                        const falloff = 1 - (distance / radius);
                        // Smooth falloff curve
                        pixel.targetIntensity = Math.pow(falloff, 1.5);
                    } else {
                        pixel.targetIntensity = 0;
                    }

                    // Smooth interpolation towards target
                    const lerpSpeed = pixel.targetIntensity > pixel.intensity
                        ? 0.3 // Quick light up
                        : speed; // Slow decay for trailing

                    pixel.intensity += (pixel.targetIntensity - pixel.intensity) * lerpSpeed;

                    // Shift color phase slowly for shimmer effect
                    pixel.colorPhase = (pixel.colorPhase + 0.001 * (deltaTime / 16)) % 1;

                    // Only draw if visible
                    if (pixel.intensity > 0.01) {
                        const color = getColorFromIntensity(pixel.intensity, pixel.colorPhase);

                        // Glow effect: draw larger, blurred version first
                        if (variant === "glow" && pixel.intensity > 0.2) {
                            for (let g = glowPasses; g > 0; g--) {
                                const glowSize = pixel.size + g * 4;
                                const glowOffset = (glowSize - pixel.size) / 2;
                                ctx.globalAlpha = pixel.intensity * 0.15 / g;
                                ctx.fillStyle = color;
                                ctx.fillRect(
                                    pixel.x - glowOffset,
                                    pixel.y - glowOffset,
                                    glowSize,
                                    glowSize
                                );
                            }
                        }

                        // Main pixel
                        ctx.globalAlpha = pixel.intensity * 0.9;
                        ctx.fillStyle = color;

                        if (variant === "trail") {
                            // Rounded pixels for trail variant
                            const cornerRadius = pixel.size * 0.3;
                            ctx.beginPath();
                            ctx.roundRect(pixel.x, pixel.y, pixel.size, pixel.size, cornerRadius);
                            ctx.fill();
                        } else {
                            ctx.fillRect(pixel.x, pixel.y, pixel.size, pixel.size);
                        }
                    }
                }
            }

            ctx.globalAlpha = 1;
            animationRef.current = requestAnimationFrame(draw);
        };

        const onMouseMove = (e: MouseEvent) => {
            const rect = canvas.getBoundingClientRect();
            mouseRef.current = {
                x: e.clientX - rect.left,
                y: e.clientY - rect.top,
            };
        };

        const onMouseLeave = () => {
            mouseRef.current = { x: -1000, y: -1000 };
        };

        const onTouchMove = (e: TouchEvent) => {
            if (e.touches.length > 0) {
                const touch = e.touches[0];
                if (touch) {
                    const rect = canvas.getBoundingClientRect();
                    mouseRef.current = {
                        x: touch.clientX - rect.left,
                        y: touch.clientY - rect.top,
                    };
                }
            }
        };

        const onTouchEnd = () => {
            mouseRef.current = { x: -1000, y: -1000 };
        };

        // Initialize
        initPixels();
        lastTimeRef.current = performance.now();
        animationRef.current = requestAnimationFrame(draw);

        // Event listeners
        window.addEventListener("resize", initPixels);
        if (!noFocus) {
            container.addEventListener("mousemove", onMouseMove);
            container.addEventListener("mouseleave", onMouseLeave);
            container.addEventListener("touchmove", onTouchMove, { passive: true });
            container.addEventListener("touchend", onTouchEnd);
        }

        return () => {
            cancelAnimationFrame(animationRef.current);
            window.removeEventListener("resize", initPixels);
            container.removeEventListener("mousemove", onMouseMove);
            container.removeEventListener("mouseleave", onMouseLeave);
            container.removeEventListener("touchmove", onTouchMove);
            container.removeEventListener("touchend", onTouchEnd);
        };
    }, [gap, speed, noFocus, variant, getColorFromIntensity]);

    return (
        <div
            ref={containerRef}
            className={cn("h-full w-full relative overflow-hidden", className)}
            {...props}
        >
            <canvas ref={canvasRef} className="block w-full h-full" />
        </div>
    );
}
```
---

## Pulsating Button <a name="pulsating-button"></a>

A button with a pulsating glow effect.

- **Registry Key**: `pulsating-button`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/pulsating-button.json
```

#### Component Source Code
##### File: `components/ui/pulsating-button.tsx`
```tsx
"use client"

import React from "react"

import { cn } from "@/lib/utils"

interface PulsatingButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  pulseColor?: string
  duration?: string
}

const PulsatingButton = React.forwardRef<
  HTMLButtonElement,
  PulsatingButtonProps
>(
  (
    {
      className,
      children,
      pulseColor = "#0096ff",
      duration = "1.5s",
      ...props
    },
    ref,
  ) => {
    return (
      <button
        ref={ref}
        className={cn(
          "relative flex cursor-pointer items-center justify-center rounded-full bg-blue-500 px-4 py-2 text-center text-white dark:bg-blue-500 dark:text-black",
          className,
        )}
        style={
          {
            "--pulse-color": pulseColor,
            "--duration": duration,
          } as React.CSSProperties
        }
        {...props}
      >
        <div className="relative z-10">{children}</div>
        <div className="absolute left-1/2 top-1/2 size-full -translate-x-1/2 -translate-y-1/2 animate-pulse rounded-lg bg-inherit opacity-40" />
      </button>
    )
  },
)

PulsatingButton.displayName = "PulsatingButton"

export { PulsatingButton }
```
---

## Scroll Based Velocity <a name="scroll-based-velocity"></a>

Text that scrolls across the screen and skews/speeds up based on the users scroll velocity.

- **Registry Key**: `scroll-based-velocity`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/scroll-based-velocity.json
```

#### Dependencies
- **NPM Packages**:
  - `framer-motion`

#### How to Use
```tsx
"use client";

import { useRef } from "react";
import { ScrollBasedVelocity } from "@workspace/ui/components/scroll-based-velocity";
import { ChevronDown } from "lucide-react";

export function ScrollBasedVelocityDemo() {
    const containerRef = useRef<HTMLDivElement>(null);

    return (
        <div
            ref={containerRef}
            className="relative w-full h-full overflow-y-auto [&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]"
        >
            {/* Tall scrollable content */}
            <div className="min-h-[300vh] flex flex-col items-center">
                {/* Top hint */}
                <div className="flex flex-col items-center gap-3 pt-16 pb-8 animate-pulse">
                    <span className="text-sm font-medium text-muted-foreground/70">
                        Scroll to see the effect
                    </span>
                    <ChevronDown className="w-4 h-4 text-muted-foreground/50" />
                </div>

                {/* Spacer */}
                <div className="h-[20vh]" />

                {/* Sticky velocity text */}
                <div className="sticky top-1/3 w-full py-8">
                    <ScrollBasedVelocity
                        text="Velocity Scroll"
                        default_velocity={5}
                        containerRef={containerRef}
                        className="font-display text-center text-4xl font-bold tracking-[-0.02em] text-foreground drop-shadow-sm md:text-7xl md:leading-[5rem]"
                    />
                </div>

                {/* Bottom spacer for scrolling room */}
                <div className="flex-1" />
            </div>
        </div>
    );
}
```
#### Component Source Code
##### File: `components/ui/scroll-based-velocity.tsx`
```tsx
"use client";

import React, { useRef } from "react";
import {
    motion,
    useScroll,
    useSpring,
    useTransform,
    useMotionValue,
    useVelocity,
    useAnimationFrame,
    wrap,
} from "framer-motion";
import { cn } from "@workspace/ui/lib/utils";

interface ScrollBasedVelocityProps {
    text: string;
    default_velocity?: number;
    className?: string;
    containerRef?: React.RefObject<HTMLElement | null>;
}

interface ParallaxProps {
    children: string;
    baseVelocity: number;
    className?: string;
    containerRef?: React.RefObject<HTMLElement | null>;
}

function ParallaxText({ children, baseVelocity = 100, className, containerRef }: ParallaxProps) {
    const baseX = useMotionValue(0);
    const { scrollY } = useScroll(containerRef ? { container: containerRef } : undefined);
    const scrollVelocity = useVelocity(scrollY);
    const smoothVelocity = useSpring(scrollVelocity, {
        damping: 50,
        stiffness: 400,
    });
    const velocityFactor = useTransform(smoothVelocity, [0, 1000], [0, 5], {
        clamp: false,
    });

    /**
     * This is a magic wrapping for the length of the text - you
     * have to replace for wrapping that works for you or dynamically
     * calculate
     */
    const x = useTransform(baseX, (v) => `${wrap(-12.5, 0, v)}%`);

    const directionFactor = useRef<number>(1);
    useAnimationFrame((t, delta) => {
        let moveBy = directionFactor.current * baseVelocity * (delta / 1000);

        /**
         * This is what changes the direction of the scroll once we
         * switch scrolling directions.
         */
        if (velocityFactor.get() < 0) {
            directionFactor.current = -1;
        } else if (velocityFactor.get() > 0) {
            directionFactor.current = 1;
        }

        moveBy += directionFactor.current * moveBy * velocityFactor.get();

        baseX.set(baseX.get() + moveBy);
    });

    return (
        <div className="overflow-hidden whitespace-nowrap flex flex-nowrap" style={{ width: '100%' }}>
            <motion.div
                className={cn("flex whitespace-nowrap", className)}
                style={{ x }}
            >
                {Array.from({ length: 8 }).map((_, i) => (
                    <span key={i} className="block mr-10 last:mr-10">{children}</span>
                ))}
            </motion.div>
        </div>
    );
}

export function ScrollBasedVelocity({
    text,
    default_velocity = 5,
    className,
    containerRef,
}: ScrollBasedVelocityProps) {
    return (
        <section className="relative w-full">
            <ParallaxText baseVelocity={default_velocity} className={className} containerRef={containerRef}>
                {text}
            </ParallaxText>
            <ParallaxText baseVelocity={-default_velocity} className={className} containerRef={containerRef}>
                {text}
            </ParallaxText>
        </section>
    );
}
```
---

## Scroll Choreography <a name="scroll-choreography"></a>

Component for scroll-choreography

- **Registry Key**: `scroll-choreography`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/scroll-choreography.json
```

#### How to Use
```tsx
"use client";

import * as React from "react";
import { cn } from "@/lib/utils";

interface ScrollChoreographyPreviewProps {
    src: string;
    title: string;
    className?: string;
}

export function ScrollChoreographyPreview({
    src,
    title,
    className,
}: ScrollChoreographyPreviewProps) {
    return (
        <div className={cn("relative w-full h-full min-h-[600px] overflow-hidden", className)}>
            <iframe src={src} className="w-full h-full border-0 bg-transparent" title={title} />
        </div>
    );
}
```
#### Component Source Code
##### File: `components/ui/scroll-choreography.tsx`
```tsx
"use client";

import { motion, useScroll, useSpring, useTransform } from "framer-motion";
import { useRef } from "react";
import { cn } from "@workspace/ui/lib/utils";

interface ScrollChoreographyProps {
  className?: string;
  images: {
    topLeft: string;
    topRight: string;
    bottomLeft: string;
    bottomRight: string;
  };
}

export function ScrollChoreography({
  className,
  images,
}: ScrollChoreographyProps) {
  const containerRef = useRef<HTMLDivElement>(null);

  const { scrollYProgress } = useScroll({
    target: containerRef,
    offset: ["start start", "end end"],
  });

  const smoothProgress = useSpring(scrollYProgress, {
    stiffness: 400, // Higher stiffness for a slightly faster snap
    damping: 50,   // Play with damping to add a little bounce/jerk
    mass: 1.2,     // Adds a bit more weight to the movement
    restDelta: 0.001,
  });

  // Default positions relative to center
  const xLeft = "-20vw";
  const xRight = "20vw";
  const yTop = "-14vh";
  const yBottom = "14vh";

  // Phase 1: 0 - 0.3 (Diagonal movement)
  // Phase 2: 0.35 - 0.65 (Stack alignment to center)
  // Phase 3: 0.7 - 0.9 (Top Right expands to full screen)

  // Top Left -> moves to Bottom Left, then to Center
  const tlX = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [xLeft, xLeft, xLeft, "0vw", "0vw"]);
  const tlY = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [yTop, yBottom, yBottom, "0vh", "0vh"]);

  // Bottom Right -> moves to Top Right, then to Center
  const brX = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [xRight, xRight, xRight, "0vw", "0vw"]);
  const brY = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [yBottom, yTop, yTop, "0vh", "0vh"]);

  // Bottom Left -> stays, then moves to Center
  const blX = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [xLeft, xLeft, xLeft, "0vw", "0vw"]);
  const blY = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [yBottom, yBottom, yBottom, "0vh", "0vh"]);

  // Top Right -> stays, then moves to Center, then expands
  const trX = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [xRight, xRight, xRight, "0vw", "0vw"]);
  const trY = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [yTop, yTop, yTop, "0vh", "0vh"]);

  // Top Right (Hero) scaling/expansion properties
  const heroWidth = useTransform(smoothProgress, [0.65, 0.7, 0.9, 1], ["36vw", "36vw", "100vw", "100vw"]);
  const heroHeight = useTransform(smoothProgress, [0.65, 0.7, 0.9, 1], ["24vh", "24vh", "100vh", "100vh"]);

  // Opacity fading for images underneath the hero as it expands
  const underImagesOpacity = useTransform(smoothProgress, [0.75, 0.85], [1, 0]);

  const baseImageClasses =
    "absolute left-1/2 top-1/2 w-[36vw] h-[24vh] overflow-hidden -translate-x-1/2 -translate-y-1/2 bg-muted shadow-2xl will-change-transform";

  return (
    <div ref={containerRef} className={cn("relative h-[300vh] w-full", className)}>
      <div className="sticky top-0 h-screen w-full overflow-hidden">
        <div className="absolute inset-0 flex items-center justify-center">

          {/* Top Left Image */}
          <motion.div
            style={{ x: tlX, y: tlY, opacity: underImagesOpacity }}
            className={cn(baseImageClasses, "z-10")}
          >
            <img src={images.topLeft} alt="Top Left" className="h-full w-full object-cover" />
          </motion.div>

          {/* Bottom Right Image */}
          <motion.div
            style={{ x: brX, y: brY, opacity: underImagesOpacity }}
            className={cn(baseImageClasses, "z-20")}
          >
            <img src={images.bottomRight} alt="Bottom Right" className="h-full w-full object-cover" />
          </motion.div>

          {/* Bottom Left Image */}
          <motion.div
            style={{ x: blX, y: blY, opacity: underImagesOpacity }}
            className={cn(baseImageClasses, "z-30")}
          >
            <img src={images.bottomLeft} alt="Bottom Left" className="h-full w-full object-cover" />
          </motion.div>

          {/* Top Right Image (Hero - expands at the end) */}
          <motion.div
            style={{
              x: trX,
              y: trY,
              width: heroWidth,
              height: heroHeight,
            }}
            className={cn(baseImageClasses, "z-40 origin-center bg-black/5")}
          >
            <img src={images.topRight} alt="Top Right (Hero)" className="h-full w-full object-cover" />
          </motion.div>
        </div>
      </div>
    </div>
  );
}
```
---

## Scroll Split Card <a name="scroll-split-card"></a>

Component for scroll-split-card

- **Registry Key**: `scroll-split-card`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/scroll-split-card.json
```

#### How to Use
```tsx
"use client";

import * as React from "react";
import { ScrollSplitCard } from "@workspace/ui/components/scroll-split-card";

export function ScrollSplitCardPreview() {
  const containerRef = React.useRef<HTMLDivElement>(null);

  return (
    <div ref={containerRef} className="w-full h-full overflow-y-auto relative [&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]">
      <ScrollSplitCard
        containerRef={containerRef}
        className="h-[250vh]" // Giving enough height to scroll

        imageSrc="https://framerusercontent.com/images/RCwmbu2vbsKf5a5D7OchNhir3Y.png?scale-down-to=1024&width=3072&height=2048"
        cards={[
          {
            title: "Going Zero to One",
            description: "If you've navigating a new business unit, or a new venture entirely, or breaking into a new market.",
            bgColor: "#e2e2e2",
            textColor: "#111111",
            icon: (
              <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
                <path d="M21 3l-6 6"></path>
                <path d="M21 3v6"></path>
                <path d="M21 3h-6"></path>
                <path d="M10.5 13.5L3 21"></path>
                <path d="M9 15l-3 3"></path>
              </svg>
            ),
          },
          {
            title: "Scaling from One to N",
            description: "If you've achieved Product/Market Fit, and are looking to scale your business to new heights.",
            bgColor: "#1a5bcf",
            textColor: "#ffffff",
            icon: (
              <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
                <circle cx="12" cy="5" r="3"></circle>
                <circle cx="5" cy="19" r="3"></circle>
                <circle cx="19" cy="19" r="3"></circle>
              </svg>
            ),
          },
          {
            title: "Need Quick Solutions",
            description: "If you know exactly what you want and need a team that can step in and quickly help you with it.",
            bgColor: "#1c1c1c",
            textColor: "#ffffff",
            icon: (
              <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
                <path d="M4.5 16.5c-1.5 1.26-2 5-2 5s3.74-.5 5-2l.5-.5"></path>
                <path d="M12 15l-1-1 7.5-7.5c1.41-1.41 3.09-1.41 4.5 0s1.41 3.09 0 4.5l-7.5 7.5-1-1z"></path>
              </svg>
            ),
          },
        ]}
      />
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/scroll-split-card.tsx`
```tsx
"use client";

import { cn } from "@/lib/utils";
import { motion, useScroll, useTransform, useMotionTemplate } from "framer-motion";
import { useRef, useState, useEffect } from "react";

interface ScrollSplitCardItem {
  title: string;
  description: string;
  bgColor: string;
  textColor: string;
  icon?: React.ReactNode;
}

interface ScrollSplitCardProps {
  className?: string;
  imageSrc: string;
  cards: ScrollSplitCardItem[];
  containerRef?: React.RefObject<HTMLElement | null>;
}

export function ScrollSplitCard({
  className,
  imageSrc,
  cards,
  containerRef: externalContainerRef,
}: ScrollSplitCardProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const [scrollContainer, setScrollContainer] = useState<React.RefObject<HTMLElement | null> | undefined>();

  useEffect(() => {
    if (externalContainerRef?.current) {
      setScrollContainer(externalContainerRef);
    }
  }, [externalContainerRef]);

  const { scrollYProgress } = useScroll({
    target: containerRef,
    ...(scrollContainer ? { container: scrollContainer } : {}),
    offset: ["start start", "end end"],
  });

  // Stage 1 to 2: Separation (0 to 0.4), then Stage 2 to 3: Overlap closer (0.4 to 0.8)
  const leftX = useTransform(scrollYProgress, [0, 0.4, 0.8], [0, -48, -24]);
  const rightX = useTransform(scrollYProgress, [0, 0.4, 0.8], [0, 48, 24]);
  const scale = useTransform(scrollYProgress, [0, 0.4], [1, 0.9]);

  // Stage 2 to 3: Flip (0.4 to 0.8)
  const rotateY = useTransform(scrollYProgress, [0.4, 0.8], [0, 180]);
  // Due to 180deg Y flip, positive Z becomes visual counter-clockwise, negative Z becomes visual clockwise
  const rotateZLeft = useTransform(scrollYProgress, [0.4, 0.8], [0, 6]);
  const rotateZRight = useTransform(scrollYProgress, [0.4, 0.8], [0, -6]);

  // Dynamic borders/radii so it looks like ONE flat image initially
  const borderRadiusLeft = useTransform(scrollYProgress, [0, 0.2], ["16px 0px 0px 16px", "16px 16px 16px 16px"]);
  const borderRadiusMiddle = useTransform(scrollYProgress, [0, 0.2], ["0px 0px 0px 0px", "16px 16px 16px 16px"]);
  const borderRadiusRight = useTransform(scrollYProgress, [0, 0.2], ["0px 16px 16px 0px", "16px 16px 16px 16px"]);
  const borderOpacity = useTransform(scrollYProgress, [0, 0.2], [0, 0.2]);
  const shadowOpacity = useTransform(scrollYProgress, [0, 0.2], [0, 0.4]);
  const boxShadow = useMotionTemplate`inset 0 1px 1px rgba(255, 255, 255, ${borderOpacity}), inset 0 -24px 48px rgba(0, 0, 0, ${shadowOpacity}), 0 25px 50px -12px rgba(0, 0, 0, ${shadowOpacity})`;

  // Cards move up in the last viewport
  const cardsY = useTransform(scrollYProgress, [0.8, 1], [0, -200]);

  // Text appearance at the end in the sticky viewport
  const textOpacity = useTransform(scrollYProgress, [0.8, 1], [0, 1]);
  const textY = useTransform(scrollYProgress, [0.8, 1], [40, 0]);

  // Indicator text appearance at the start
  const startTextOpacity = useTransform(scrollYProgress, [0, 0.1], [1, 0]);
  const startTextY = useTransform(scrollYProgress, [0, 0.1], [0, 20]);

  return (
    <div
      ref={containerRef}
      className={cn("relative h-[500vh] w-full", className)}
    >
      <div className="sticky top-0 flex h-screen w-full items-center justify-center overflow-hidden [perspective:1200px]">
        {/* Starting Text indicator */}
        <motion.div
          className="absolute top-[20%] left-0 right-0 text-center"
          style={{
            opacity: startTextOpacity,
            y: startTextY,
          }}
        >
          <p className="text-sm font-medium tracking-widest text-foreground/50 uppercase">
            Scroll down
          </p>
        </motion.div>

        <motion.div
          style={{ scale, y: cardsY, transformStyle: "preserve-3d" }}
          className="flex h-[400px] w-full max-w-4xl px-4 relative"
        >
          {cards.slice(0, 3).map((card, i) => (
            <motion.div
              key={i}
              className="relative h-full flex-1"
              style={{
                x: i === 0 ? leftX : i === 2 ? rightX : 0,
                rotateY,
                rotateZ: i === 0 ? rotateZLeft : i === 2 ? rotateZRight : 0,
                zIndex: i, // Ensures Left is under Middle, and Right is above Middle
                transformStyle: "preserve-3d",
              }}
            >
              {/* Front Side: Original Image Split */}
              <motion.div
                className="absolute inset-0 overflow-hidden [backface-visibility:hidden]"
                style={{
                  zIndex: 2, // Ensure front stays above initially
                  borderRadius: i === 0 ? borderRadiusLeft : i === 2 ? borderRadiusRight : borderRadiusMiddle,
                  boxShadow,
                }}
              >
                <div
                  className="absolute inset-0 h-full w-[300%]"
                  style={{
                    left: `${-100 * i}%`,
                    backgroundImage: `url(${imageSrc})`,
                    backgroundSize: "100% 100%",
                    backgroundPosition: "center",
                  }}
                />
              </motion.div>

              {/* Back Side: New Content Card */}
              <motion.div
                className={cn(
                  "absolute inset-0 overflow-hidden flex flex-col justify-end p-8 [backface-visibility:hidden] will-change-transform",
                  "border border-white/5 bg-gradient-to-br from-white/10 to-transparent",
                  "shadow-[inset_0_1px_1px_rgba(255,255,255,0.1),inset_0_-24px_48px_rgba(0,0,0,0.2)]"
                )}
                style={{
                  backgroundColor: card.bgColor,
                  color: card.textColor,
                  transform: "rotateY(180deg)",
                  zIndex: 1, // Ensure back is behind before flip
                  borderRadius: i === 0 ? borderRadiusLeft : i === 2 ? borderRadiusRight : borderRadiusMiddle,
                  boxShadow,
                }}
              >
                {/* Grainy Noise Overlay */}
                <div
                  className="pointer-events-none absolute inset-0 opacity-20 mix-blend-overlay"
                  style={{
                    backgroundImage: `url("https://framerusercontent.com/images/6mcf62RlDfRfU61Yg5vb2pefpi4.png?width=256&height=256")`,
                    backgroundRepeat: "repeat",
                  }}
                />

                <div className="relative z-10 mb-auto">{card.icon}</div>
                <h3 className="relative z-10 mb-4 text-2xl font-medium leading-tight">
                  {card.title}
                </h3>
                <p className="relative z-10 text-sm opacity-80">{card.description}</p>
              </motion.div>
            </motion.div>
          ))}
        </motion.div>

        {/* Ending Text fixed in the sticky viewport */}
        <motion.div
          className="absolute bottom-[20%] left-0 right-0 text-center"
          style={{
            opacity: textOpacity,
            y: textY,
          }}
        >
          <p className="text-3xl font-medium tracking-tight text-foreground/80 font-serif italic">
            So cool, right?
          </p>
        </motion.div>
      </div>
    </div>
  );
}
```
---

## Scrub Input <a name="scrub-input"></a>

An inline slider input styled as a pill, perfect for adjusting variables smoothly.

- **Registry Key**: `scrub-input`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/scrub-input.json
```

#### How to Use
```tsx
"use client";

import { useState } from "react";
import { ScrubInput } from "@workspace/ui/components/scrub-input";

export function ScrubInputDemo() {
    const [opacity, setOpacity] = useState(44);

    return (
        <div className="flex h-full w-full items-center justify-center p-8">
            <ScrubInput
                label="Opacity"
                value={opacity}
                onChange={setOpacity}
                min={0}
                max={100}
            />
        </div>
    );
}

export function ScrubInputMultipleDemo() {
    const [radius, setRadius] = useState(12);
    const [blur, setBlur] = useState(24);

    return (
        <div className="flex h-full w-full flex-col gap-4 items-center justify-center p-8">
            <ScrubInput
                label="Border Radius"
                value={radius}
                onChange={setRadius}
                min={0}
                max={100}
            />
            <ScrubInput
                label="Blur"
                value={blur}
                onChange={setBlur}
                min={0}
                max={50}
            />
        </div>
    );
}
```
#### Component Source Code
##### File: `components/ui/scrub-input.tsx`
```tsx
"use client";

import React, { useRef, useState } from "react";
import { cn } from "@workspace/ui/lib/utils";

export interface ScrubInputProps {
    value?: number;
    defaultValue?: number;
    onChange?: (value: number) => void;
    label: string;
    min?: number;
    max?: number;
    step?: number;
    className?: string;
}

export function ScrubInput({
    value: controlledValue,
    defaultValue = 0,
    onChange,
    label,
    min = 0,
    max = 100,
    step = 1,
    className,
}: ScrubInputProps) {
    const [uncontrolledValue, setUncontrolledValue] = useState(defaultValue);
    const isControlled = controlledValue !== undefined;
    const value = isControlled ? controlledValue : uncontrolledValue;

    const [isDragging, setIsDragging] = useState(false);
    const containerRef = useRef<HTMLDivElement>(null);

    const handleValueChange = (newValue: number) => {
        if (!isControlled) setUncontrolledValue(newValue);
        onChange?.(newValue);
    };

    const updateValueFromPointer = (clientX: number) => {
        if (!containerRef.current) return;
        const rect = containerRef.current.getBoundingClientRect();
        const x = clientX - rect.left;
        let percentage = x / rect.width;
        percentage = Math.max(0, Math.min(1, percentage));

        let newValue = min + percentage * (max - min);
        newValue = Math.round(newValue / step) * step;

        // Ensure value stays within bounds due to rounding
        newValue = Math.max(min, Math.min(max, newValue));

        if (newValue !== value) {
            handleValueChange(newValue);
        }
    };

    const onPointerDown = (e: React.PointerEvent) => {
        containerRef.current?.setPointerCapture(e.pointerId);
        setIsDragging(true);
        updateValueFromPointer(e.clientX);
    };

    const onPointerMove = (e: React.PointerEvent) => {
        if (!isDragging) return;
        updateValueFromPointer(e.clientX);
    };

    const onPointerUp = (e: React.PointerEvent) => {
        containerRef.current?.releasePointerCapture(e.pointerId);
        setIsDragging(false);
    };

    const percentage = max > min ? ((value - min) / (max - min)) * 100 : 0;

    // Create a minimum width so the fill doesn't look too weird when value is 0
    const fillWidth = Math.max(20, percentage);

    return (
        <div
            ref={containerRef}
            className={cn(
                "relative h-12 w-[280px] rounded-2xl bg-zinc-100 dark:bg-zinc-900 border border-zinc-200 dark:border-zinc-800 select-none touch-none cursor-ew-resize overflow-hidden",
                "transition-transform active:scale-[0.98]",
                className
            )}
            onPointerDown={onPointerDown}
            onPointerMove={onPointerMove}
            onPointerUp={onPointerUp}
            onPointerCancel={onPointerUp}
        >
            {/* The fill / thumb */}
            <div
                className={cn(
                    "absolute left-0 top-0 h-full rounded-2xl flex items-center justify-end pr-3",
                    "bg-white dark:bg-zinc-800 shadow-[0_2px_8px_rgba(0,0,0,0.1)] dark:shadow-none border-r border-zinc-200 dark:border-zinc-700",
                    isDragging ? "transition-none" : "transition-[width] duration-300 ease-out"
                )}
                style={{ width: `${fillWidth}%` }}
            >
                {/* Handle mark */}
                <div className="h-5 w-[3px] rounded-full bg-zinc-300 dark:bg-zinc-600" />
            </div>

            {/* Label and Value (Always on top to be visible whether the thumb covers them or not) */}
            <div className="absolute inset-0 flex items-center justify-between px-5 pointer-events-none">
                <span className="text-[15px] tracking-tight font-medium text-black/60 dark:text-white/60">
                    {label}
                </span>
                <span className="text-[16px] tracking-tight font-medium text-black/50 dark:text-white/50">
                    {value}
                </span>
            </div>
        </div>
    );
}
```
---

## Shimmer Button <a name="shimmer-button"></a>

A button with a shimmering light effect.

- **Registry Key**: `shimmer-button`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/shimmer-button.json
```

#### Component Source Code
##### File: `components/ui/shimmer-button.tsx`
```tsx
"use client"

import React from "react"

import { cn } from "@/lib/utils"

export interface ShimmerButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  shimmerColor?: string
  shimmerSize?: string
  borderRadius?: string
  shimmerDuration?: string
  background?: string
  className?: string
  children?: React.ReactNode
}

const ShimmerButton = React.forwardRef<HTMLButtonElement, ShimmerButtonProps>(
  (
    {
      shimmerColor = "#ffffff",
      shimmerSize = "0.05em",
      shimmerDuration = "3s",
      borderRadius = "100px",
      background = "rgba(0, 0, 0, 1)",
      className,
      children,
      ...props
    },
    ref,
  ) => {
    return (
      <button
        style={
          {
            "--spread": "90deg",
            "--shimmer-color": shimmerColor,
            "--radius": borderRadius,
            "--speed": shimmerDuration,
            "--cut": shimmerSize,
            "--bg": background,
          } as React.CSSProperties
        }
        className={cn(
          "group relative z-0 flex cursor-pointer items-center justify-center overflow-hidden whitespace-nowrap border border-white/10 px-6 py-3 text-white [background:var(--bg)] [border-radius:var(--radius)] dark:text-black",
          "transform-gpu transition-transform duration-300 ease-in-out active:translate-y-px",
          className,
        )}
        ref={ref}
        {...props}
      >
        {/* spark container */}
        <div
          className={cn(
            "-z-30 blur-[2px]",
            "absolute inset-0 overflow-visible [container-type:size]",
          )}
        >
          {/* spark */}
          <div className="absolute inset-0 h-[100cqh] animate-shimmer-slide [aspect-ratio:1] [border-radius:0] [mask:none]">
            {/* spark before */}
            <div className="animate-spin-around absolute -inset-full w-auto rotate-0 [background:conic-gradient(from_calc(270deg-(var(--spread)*0.5)),transparent_0,var(--shimmer-color)_var(--spread),transparent_var(--spread))] [translate:0_0]" />
          </div>
        </div>
        {children}

        {/* Highlight */}
        <div
          className={cn(
            "insert-0 absolute size-full",
            "rounded-2xl px-4 py-1.5 text-sm font-medium shadow-[inset_0_-8px_10px_#ffffff1f]",
            "transform-gpu transition-all duration-300 ease-in-out",
            "group-hover:shadow-[inset_0_-6px_10px_#ffffff3f]",
            "group-active:shadow-[inset_0_-10px_10px_#ffffff3f]",
          )}
        />

        {/* backdrop */}
        <div
          className={cn(
            "absolute -z-20 [background:var(--bg)] [border-radius:var(--radius)] [inset:var(--cut)]",
          )}
        />
      </button>
    )
  },
)

ShimmerButton.displayName = "ShimmerButton"

export { ShimmerButton }
```
---

## Showcase Card <a name="showcase-card"></a>

A premium showcase card component with 3D tilt effect, parallax image, micro-interactions, and multiple variants. Perfect for portfolios, agency sites, and product showcases.

- **Registry Key**: `showcase-card`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/showcase-card.json
```

#### Dependencies
- **NPM Packages**:
  - `framer-motion`

#### Component Source Code
##### File: `components/ui/showcase-card.tsx`
```tsx
"use client"

import * as React from "react"
import { motion, useMotionValue, useSpring, useTransform } from "framer-motion"
import { cn } from "@/lib/utils"

interface ShowcaseCardProps {
    /** Top tagline text */
    tagline?: string
    /** Main heading text */
    heading: string
    /** Description text below heading */
    description?: string
    /** Image URL for the hero section */
    imageUrl: string
    /** Alt text for the image */
    imageAlt?: string
    /** CTA button text */
    ctaText?: string
    /** CTA button click handler */
    onCtaClick?: () => void
    /** Brand name or logo text */
    brandName?: string
    /** Array of service tags */
    services?: string[]
    /** Custom class name */
    className?: string
    /** Enable 3D tilt effect on hover */
    enableTilt?: boolean
    /** Maximum tilt angle in degrees */
    maxTilt?: number
    /** Enable parallax effect on image */
    enableParallax?: boolean
}

function ShowcaseCard({
    tagline,
    heading,
    description,
    imageUrl,
    imageAlt = "Showcase image",
    ctaText,
    onCtaClick,
    brandName,
    services = [],
    className,
    enableTilt = true,
    maxTilt = 8,
    enableParallax = true,
}: ShowcaseCardProps) {
    const cardRef = React.useRef<HTMLDivElement>(null)
    const [isHovered, setIsHovered] = React.useState(false)

    // Mouse position for tilt effect
    const mouseX = useMotionValue(0)
    const mouseY = useMotionValue(0)

    // Smooth spring animation for tilt
    const springConfig = { damping: 25, stiffness: 150 }
    const rotateX = useSpring(useTransform(mouseY, [-0.5, 0.5], [maxTilt, -maxTilt]), springConfig)
    const rotateY = useSpring(useTransform(mouseX, [-0.5, 0.5], [-maxTilt, maxTilt]), springConfig)

    // Parallax transform for image
    const parallaxX = useSpring(useTransform(mouseX, [-0.5, 0.5], [-15, 15]), springConfig)
    const parallaxY = useSpring(useTransform(mouseY, [-0.5, 0.5], [-15, 15]), springConfig)

    // Glow effect position
    const glowX = useSpring(useTransform(mouseX, [-0.5, 0.5], [0, 100]), springConfig)
    const glowY = useSpring(useTransform(mouseY, [-0.5, 0.5], [0, 100]), springConfig)

    const handleMouseMove = React.useCallback(
        (e: React.MouseEvent<HTMLDivElement>) => {
            if (!cardRef.current || !enableTilt) return

            const rect = cardRef.current.getBoundingClientRect()
            const x = (e.clientX - rect.left) / rect.width - 0.5
            const y = (e.clientY - rect.top) / rect.height - 0.5

            mouseX.set(x)
            mouseY.set(y)
        },
        [mouseX, mouseY, enableTilt]
    )

    const handleMouseEnter = () => {
        setIsHovered(true)
    }

    const handleMouseLeave = () => {
        setIsHovered(false)
        mouseX.set(0)
        mouseY.set(0)
    }

    return (
        <motion.div
            ref={cardRef}
            className={cn(
                "relative w-full max-w-[400px] rounded-3xl overflow-hidden",
                "bg-neutral-950 dark:bg-neutral-950",
                "shadow-2xl shadow-black/20 dark:shadow-black/40",
                "cursor-pointer select-none",
                className
            )}
            style={{
                transformStyle: "preserve-3d",
                perspective: 1000,
                rotateX: enableTilt ? rotateX : 0,
                rotateY: enableTilt ? rotateY : 0,
            }}
            onMouseMove={handleMouseMove}
            onMouseEnter={handleMouseEnter}
            onMouseLeave={handleMouseLeave}
            initial={{ opacity: 0, y: 40 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.6, ease: [0.25, 0.46, 0.45, 0.94] }}
            whileHover={{
                scale: 1.02,
                boxShadow: "0 40px 80px -20px rgba(0,0,0,0.5)",
            }}
        >
            {/* Subtle glow overlay on hover */}
            <motion.div
                className="absolute inset-0 z-10 pointer-events-none opacity-0"
                style={{
                    background: `radial-gradient(circle at ${glowX}% ${glowY}%, rgba(255,255,255,0.08) 0%, transparent 50%)`,
                }}
                animate={{ opacity: isHovered ? 1 : 0 }}
                transition={{ duration: 0.3 }}
            />

            {/* Image Section */}
            <div className="relative aspect-[4/3] overflow-hidden">
                {/* Tagline */}
                {tagline && (
                    <motion.div
                        className="absolute top-4 left-4 sm:top-6 sm:left-6 z-20"
                        initial={{ opacity: 0, y: -10 }}
                        animate={{ opacity: 1, y: 0 }}
                        transition={{ delay: 0.2, duration: 0.5 }}
                    >
                        <span className="text-white/90 text-sm sm:text-base font-medium tracking-tight">
                            {tagline}
                        </span>
                    </motion.div>
                )}

                {/* Hero Image with Parallax */}
                <motion.div
                    className="absolute inset-0"
                    style={{
                        x: enableParallax ? parallaxX : 0,
                        y: enableParallax ? parallaxY : 0,
                        scale: 1.1,
                    }}
                >
                    <motion.img
                        src={imageUrl}
                        alt={imageAlt}
                        className="w-full h-full object-cover"
                        initial={{ scale: 1.2 }}
                        animate={{ scale: isHovered ? 1.15 : 1.1 }}
                        transition={{ duration: 0.6, ease: "easeOut" }}
                    />
                </motion.div>

                {/* Gradient overlay for text readability */}
                <div className="absolute inset-0 bg-gradient-to-t from-neutral-950 via-transparent to-transparent opacity-60" />
            </div>

            {/* Content Section */}
            <div className="relative z-10 px-4 sm:px-6 pb-4 sm:pb-6 -mt-8">
                {/* Heading */}
                <motion.h2
                    className="text-2xl sm:text-3xl lg:text-4xl font-medium text-white tracking-tight leading-tight mb-2 sm:mb-3"
                    initial={{ opacity: 0, y: 20 }}
                    animate={{ opacity: 1, y: 0 }}
                    transition={{ delay: 0.3, duration: 0.5 }}
                >
                    {heading}
                </motion.h2>

                {/* Description */}
                {description && (
                    <motion.p
                        className="text-sm sm:text-base text-neutral-400 mb-4 sm:mb-6 leading-relaxed"
                        initial={{ opacity: 0, y: 20 }}
                        animate={{ opacity: 1, y: 0 }}
                        transition={{ delay: 0.4, duration: 0.5 }}
                    >
                        {description}
                    </motion.p>
                )}

                {/* CTA Button */}
                {ctaText && (
                    <motion.button
                        onClick={onCtaClick}
                        className={cn(
                            "relative px-4 sm:px-5 py-2 sm:py-2.5 rounded-full",
                            "text-sm font-medium",
                            "bg-neutral-800/80 text-neutral-200",
                            "border border-neutral-700/50",
                            "overflow-hidden",
                            "transition-colors duration-300",
                            "hover:border-neutral-600/80"
                        )}
                        initial={{ opacity: 0, y: 20 }}
                        animate={{ opacity: 1, y: 0 }}
                        transition={{ delay: 0.5, duration: 0.5 }}
                        whileHover={{ scale: 1.05 }}
                        whileTap={{ scale: 0.98 }}
                    >
                        {/* Button hover shine effect */}
                        <motion.span
                            className="absolute inset-0 bg-gradient-to-r from-transparent via-white/10 to-transparent -translate-x-full"
                            animate={isHovered ? { translateX: "200%" } : { translateX: "-100%" }}
                            transition={{ duration: 0.6, ease: "easeInOut" }}
                        />
                        <span className="relative z-10">{ctaText}</span>
                    </motion.button>
                )}
            </div>

            {/* Footer Section */}
            {(brandName || services.length > 0) && (
                <motion.div
                    className="px-4 sm:px-6 py-4 sm:py-5 border-t border-neutral-800/50"
                    initial={{ opacity: 0 }}
                    animate={{ opacity: 1 }}
                    transition={{ delay: 0.6, duration: 0.5 }}
                >
                    <div className="flex items-center justify-between flex-wrap gap-2">
                        {/* Brand Name */}
                        {brandName && (
                            <motion.span
                                className="text-xs sm:text-sm text-neutral-400 font-medium"
                                whileHover={{ color: "#ffffff" }}
                                transition={{ duration: 0.2 }}
                            >
                                {brandName}
                            </motion.span>
                        )}

                        {/* Services */}
                        {services.length > 0 && (
                            <div className="flex items-center gap-2 sm:gap-3 flex-wrap">
                                {services.map((service, index) => (
                                    <React.Fragment key={service}>
                                        <motion.span
                                            className="text-xs sm:text-sm text-neutral-500"
                                            whileHover={{ color: "#ffffff", scale: 1.05 }}
                                            transition={{ duration: 0.2 }}
                                        >
                                            {service}
                                        </motion.span>
                                        {index < services.length - 1 && (
                                            <motion.span
                                                className="text-neutral-600"
                                                initial={{ rotate: 0 }}
                                                whileHover={{ rotate: 90 }}
                                                transition={{ duration: 0.3 }}
                                            >
                                                ✦
                                            </motion.span>
                                        )}
                                    </React.Fragment>
                                ))}
                            </div>
                        )}
                    </div>
                </motion.div>
            )}

            {/* Border glow effect */}
            <motion.div
                className="absolute inset-0 rounded-3xl pointer-events-none"
                style={{
                    boxShadow: "inset 0 0 0 1px rgba(255,255,255,0.05)",
                }}
                animate={{
                    boxShadow: isHovered
                        ? "inset 0 0 0 1px rgba(255,255,255,0.1)"
                        : "inset 0 0 0 1px rgba(255,255,255,0.05)",
                }}
                transition={{ duration: 0.3 }}
            />
        </motion.div>
    )
}

// Compact variant for grids
interface ShowcaseCardCompactProps {
    heading: string
    description?: string
    imageUrl: string
    imageAlt?: string
    className?: string
    onClick?: () => void
}

function ShowcaseCardCompact({
    heading,
    description,
    imageUrl,
    imageAlt = "Showcase image",
    className,
    onClick,
}: ShowcaseCardCompactProps) {
    const [isHovered, setIsHovered] = React.useState(false)

    return (
        <motion.div
            className={cn(
                "relative w-full rounded-2xl overflow-hidden cursor-pointer",
                "bg-neutral-900 dark:bg-neutral-900",
                "shadow-lg shadow-black/10",
                className
            )}
            onClick={onClick}
            onMouseEnter={() => setIsHovered(true)}
            onMouseLeave={() => setIsHovered(false)}
            whileHover={{ scale: 1.02, y: -4 }}
            whileTap={{ scale: 0.98 }}
            transition={{ duration: 0.3 }}
        >
            {/* Image */}
            <div className="relative aspect-[16/10] overflow-hidden">
                <motion.img
                    src={imageUrl}
                    alt={imageAlt}
                    className="w-full h-full object-cover"
                    animate={{ scale: isHovered ? 1.08 : 1 }}
                    transition={{ duration: 0.4, ease: "easeOut" }}
                />
                <div className="absolute inset-0 bg-gradient-to-t from-neutral-900 via-transparent to-transparent opacity-80" />
            </div>

            {/* Content */}
            <div className="p-4 -mt-6 relative z-10">
                <h3 className="text-lg font-medium text-white mb-1 line-clamp-2">{heading}</h3>
                {description && (
                    <p className="text-sm text-neutral-400 line-clamp-2">{description}</p>
                )}
            </div>

            {/* Hover indicator */}
            <motion.div
                className="absolute bottom-4 right-4"
                initial={{ opacity: 0, x: -10 }}
                animate={{ opacity: isHovered ? 1 : 0, x: isHovered ? 0 : -10 }}
                transition={{ duration: 0.2 }}
            >
                <svg
                    width="20"
                    height="20"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    strokeWidth="2"
                    strokeLinecap="round"
                    strokeLinejoin="round"
                    className="text-neutral-400"
                >
                    <path d="M7 17L17 7" />
                    <path d="M7 7h10v10" />
                </svg>
            </motion.div>
        </motion.div>
    )
}

// Horizontal variant for featured sections
interface ShowcaseCardHorizontalProps extends Omit<ShowcaseCardProps, "enableTilt" | "maxTilt"> {
    imagePosition?: "left" | "right"
}

function ShowcaseCardHorizontal({
    tagline,
    heading,
    description,
    imageUrl,
    imageAlt = "Showcase image",
    ctaText,
    onCtaClick,
    brandName,
    services = [],
    className,
    imagePosition = "left",
    enableParallax = true,
}: ShowcaseCardHorizontalProps) {
    const [isHovered, setIsHovered] = React.useState(false)

    return (
        <motion.div
            className={cn(
                "relative w-full rounded-3xl overflow-hidden",
                "bg-neutral-950 dark:bg-neutral-950",
                "shadow-2xl shadow-black/20",
                "flex flex-col md:flex-row",
                imagePosition === "right" && "md:flex-row-reverse",
                className
            )}
            onMouseEnter={() => setIsHovered(true)}
            onMouseLeave={() => setIsHovered(false)}
            initial={{ opacity: 0, y: 40 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.6 }}
            whileHover={{ scale: 1.01 }}
        >
            {/* Image Section */}
            <div className="relative w-full md:w-1/2 aspect-[4/3] md:aspect-auto md:min-h-[400px] overflow-hidden">
                {tagline && (
                    <div className="absolute top-6 left-6 z-20">
                        <span className="text-white/90 text-sm font-medium">{tagline}</span>
                    </div>
                )}

                <motion.img
                    src={imageUrl}
                    alt={imageAlt}
                    className="w-full h-full object-cover"
                    animate={{ scale: isHovered ? 1.05 : 1 }}
                    transition={{ duration: 0.6, ease: "easeOut" }}
                />
                <div className="absolute inset-0 bg-gradient-to-r from-neutral-950/60 via-transparent to-transparent md:hidden" />
            </div>

            {/* Content Section */}
            <div className="relative z-10 w-full md:w-1/2 p-6 md:p-10 flex flex-col justify-center">
                <motion.h2
                    className="text-3xl md:text-4xl lg:text-5xl font-medium text-white tracking-tight leading-tight mb-4"
                    initial={{ opacity: 0, y: 20 }}
                    animate={{ opacity: 1, y: 0 }}
                    transition={{ delay: 0.2, duration: 0.5 }}
                >
                    {heading}
                </motion.h2>

                {description && (
                    <motion.p
                        className="text-base md:text-lg text-neutral-400 mb-8 leading-relaxed max-w-md"
                        initial={{ opacity: 0, y: 20 }}
                        animate={{ opacity: 1, y: 0 }}
                        transition={{ delay: 0.3, duration: 0.5 }}
                    >
                        {description}
                    </motion.p>
                )}

                {ctaText && (
                    <motion.button
                        onClick={onCtaClick}
                        className={cn(
                            "w-fit px-6 py-3 rounded-full",
                            "text-sm font-medium",
                            "bg-white text-neutral-900",
                            "transition-all duration-300",
                            "hover:bg-neutral-200"
                        )}
                        initial={{ opacity: 0, y: 20 }}
                        animate={{ opacity: 1, y: 0 }}
                        transition={{ delay: 0.4, duration: 0.5 }}
                        whileHover={{ scale: 1.05 }}
                        whileTap={{ scale: 0.98 }}
                    >
                        {ctaText}
                    </motion.button>
                )}

                {(brandName || services.length > 0) && (
                    <div className="mt-auto pt-8 flex items-center justify-between">
                        {brandName && (
                            <span className="text-sm text-neutral-500">{brandName}</span>
                        )}
                        {services.length > 0 && (
                            <div className="flex items-center gap-3">
                                {services.map((service, index) => (
                                    <React.Fragment key={service}>
                                        <span className="text-sm text-neutral-500">{service}</span>
                                        {index < services.length - 1 && (
                                            <span className="text-neutral-600">✦</span>
                                        )}
                                    </React.Fragment>
                                ))}
                            </div>
                        )}
                    </div>
                )}
            </div>
        </motion.div>
    )
}

// Grid container for showcase cards
interface ShowcaseGridProps {
    children: React.ReactNode
    columns?: 1 | 2 | 3 | 4
    gap?: "sm" | "md" | "lg"
    className?: string
}

function ShowcaseGrid({
    children,
    columns = 3,
    gap = "md",
    className,
}: ShowcaseGridProps) {
    const gapClasses = {
        sm: "gap-4",
        md: "gap-6",
        lg: "gap-8",
    }

    const columnClasses = {
        1: "grid-cols-1",
        2: "grid-cols-1 sm:grid-cols-2",
        3: "grid-cols-1 sm:grid-cols-2 lg:grid-cols-3",
        4: "grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4",
    }

    return (
        <div className={cn("grid", columnClasses[columns], gapClasses[gap], className)}>
            {children}
        </div>
    )
}

export {
    ShowcaseCard,
    ShowcaseCardCompact,
    ShowcaseCardHorizontal,
    ShowcaseGrid,
    type ShowcaseCardProps,
    type ShowcaseCardCompactProps,
    type ShowcaseCardHorizontalProps,
    type ShowcaseGridProps,
}
```
---

## Signature <a name="signature"></a>

Component for signature

- **Registry Key**: `signature`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/signature.json
```

#### Dependencies
- **NPM Packages**:
  - `framer-motion`
  - `opentype.js`

#### How to Use
```tsx
"use client";

import { useEffect, useMemo, useState } from "react";
import { Signature } from "@workspace/ui/components/signature";
import { cn } from "@/lib/utils";
import {
  SIGNATURE_DEFAULT_CONFIG,
  type SignatureConfig,
  usePlaygroundStore,
} from "@/hooks/use-playground-store";

const PRESETS: Array<{ name: string; config: SignatureConfig }> = [
  {
    name: "Default",
    config: SIGNATURE_DEFAULT_CONFIG,
  },
  {
    name: "John Doe",
    config: {
      ...SIGNATURE_DEFAULT_CONFIG,
      text: "John Doe",
      color: "#3b82f6",
      duration: 2,
    },
  },
  {
    name: "Autograph",
    config: {
      ...SIGNATURE_DEFAULT_CONFIG,
      text: "Jane Smith",
      color: "#ec4899",
      duration: 1.2,
      fontSize: 56,
    },
  },
];

const SectionTitle = ({ children }: { children: React.ReactNode }) => (
  <div className="mb-3 text-[10px] font-mono uppercase tracking-widest text-muted-foreground">
    {children}
  </div>
);

const Input = ({
  className,
  ...props
}: React.InputHTMLAttributes<HTMLInputElement>) => (
  <input
    className={cn(
      "h-10 w-full rounded-md border border-border/70 bg-transparent px-3 text-sm text-foreground placeholder:text-muted-foreground/70 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-zinc-400/25",
      className,
    )}
    {...props}
  />
);

const Slider = ({
  value,
  min,
  max,
  step,
  onChange,
  label,
  unit = "",
}: {
  value: number;
  min: number;
  max: number;
  step: number;
  onChange: (val: number) => void;
  label: string;
  unit?: string;
}) => (
  <div className="space-y-2">
    <div className="flex items-center justify-between text-sm">
      <span className="text-foreground/90">{label}</span>
      <span className="font-mono text-muted-foreground">
        {Number(value).toFixed(step < 0.1 ? 2 : 1)}
        {unit}
      </span>
    </div>
    <input
      type="range"
      min={min}
      max={max}
      step={step}
      value={value}
      onChange={(e) => onChange(parseFloat(e.target.value))}
      className="h-2 w-full cursor-pointer appearance-none rounded-full bg-zinc-300/70 dark:bg-zinc-700/70
      [&::-webkit-slider-thumb]:h-4 [&::-webkit-slider-thumb]:w-4 [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:rounded-full [&::-webkit-slider-thumb]:border [&::-webkit-slider-thumb]:border-border [&::-webkit-slider-thumb]:bg-white dark:[&::-webkit-slider-thumb]:bg-zinc-100"
    />
  </div>
);

function normalizeHexColor(value: string) {
  const trimmed = value.trim();
  if (!/^#?[0-9a-fA-F]{6}$/.test(trimmed)) {
    return null;
  }
  return `#${trimmed.replace("#", "").toLowerCase()}`;
}

const ColorPicker = ({
  value,
  onChange,
  label,
  defaultColor = "#000000",
}: {
  value: string;
  onChange: (val: string) => void;
  label: string;
  defaultColor?: string;
}) => {
  const [draft, setDraft] = useState(value);

  // Sync draft when value changes externally
  useEffect(() => {
    setDraft(value);
  }, [value]);

  const isValidHex = /^#?[0-9a-fA-F]{6}$/.test(value);
  // If not valid, fallback to the default computed color for the color input UI
  const colorInputVal = isValidHex
    ? value.startsWith("#")
      ? value
      : `#${value}`
    : defaultColor;

  return (
    <label className="flex items-center gap-3 rounded-md border border-border/70 p-2.5">
      <span className="relative h-8 w-8 overflow-hidden rounded border border-border">
        {isValidHex ? (
          <span
            aria-hidden="true"
            className="absolute inset-0"
            style={{ backgroundColor: value }}
          />
        ) : (
          <span
            aria-hidden="true"
            className="absolute inset-0"
            style={{ backgroundColor: defaultColor }}
          />
        )}
        <input
          type="color"
          value={colorInputVal}
          onInput={(e) => onChange((e.target as HTMLInputElement).value)}
          onChange={(e) => onChange(e.target.value)}
          aria-label={`${label} color picker`}
          className="absolute inset-0 h-full w-full cursor-pointer opacity-0"
        />
      </span>
      <span className="min-w-0 flex-1">
        <span className="mb-1 block text-xs font-semibold text-foreground/90">
          {label}
        </span>
        <input
          value={draft}
          onChange={(e) => {
            const next = e.target.value;
            setDraft(next);
            if (next === "") {
              onChange("");
              return;
            }
            const normalized = normalizeHexColor(next);
            if (normalized) {
              onChange(normalized);
            }
          }}
          placeholder="e.g. #000000 or empty for auto"
          className="h-8 w-full rounded border border-border/70 bg-transparent px-2.5 font-mono text-xs text-foreground placeholder:text-muted-foreground/70 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-zinc-400/25"
        />
      </span>
    </label>
  );
};

function generateCode(config: SignatureConfig) {
  const props = Object.entries(config)
    .filter(([key, value]) => {
      if (value === undefined || value === null) return false;
      if (key === "color" && value === "") return false;
      return true;
    })
    .map(([key, value]) => {
      if (typeof value === "string") {
        return `${key}="${value}"`;
      }
      return `${key}={${value}}`;
    })
    .join("\n  ");

  return `import { Signature } from "@/components/ui/signature"\n\n<Signature\n  ${props}\n/>`;
}

import { useTheme } from "next-themes";

export function SignaturePlayground() {
  const config = usePlaygroundStore((state) => state.signatureConfig);
  const renderVersion = usePlaygroundStore(
    (state) => state.signatureRenderVersion,
  );

  useEffect(() => {
    const code = generateCode(config);
    const timeoutId = setTimeout(() => {
      usePlaygroundStore.getState().setCode(code);
    }, 250);

    return () => clearTimeout(timeoutId);
  }, [config]);

  return (
    <div className="relative h-full w-full flex items-center justify-center p-8">
      <Signature
        key={renderVersion}
        text={config.text}
        color={config.color || undefined}
        fontSize={config.fontSize}
        duration={config.duration}
      />
    </div>
  );
}

export function SignaturePersonalizePanel() {
  const { resolvedTheme } = useTheme();

  const config = usePlaygroundStore((state) => state.signatureConfig);
  const activePreset = usePlaygroundStore(
    (state) => state.activeSignaturePreset,
  );
  const setConfig = usePlaygroundStore((state) => state.setSignatureConfig);
  const setActivePreset = usePlaygroundStore(
    (state) => state.setActiveSignaturePreset,
  );
  const updateConfig = usePlaygroundStore(
    (state) => state.updateSignatureConfig,
  );
  const resetPreview = usePlaygroundStore(
    (state) => state.resetSignaturePreview,
  );
  const resetConfig = usePlaygroundStore((state) => state.resetSignatureConfig);

  const selectedPresetConfig = useMemo(
    () => PRESETS.find((preset) => preset.name === activePreset)?.config,
    [activePreset],
  );

  // Compute the default hex visual color to show when the custom color is empty/dynamic
  const pickerDefaultColor = resolvedTheme === "dark" ? "#ffffff" : "#000000";

  const handleChange = (key: keyof SignatureConfig, value: string | number) => {
    updateConfig({ [key]: value } as Partial<SignatureConfig>);

    if (activePreset === "Custom") {
      return;
    }

    const matchedPreset = PRESETS.find(
      (preset) =>
        JSON.stringify(preset.config) ===
        JSON.stringify({ ...config, [key]: value }),
    );
    setActivePreset(matchedPreset?.name ?? "Custom");
  };

  const handlePresetChange = (presetName: string) => {
    const preset = PRESETS.find((item) => item.name === presetName);
    if (!preset) {
      return;
    }

    setConfig({ ...preset.config });
    setActivePreset(presetName);
    resetPreview();
  };

  return (
    <div className="h-full overflow-auto bg-[#f3f4f6] dark:bg-[#080808] [&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]">
      <div className="space-y-6 px-4 pb-10 pt-20">
        <header className="space-y-2">
          <h2 className="text-2xl font-bold tracking-tighter text-foreground">
            Personalize
          </h2>
          <p className="text-sm leading-relaxed text-muted-foreground/90">
            Customize the signature text, size, color, and trace speed.
          </p>
        </header>

        <div className="flex items-center justify-between">
          <div className="text-sm text-muted-foreground/90">
            Current preset:{" "}
            <span className="font-mono text-foreground">{activePreset}</span>
          </div>
          <button
            type="button"
            onClick={resetConfig}
            className="rounded-md border border-border/40 bg-white/50 px-3 py-1.5 text-[10px] font-mono uppercase tracking-widest text-muted-foreground transition-colors hover:text-foreground dark:bg-white/[0.03]"
          >
            Reset
          </button>
        </div>

        <div>
          <SectionTitle>Presets</SectionTitle>
          <div className="grid grid-cols-3 gap-1.5">
            {PRESETS.map((preset) => {
              const isActive =
                activePreset === preset.name ||
                (activePreset !== "Custom" &&
                  selectedPresetConfig &&
                  preset.name === activePreset);

              return (
                <button
                  key={preset.name}
                  type="button"
                  onClick={() => handlePresetChange(preset.name)}
                  className={cn(
                    "rounded-md border p-1.5 text-center transition-colors",
                    isActive
                      ? "border-zinc-500/70 bg-zinc-100 dark:border-zinc-500/80 dark:bg-zinc-900/70"
                      : "border-border/70 bg-transparent",
                  )}
                >
                  <span className="block truncate text-[10px] font-mono uppercase tracking-widest text-foreground/90">
                    {preset.name}
                  </span>
                </button>
              );
            })}
          </div>
        </div>

        <div>
          <SectionTitle>Content</SectionTitle>
          <div className="grid grid-cols-1 gap-2.5">
            <Input
              value={config.text}
              onChange={(e) => handleChange("text", e.target.value)}
              placeholder="Text to trace"
            />
          </div>
        </div>

        <div>
          <SectionTitle>Styling</SectionTitle>
          <div className="grid grid-cols-1 gap-2.5">
            <ColorPicker
              label="Color"
              value={config.color}
              defaultColor={pickerDefaultColor}
              onChange={(v) => handleChange("color", v)}
            />
          </div>
        </div>

        <div>
          <SectionTitle>Parameters</SectionTitle>
          <div className="grid grid-cols-1 gap-3">
            <Slider
              label="Font Size"
              min={16}
              max={120}
              step={1}
              value={config.fontSize}
              onChange={(v) => handleChange("fontSize", v)}
              unit="px"
            />
            <Slider
              label="Duration"
              min={0.5}
              max={10}
              step={0.1}
              value={config.duration}
              onChange={(v) => handleChange("duration", v)}
              unit="s"
            />
          </div>
        </div>
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/signature.tsx`
```tsx
"use client";

import { useEffect, useId, useState } from "react";
import { motion } from "framer-motion";
import opentype from "opentype.js";
import { cn } from "@/lib/utils";

interface SignatureProps {
  /** Text to generate signature for */
  text?: string;
  /** Color of the signature path */
  color?: string;
  /** Font size of the signature */
  fontSize?: number;
  /** Animation duration in seconds */
  duration?: number;
  /** Delay before animation starts in seconds */
  delay?: number;
  /** Additional CSS classes */
  className?: string;
  /** Only animate when in view */
  inView?: boolean;
  /** Only animate once */
  once?: boolean;
  /** Custom font URL to load */
  fontUrl?: string;
}

export function Signature({
  text = "Signature",
  color = "currentColor",
  fontSize = 32,
  duration = 1.5,
  delay = 0,
  className,
  inView = false,
  once = true,
  fontUrl,
}: SignatureProps) {
  const [paths, setPaths] = useState<string[]>([]);
  const [width, setWidth] = useState<number>(300);
  const height = fontSize * 3; // Give plenty of vertical space
  const horizontalPadding = fontSize * 0.1;
  const topMargin = fontSize * 1.5; // Shift down
  const baseline = topMargin;
  const maskId = `signature-reveal-${useId().replace(/:/g, "")}`;

  useEffect(() => {
    async function load() {
      try {
        let font;
        const fontPaths = fontUrl 
          ? [fontUrl] 
          : [
              "/LastoriaBoldRegular.otf",
              "./LastoriaBoldRegular.otf",
              "https://www.componentry.fun/LastoriaBoldRegular.otf",
            ];

        for (const path of fontPaths) {
          try {
            font = await opentype.load(path as string);
            break;
          } catch {
            // Try next path
          }
        }

        if (!font) {
          throw new Error("Font could not be loaded from any path");
        }

        let x = horizontalPadding;
        const newPaths: string[] = [];

        for (const char of text) {
          const glyph = font.charToGlyph(char);
          const path = glyph.getPath(x, baseline, fontSize);
          newPaths.push(path.toPathData(3));

          const advanceWidth = glyph.advanceWidth ?? font.unitsPerEm;
          x += advanceWidth * (fontSize / font.unitsPerEm);
        }

        setPaths(newPaths);
        setWidth(x + horizontalPadding);
      } catch (error) {
        console.error("Signature component font load error:", error);
        setPaths([]);
        setWidth(text.length * fontSize * 0.6);
      }
    }

    load();
  }, [text, fontSize, baseline, horizontalPadding, fontUrl]);

  const variants = {
    hidden: { pathLength: 0, opacity: 0 },
    visible: { pathLength: 1, opacity: 1 },
  };

  return (
    <motion.svg
      key={paths.length}
      width={width}
      height={height}
      viewBox={`0 0 ${width} ${height}`}
      fill="none"
      className={cn("text-foreground overflow-visible", className)}
      initial="hidden"
      whileInView={inView ? "visible" : undefined}
      animate={inView ? undefined : "visible"}
      viewport={{ once }}
    >
      <defs>
        <mask id={maskId} maskUnits="userSpaceOnUse">
          {paths.map((d, i) => (
            <motion.path
              key={i}
              d={d}
              stroke="white"
              strokeWidth={fontSize * 0.22}
              fill="none"
              variants={variants}
              transition={{
                pathLength: {
                  delay: delay + i * 0.2,
                  duration,
                  ease: "easeInOut",
                },
                opacity: {
                  delay: delay + i * 0.2 + 0.01,
                  duration: 0.01,
                },
              }}
              vectorEffect="non-scaling-stroke"
              strokeLinecap="round"
              strokeLinejoin="round"
            />
          ))}
        </mask>
      </defs>

      {paths.map((d, i) => (
        <motion.path
          key={i}
          d={d}
          stroke={color}
          strokeWidth={2}
          fill="none"
          variants={variants}
          transition={{
            pathLength: {
              delay: delay + i * 0.2,
              duration,
              ease: "easeInOut",
            },
            opacity: {
              delay: delay + i * 0.2 + 0.01,
              duration: 0.01,
            },
          }}
          vectorEffect="non-scaling-stroke"
          strokeLinecap="butt"
          strokeLinejoin="round"
        />
      ))}

      <g mask={`url(#${maskId})`}>
        {paths.map((d, i) => <path key={i} d={d} fill={color} />)}
      </g>
    </motion.svg>
  );
}
```
---

## Split Flap Display <a name="split-flap-display"></a>

Component for split-flap-display

- **Registry Key**: `split-flap-display`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/split-flap-display.json
```

#### How to Use
```tsx
"use client";

import { useEffect, useState } from "react";
import { SplitFlapDisplay } from "@workspace/ui/components/split-flap-display";
import { cn } from "@/lib/utils";
import {
  SPLIT_FLAP_DISPLAY_DEFAULT_CONFIG,
  type SplitFlapDisplayConfig,
  usePlaygroundStore,
} from "@/hooks/use-playground-store";

const PRESETS: Array<{ name: string; config: SplitFlapDisplayConfig }> = [
  {
    name: "Default",
    config: SPLIT_FLAP_DISPLAY_DEFAULT_CONFIG,
  },
  {
    name: "Airport",
    config: {
      ...SPLIT_FLAP_DISPLAY_DEFAULT_CONFIG,
      text: "FLIGHT 404",
      columns: 12,
      size: "lg",
      accentColor: "#22c55e",
      showIndicators: true,
      staggerDelay: 25,
      flipSpeed: 30,
    },
  },
  {
    name: "Compact",
    config: {
      ...SPLIT_FLAP_DISPLAY_DEFAULT_CONFIG,
      text: "GATE B42",
      columns: 10,
      size: "sm",
      accentColor: "#3b82f6",
      showIndicators: true,
      staggerDelay: 20,
      flipSpeed: 25,
    },
  },
  {
    name: "Gold",
    config: {
      ...SPLIT_FLAP_DISPLAY_DEFAULT_CONFIG,
      text: "PREMIUM",
      columns: 10,
      size: "lg",
      accentColor: "#eab308",
      showIndicators: true,
      staggerDelay: 40,
      flipSpeed: 40,
    },
  },
  {
    name: "Minimal",
    config: {
      ...SPLIT_FLAP_DISPLAY_DEFAULT_CONFIG,
      text: "HELLO WORLD",
      columns: 14,
      size: "md",
      accentColor: "#22c55e",
      showIndicators: false,
      staggerDelay: 30,
      flipSpeed: 35,
    },
  },
];

const SectionTitle = ({ children }: { children: React.ReactNode }) => (
  <div className="mb-3 text-[10px] font-mono uppercase tracking-widest text-muted-foreground">{children}</div>
);

const Input = ({ className, ...props }: React.InputHTMLAttributes<HTMLInputElement>) => (
  <input
    className={cn(
      "h-10 w-full rounded-md border border-border/70 bg-transparent px-3 text-sm text-foreground placeholder:text-muted-foreground/70 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-zinc-400/25",
      className,
    )}
    {...props}
  />
);

const Slider = ({
  value,
  min,
  max,
  step,
  onChange,
  label,
  unit = "",
}: {
  value: number;
  min: number;
  max: number;
  step: number;
  onChange: (val: number) => void;
  label: string;
  unit?: string;
}) => (
  <div className="space-y-2">
    <div className="flex items-center justify-between text-sm">
      <span className="text-foreground/90">{label}</span>
      <span className="font-mono text-muted-foreground">
        {Number(value).toFixed(step < 1 ? 0 : 0)}
        {unit}
      </span>
    </div>
    <input
      type="range"
      min={min}
      max={max}
      step={step}
      value={value}
      onChange={(e) => onChange(parseFloat(e.target.value))}
      className="h-2 w-full cursor-pointer appearance-none rounded-full bg-zinc-300/70 dark:bg-zinc-700/70
      [&::-webkit-slider-thumb]:h-4 [&::-webkit-slider-thumb]:w-4 [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:rounded-full [&::-webkit-slider-thumb]:border [&::-webkit-slider-thumb]:border-border [&::-webkit-slider-thumb]:bg-white dark:[&::-webkit-slider-thumb]:bg-zinc-100"
    />
  </div>
);

function normalizeHexColor(value: string) {
  const trimmed = value.trim();
  if (!/^#?[0-9a-fA-F]{6}$/.test(trimmed)) return null;
  return `#${trimmed.replace("#", "").toLowerCase()}`;
}

const ColorPicker = ({
  value,
  onChange,
  label,
}: {
  value: string;
  onChange: (val: string) => void;
  label: string;
}) => {
  const [draft, setDraft] = useState(value);

  useEffect(() => {
    setDraft(value);
  }, [value]);

  return (
    <label className="flex items-center gap-3 rounded-md border border-border/70 p-2.5">
      <span className="relative h-8 w-8 overflow-hidden rounded border border-border">
        <span aria-hidden="true" className="absolute inset-0" style={{ backgroundColor: value }} />
        <input
          type="color"
          value={value}
          onInput={(e) => onChange((e.target as HTMLInputElement).value)}
          onChange={(e) => onChange(e.target.value)}
          aria-label={`${label} color picker`}
          className="absolute inset-0 h-full w-full cursor-pointer opacity-0"
        />
      </span>
      <span className="min-w-0 flex-1">
        <span className="mb-1 block text-xs font-semibold text-foreground/90">{label}</span>
        <input
          value={draft}
          onChange={(e) => {
            const next = e.target.value;
            setDraft(next);
            const normalized = normalizeHexColor(next);
            if (normalized) onChange(normalized);
          }}
          placeholder="#000000"
          className="h-8 w-full rounded border border-border/70 bg-transparent px-2.5 font-mono text-xs text-foreground placeholder:text-muted-foreground/70 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-zinc-400/25"
        />
      </span>
    </label>
  );
};

function generateCode(config: SplitFlapDisplayConfig) {
  const lines: string[] = [];
  lines.push(`import { SplitFlapDisplay } from "@/components/ui/split-flap-display"`);
  lines.push(``);
  lines.push(`<SplitFlapDisplay`);
  lines.push(`  text="${config.text}"`);
  lines.push(`  columns={${config.columns}}`);
  lines.push(`  size="${config.size}"`);
  if (config.accentColor !== "#22c55e") lines.push(`  accentColor="${config.accentColor}"`);
  if (!config.showIndicators) lines.push(`  showIndicators={false}`);
  if (config.staggerDelay !== 30) lines.push(`  staggerDelay={${config.staggerDelay}}`);
  if (config.flipSpeed !== 35) lines.push(`  flipSpeed={${config.flipSpeed}}`);
  lines.push(`/>`);
  return lines.join("\n");
}

/* ─── Playground Preview ───────────────────────────────── */

export function SplitFlapDisplayPlayground() {
  const config = usePlaygroundStore((s) => s.splitFlapDisplayConfig);
  const renderVersion = usePlaygroundStore((s) => s.splitFlapDisplayRenderVersion);

  useEffect(() => {
    const code = generateCode(config);
    const t = setTimeout(() => {
      usePlaygroundStore.getState().setCode(code);
    }, 250);
    return () => clearTimeout(t);
  }, [config]);

  return (
    <div className="relative flex h-full w-full items-center justify-center bg-black p-10">
      <SplitFlapDisplay
        key={renderVersion}
        text={config.text}
        columns={config.columns}
        size={config.size}
        accentColor={config.accentColor}
        showIndicators={config.showIndicators}
        staggerDelay={config.staggerDelay}
        flipSpeed={config.flipSpeed}
      />
    </div>
  );
}

/* ─── Personalize Panel ────────────────────────────────── */

export function SplitFlapDisplayPersonalizePanel() {
  const config = usePlaygroundStore((s) => s.splitFlapDisplayConfig);
  const activePreset = usePlaygroundStore((s) => s.activeSplitFlapDisplayPreset);
  const setConfig = usePlaygroundStore((s) => s.setSplitFlapDisplayConfig);
  const setActivePreset = usePlaygroundStore((s) => s.setActiveSplitFlapDisplayPreset);
  const updateConfig = usePlaygroundStore((s) => s.updateSplitFlapDisplayConfig);
  const resetPreview = usePlaygroundStore((s) => s.resetSplitFlapDisplayPreview);
  const resetConfig = usePlaygroundStore((s) => s.resetSplitFlapDisplayConfig);

  const handleChange = (key: keyof SplitFlapDisplayConfig, value: string | number | boolean) => {
    updateConfig({ [key]: value } as Partial<SplitFlapDisplayConfig>);
    resetPreview();

    if (activePreset === "Custom") return;

    const matchedPreset = PRESETS.find(
      (preset) => JSON.stringify(preset.config) === JSON.stringify({ ...config, [key]: value }),
    );
    setActivePreset(matchedPreset?.name ?? "Custom");
  };

  const handlePresetChange = (presetName: string) => {
    const preset = PRESETS.find((item) => item.name === presetName);
    if (!preset) return;
    setConfig({ ...preset.config });
    setActivePreset(presetName);
    resetPreview();
  };

  return (
    <div className="h-full overflow-auto bg-[#f3f4f6] dark:bg-[#080808] [&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]">
      <div className="space-y-6 px-4 pb-10 pt-20">
        <header className="space-y-2">
          <h2 className="text-2xl font-bold tracking-tighter text-foreground">Personalize</h2>
          <p className="text-sm leading-relaxed text-muted-foreground/90">
            Customize your split-flap board with text, sizing, speed, and accent colors.
          </p>
        </header>

        <div className="flex items-center justify-between">
          <div className="text-sm text-muted-foreground/90">
            Current preset: <span className="font-mono text-foreground">{activePreset}</span>
          </div>
          <button
            type="button"
            onClick={resetConfig}
            className="rounded-md border border-border/40 bg-white/50 px-3 py-1.5 text-[10px] font-mono uppercase tracking-widest text-muted-foreground transition-colors hover:text-foreground dark:bg-white/[0.03]"
          >
            Reset
          </button>
        </div>

        {/* Presets */}
        <div>
          <SectionTitle>Presets</SectionTitle>
          <div className="grid grid-cols-5 gap-1.5">
            {PRESETS.map((preset) => {
              const isActive = activePreset === preset.name;
              return (
                <button
                  key={preset.name}
                  type="button"
                  onClick={() => handlePresetChange(preset.name)}
                  className={cn(
                    "rounded-md border p-1.5 text-center transition-colors",
                    isActive
                      ? "border-zinc-500/70 bg-zinc-100 dark:border-zinc-500/80 dark:bg-zinc-900/70"
                      : "border-border/70 bg-transparent",
                  )}
                >
                  <div
                    className="mb-1.5 h-4 rounded-sm border border-white/20"
                    style={{
                      background: `linear-gradient(125deg, #111 0%, ${preset.config.accentColor} 100%)`,
                    }}
                  />
                  <span className="block truncate text-[10px] font-mono uppercase tracking-widest text-foreground/90">
                    {preset.name}
                  </span>
                </button>
              );
            })}
          </div>
        </div>

        {/* Content */}
        <div>
          <SectionTitle>Content</SectionTitle>
          <div className="space-y-2.5">
            <Input
              value={config.text}
              onChange={(e) => handleChange("text", e.target.value.toUpperCase())}
              placeholder="Display text"
            />
          </div>
        </div>

        {/* Layout */}
        <div>
          <SectionTitle>Layout</SectionTitle>
          <div className="space-y-3">
            <Slider
              label="Columns"
              min={4}
              max={20}
              step={1}
              value={config.columns}
              onChange={(v) => handleChange("columns", v)}
            />
            <div>
              <span className="mb-2 block text-sm text-foreground/90">Size</span>
              <div className="grid grid-cols-3 gap-1.5">
                {(["sm", "md", "lg"] as const).map((s) => (
                  <button
                    key={s}
                    type="button"
                    onClick={() => handleChange("size", s)}
                    className={cn(
                      "rounded-md border px-3 py-2 text-xs font-mono uppercase tracking-widest transition-colors",
                      config.size === s
                        ? "border-zinc-500/70 bg-zinc-100 text-foreground dark:border-zinc-500/80 dark:bg-zinc-900/70"
                        : "border-border/70 bg-transparent text-muted-foreground",
                    )}
                  >
                    {s}
                  </button>
                ))}
              </div>
            </div>
            <div className="flex items-center justify-between">
              <span className="text-sm text-foreground/90">Show Indicators</span>
              <button
                type="button"
                onClick={() => handleChange("showIndicators", !config.showIndicators)}
                className={cn(
                  "relative h-6 w-11 rounded-full transition-colors",
                  config.showIndicators ? "bg-green-500" : "bg-zinc-300 dark:bg-zinc-700",
                )}
              >
                <span
                  className={cn(
                    "absolute top-0.5 left-0.5 h-5 w-5 rounded-full bg-white shadow transition-transform",
                    config.showIndicators && "translate-x-5",
                  )}
                />
              </button>
            </div>
          </div>
        </div>

        {/* Style */}
        <div>
          <SectionTitle>Style</SectionTitle>
          <ColorPicker
            label="Accent Color"
            value={config.accentColor}
            onChange={(v) => handleChange("accentColor", v)}
          />
        </div>

        {/* Animation */}
        <div>
          <SectionTitle>Animation</SectionTitle>
          <div className="grid grid-cols-1 gap-3">
            <Slider
              label="Stagger Delay"
              min={5}
              max={80}
              step={5}
              value={config.staggerDelay}
              onChange={(v) => handleChange("staggerDelay", v)}
              unit="ms"
            />
            <Slider
              label="Flip Speed"
              min={15}
              max={80}
              step={5}
              value={config.flipSpeed}
              onChange={(v) => handleChange("flipSpeed", v)}
              unit="ms"
            />
          </div>
        </div>
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/split-flap-display.tsx`
```tsx
"use client";

import { cn } from "@/lib/utils";
import { useEffect, useRef, useState, useCallback } from "react";

/* ─── Types ─────────────────────────────────────────────────── */

interface SplitFlapRow {
  /** Label text shown on the left side of the row */
  label: string;
  /** Value text shown on the right side of the row */
  value: string;
}

interface SplitFlapDisplayProps {
  /** Rows of label + value pairs to display on the board */
  rows?: SplitFlapRow[];
  /** Simple text mode – renders a single row of characters */
  text?: string;
  /** Total number of character cells per row (pads shorter strings with spaces) */
  columns?: number;
  /** Size variant controlling cell dimensions and typography */
  size?: "sm" | "md" | "lg";
  /** Accent color for the side indicator strips */
  accentColor?: string;
  /** Whether to show the green indicator strips on each row */
  showIndicators?: boolean;
  /** Stagger delay in ms between each character starting its flip (creates a wave) */
  staggerDelay?: number;
  /** Speed in ms for each individual character flip step */
  flipSpeed?: number;
  /** Additional CSS classes for the outer container */
  className?: string;
}

/* ─── Character Set ─────────────────────────────────────────── */

const CHARACTERS = " ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789$.,!?:;+-=%&#@";

function getNextChar(current: string): string {
  const idx = CHARACTERS.indexOf(current);
  if (idx === -1 || idx >= CHARACTERS.length - 1) return CHARACTERS[0] ?? " ";
  return CHARACTERS[idx + 1] ?? " ";
}

/* ─── Individual Flap Cell ──────────────────────────────────── */

function FlapCell({
  targetChar,
  size = "md",
  delay = 0,
  flipSpeed = 35,
}: {
  targetChar: string;
  size?: "sm" | "md" | "lg";
  delay?: number;
  flipSpeed?: number;
}) {
  const [displayChar, setDisplayChar] = useState(" ");
  const [isFlipping, setIsFlipping] = useState(false);
  const [flipPhase, setFlipPhase] = useState<"idle" | "top-down" | "bottom-up">("idle");
  const prevCharRef = useRef(" ");
  const timeoutRef = useRef<ReturnType<typeof setTimeout> | null>(null);
  const intervalRef = useRef<ReturnType<typeof setInterval> | null>(null);

  const cleanup = useCallback(() => {
    if (timeoutRef.current) clearTimeout(timeoutRef.current);
    if (intervalRef.current) clearInterval(intervalRef.current);
  }, []);

  useEffect(() => {
    cleanup();
    const target = targetChar.toUpperCase();

    if (displayChar === target) {
      setIsFlipping(false);
      setFlipPhase("idle");
      return;
    }

    timeoutRef.current = setTimeout(() => {
      setIsFlipping(true);

      intervalRef.current = setInterval(() => {
        setDisplayChar((prev) => {
          const next = getNextChar(prev);
          // Trigger flip animation phases
          setFlipPhase("top-down");
          setTimeout(() => setFlipPhase("bottom-up"), flipSpeed * 0.4);
          setTimeout(() => setFlipPhase("idle"), flipSpeed * 0.8);

          prevCharRef.current = prev;

          if (next === target) {
            if (intervalRef.current) clearInterval(intervalRef.current);
            setTimeout(() => {
              setIsFlipping(false);
              setFlipPhase("idle");
            }, flipSpeed);
          }
          return next;
        });
      }, flipSpeed);
    }, delay);

    return cleanup;
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [targetChar]);

  const sizeMap = {
    sm: { cell: "w-[26px] h-[38px] text-[16px]", gap: "gap-[1px]" },
    md: { cell: "w-[38px] h-[54px] text-[24px]", gap: "gap-[1px]" },
    lg: { cell: "w-[52px] h-[72px] text-[34px]", gap: "gap-[2px]" },
  };

  const s = sizeMap[size];

  return (
    <div
      className={cn(
        "relative select-none font-mono font-bold",
        s.cell
      )}
      style={{ perspective: "400px" }}
    >
      {/* ── Static top half ──────────────── */}
      <div
        className="absolute inset-x-0 top-0 h-1/2 overflow-hidden rounded-t-[3px]"
        style={{
          background: "linear-gradient(180deg, #1e1e1e 0%, #181818 100%)",
          borderBottom: "none",
        }}
      >
        <span
          className="absolute left-1/2 bottom-0 -translate-x-1/2 translate-y-[48%] text-[#e8e6e3] drop-shadow-[0_1px_1px_rgba(0,0,0,0.6)]"
          style={{ fontFamily: "'SF Mono', 'Fira Code', 'Courier New', monospace" }}
        >
          {displayChar}
        </span>
      </div>

      {/* ── Static bottom half ───────────── */}
      <div
        className="absolute inset-x-0 bottom-0 h-1/2 overflow-hidden rounded-b-[3px]"
        style={{
          background: "linear-gradient(180deg, #151515 0%, #111111 100%)",
        }}
      >
        <span
          className="absolute left-1/2 top-0 -translate-x-1/2 -translate-y-[48%] text-[#d4d2cf] drop-shadow-[0_-1px_1px_rgba(0,0,0,0.6)]"
          style={{ fontFamily: "'SF Mono', 'Fira Code', 'Courier New', monospace" }}
        >
          {displayChar}
        </span>
      </div>

      {/* ── Flipping top panel ───────────── */}
      {isFlipping && flipPhase === "top-down" && (
        <div
          className="absolute inset-x-0 top-0 h-1/2 overflow-hidden rounded-t-[3px] z-10"
          style={{
            background: "linear-gradient(180deg, #222 0%, #1a1a1a 100%)",
            transformOrigin: "bottom center",
            animation: `flapTopDown ${flipSpeed * 0.4}ms ease-in forwards`,
          }}
        >
          <span
            className="absolute left-1/2 bottom-0 -translate-x-1/2 translate-y-[48%] text-[#e8e6e3]"
            style={{ fontFamily: "'SF Mono', 'Fira Code', 'Courier New', monospace" }}
          >
            {prevCharRef.current}
          </span>
        </div>
      )}

      {/* ── Flipping bottom panel ────────── */}
      {isFlipping && flipPhase === "bottom-up" && (
        <div
          className="absolute inset-x-0 bottom-0 h-1/2 overflow-hidden rounded-b-[3px] z-10"
          style={{
            background: "linear-gradient(180deg, #181818 0%, #111 100%)",
            transformOrigin: "top center",
            animation: `flapBottomUp ${flipSpeed * 0.4}ms ease-out forwards`,
          }}
        >
          <span
            className="absolute left-1/2 top-0 -translate-x-1/2 -translate-y-[48%] text-[#d4d2cf]"
            style={{ fontFamily: "'SF Mono', 'Fira Code', 'Courier New', monospace" }}
          >
            {displayChar}
          </span>
        </div>
      )}

      {/* ── Center divider line ──────────── */}
      <div
        className="absolute inset-x-0 top-1/2 -translate-y-1/2 z-20 pointer-events-none"
        style={{
          height: "2px",
          background: "linear-gradient(90deg, rgba(0,0,0,0.9) 0%, rgba(0,0,0,0.7) 50%, rgba(0,0,0,0.9) 100%)",
          boxShadow: "0 1px 0 rgba(255,255,255,0.04)",
        }}
      />

      {/* ── Subtle inner shadow overlay ──── */}
      <div
        className="absolute inset-0 rounded-[3px] z-20 pointer-events-none"
        style={{
          boxShadow: "inset 0 1px 2px rgba(0,0,0,0.5), inset 0 -1px 2px rgba(0,0,0,0.3)",
        }}
      />
    </div>
  );
}

/* ─── Indicator Strip (green side bar) ──────────────────────── */

function IndicatorStrip({
  color = "#22c55e",
  size = "md",
}: {
  color?: string;
  size?: "sm" | "md" | "lg";
}) {
  const heightMap = { sm: "h-[38px]", md: "h-[54px]", lg: "h-[72px]" };
  return (
    <div
      className={cn("w-[6px] rounded-[2px] flex-shrink-0 self-stretch", heightMap[size])}
      style={{
        background: `linear-gradient(180deg, ${color} 0%, ${color}99 40%, ${color}66 60%, ${color}99 100%)`,
        boxShadow: `0 0 8px ${color}44, inset 0 1px 2px rgba(255,255,255,0.2)`,
      }}
    />
  );
}

/* ─── Row Component ─────────────────────────────────────────── */

function FlapRow({
  text,
  columns,
  size = "md",
  accentColor = "#22c55e",
  showIndicators = true,
  staggerDelay = 30,
  flipSpeed = 35,
}: {
  text: string;
  columns: number;
  size?: "sm" | "md" | "lg";
  accentColor?: string;
  showIndicators?: boolean;
  staggerDelay?: number;
  flipSpeed?: number;
}) {
  const padded = text.toUpperCase().padEnd(columns, " ").substring(0, columns);

  return (
    <div className="flex items-center gap-1.5">
      {showIndicators && <IndicatorStrip color={accentColor} size={size} />}
      <div className="flex gap-[3px]">
        {padded.split("").map((char, i) => (
          <FlapCell
            key={i}
            targetChar={char}
            size={size}
            delay={i * staggerDelay}
            flipSpeed={flipSpeed}
          />
        ))}
      </div>
      {showIndicators && <IndicatorStrip color={accentColor} size={size} />}
    </div>
  );
}

/* ─── Keyframes (injected once) ─────────────────────────────── */

const KEYFRAMES_ID = "split-flap-keyframes";

function useInjectKeyframes() {
  useEffect(() => {
    if (typeof document === "undefined") return;
    if (document.getElementById(KEYFRAMES_ID)) return;

    const style = document.createElement("style");
    style.id = KEYFRAMES_ID;
    style.textContent = `
      @keyframes flapTopDown {
        0%   { transform: rotateX(0deg); }
        100% { transform: rotateX(-90deg); }
      }
      @keyframes flapBottomUp {
        0%   { transform: rotateX(90deg); }
        100% { transform: rotateX(0deg); }
      }
    `;
    document.head.appendChild(style);

    return () => {
      const el = document.getElementById(KEYFRAMES_ID);
      if (el) el.remove();
    };
  }, []);
}

/* ─── Main Export ────────────────────────────────────────────── */

export function SplitFlapDisplay({
  rows,
  text,
  columns = 14,
  size = "md",
  accentColor = "#22c55e",
  showIndicators = true,
  staggerDelay = 30,
  flipSpeed = 35,
  className,
}: SplitFlapDisplayProps) {
  useInjectKeyframes();

  // Simple single-row text mode
  if (text && !rows) {
    return (
      <div
        className={cn(
          "inline-flex flex-col gap-2 p-4 rounded-2xl",
          className
        )}
        style={{
          background: "linear-gradient(145deg, #0c0c0c 0%, #080808 50%, #0a0a0a 100%)",
          border: "1px solid rgba(255,255,255,0.06)",
          boxShadow:
            "0 20px 60px rgba(0,0,0,0.8), 0 0 0 1px rgba(255,255,255,0.03), inset 0 1px 0 rgba(255,255,255,0.04)",
        }}
      >
        <FlapRow
          text={text}
          columns={columns}
          size={size}
          accentColor={accentColor}
          showIndicators={showIndicators}
          staggerDelay={staggerDelay}
          flipSpeed={flipSpeed}
        />
      </div>
    );
  }

  // Multi-row board mode
  return (
    <div
      className={cn(
        "inline-flex flex-col gap-2 p-5 rounded-2xl",
        className
      )}
      style={{
        background: "linear-gradient(145deg, #0c0c0c 0%, #080808 50%, #0a0a0a 100%)",
        border: "1px solid rgba(255,255,255,0.06)",
        boxShadow:
          "0 25px 80px rgba(0,0,0,0.9), 0 0 0 1px rgba(255,255,255,0.03), inset 0 1px 0 rgba(255,255,255,0.04)",
      }}
    >
      {(rows ?? []).map((row, idx) => {
        const combined = `${row.label}${" ".repeat(Math.max(1, columns - row.label.length - row.value.length))}${row.value}`;
        return (
          <FlapRow
            key={idx}
            text={combined}
            columns={columns}
            size={size}
            accentColor={accentColor}
            showIndicators={showIndicators}
            staggerDelay={staggerDelay}
            flipSpeed={flipSpeed}
          />
        );
      })}
    </div>
  );
}
```
---

## Spotlight Card <a name="spotlight-card"></a>

Interactive cards with cursor-following spotlight effects, animated gradient borders, and 3D tilt animations.

- **Registry Key**: `spotlight-card`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/spotlight-card.json
```

#### Component Source Code
##### File: `components/ui/spotlight-card.tsx`
```tsx
"use client"

import * as React from "react"
import { cn } from "@workspace/ui/lib/utils"

interface SpotlightCardProps extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode
  spotlightColor?: string
  borderColor?: string
  borderWidth?: number
  borderRadius?: number
  glowIntensity?: number
}

function SpotlightCard({
  children,
  className,
  spotlightColor = "rgba(120, 119, 198, 0.3)",
  borderColor,
  borderWidth = 1,
  borderRadius = 16,
  glowIntensity = 0.15,
  ...props
}: SpotlightCardProps) {
  const containerRef = React.useRef<HTMLDivElement>(null)
  const [position, setPosition] = React.useState({ x: 0, y: 0 })
  const [isHovered, setIsHovered] = React.useState(false)
  const [opacity, setOpacity] = React.useState(0)

  const handleMouseMove = React.useCallback(
    (e: React.MouseEvent<HTMLDivElement>) => {
      if (!containerRef.current) return

      const rect = containerRef.current.getBoundingClientRect()
      setPosition({
        x: e.clientX - rect.left,
        y: e.clientY - rect.top,
      })
    },
    []
  )

  const handleMouseEnter = React.useCallback(() => {
    setIsHovered(true)
    setOpacity(1)
  }, [])

  const handleMouseLeave = React.useCallback(() => {
    setIsHovered(false)
    setOpacity(0)
  }, [])

  return (
    <div
      ref={containerRef}
      onMouseMove={handleMouseMove}
      onMouseEnter={handleMouseEnter}
      onMouseLeave={handleMouseLeave}
      className={cn(
        "relative overflow-hidden bg-gradient-to-b",
        "from-neutral-50 to-white",
        "dark:from-neutral-950 dark:to-neutral-900",
        "transition-all duration-500",
        className
      )}
      style={{
        borderRadius: `${borderRadius}px`,
      }}
      {...props}
    >
      {/* Gradient border */}
      <div
        className="absolute inset-0 pointer-events-none transition-opacity duration-500"
        style={{
          borderRadius: `${borderRadius}px`,
          padding: `${borderWidth}px`,
          background: borderColor
            ? borderColor
            : `conic-gradient(
                from 225deg,
                rgba(120, 119, 198, 0.9),
                rgba(120, 119, 198, 0.1) 25%,
                rgba(255, 255, 255, 0.15) 50%,
                rgba(120, 119, 198, 0.1) 75%,
                rgba(120, 119, 198, 0.9)
              )`,
          mask: "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
          maskComposite: "exclude",
          WebkitMask:
            "linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0)",
          WebkitMaskComposite: "xor",
          opacity: isHovered ? 1 : 0.5,
        }}
      />

      {/* Spotlight effect */}
      <div
        className="absolute pointer-events-none transition-opacity duration-300"
        style={{
          left: position.x,
          top: position.y,
          width: "400px",
          height: "400px",
          transform: "translate(-50%, -50%)",
          background: `radial-gradient(circle, ${spotlightColor} 0%, transparent 70%)`,
          opacity: opacity * glowIntensity * 5,
        }}
      />

      {/* Border glow on hover */}
      <div
        className="absolute inset-0 pointer-events-none transition-opacity duration-500"
        style={{
          borderRadius: `${borderRadius}px`,
          opacity: isHovered ? 0.5 : 0,
          boxShadow: `inset 0 0 30px rgba(120, 119, 198, 0.1), 0 0 30px rgba(120, 119, 198, 0.1)`,
        }}
      />

      {/* Content */}
      <div className="relative z-10">{children}</div>
    </div>
  )
}

interface SpotlightCardContentProps
  extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode
}

function SpotlightCardContent({
  children,
  className,
  ...props
}: SpotlightCardContentProps) {
  return (
    <div className={cn("p-6", className)} {...props}>
      {children}
    </div>
  )
}

interface SpotlightCardHeaderProps
  extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode
}

function SpotlightCardHeader({
  children,
  className,
  ...props
}: SpotlightCardHeaderProps) {
  return (
    <div className={cn("flex flex-col space-y-1.5 p-6 pb-0", className)} {...props}>
      {children}
    </div>
  )
}

interface SpotlightCardTitleProps
  extends React.HTMLAttributes<HTMLHeadingElement> {
  children: React.ReactNode
}

function SpotlightCardTitle({
  children,
  className,
  ...props
}: SpotlightCardTitleProps) {
  return (
    <h3
      className={cn(
        "text-xl font-semibold leading-none tracking-tight",
        "text-neutral-900 dark:text-white",
        className
      )}
      {...props}
    >
      {children}
    </h3>
  )
}

interface SpotlightCardDescriptionProps
  extends React.HTMLAttributes<HTMLParagraphElement> {
  children: React.ReactNode
}

function SpotlightCardDescription({
  children,
  className,
  ...props
}: SpotlightCardDescriptionProps) {
  return (
    <p
      className={cn("text-sm text-neutral-600 dark:text-neutral-400", className)}
      {...props}
    >
      {children}
    </p>
  )
}

// A more advanced variant with multiple spotlight sources
interface MultiSpotlightCardProps extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode
  colors?: string[]
  borderRadius?: number
}

function MultiSpotlightCard({
  children,
  className,
  colors = [
    "rgba(120, 119, 198, 0.4)",
    "rgba(255, 77, 77, 0.3)",
    "rgba(77, 255, 174, 0.3)",
  ],
  borderRadius = 16,
  ...props
}: MultiSpotlightCardProps) {
  const containerRef = React.useRef<HTMLDivElement>(null)
  const [position, setPosition] = React.useState({ x: 0, y: 0 })
  const [isHovered, setIsHovered] = React.useState(false)

  const handleMouseMove = React.useCallback(
    (e: React.MouseEvent<HTMLDivElement>) => {
      if (!containerRef.current) return

      const rect = containerRef.current.getBoundingClientRect()
      setPosition({
        x: e.clientX - rect.left,
        y: e.clientY - rect.top,
      })
    },
    []
  )

  return (
    <div
      ref={containerRef}
      onMouseMove={handleMouseMove}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
      className={cn(
        "relative overflow-hidden",
        "bg-white dark:bg-neutral-950",
        "border border-neutral-200 dark:border-neutral-800",
        "transition-all duration-500",
        className
      )}
      style={{ borderRadius: `${borderRadius}px` }}
      {...props}
    >
      {/* Multiple spotlight layers */}
      {colors.map((color, index) => (
        <div
          key={index}
          className="absolute pointer-events-none transition-opacity duration-500"
          style={{
            left: position.x + (index - 1) * 50,
            top: position.y + (index - 1) * 50,
            width: "300px",
            height: "300px",
            transform: "translate(-50%, -50%)",
            background: `radial-gradient(circle, ${color} 0%, transparent 70%)`,
            opacity: isHovered ? 1 : 0,
            filter: "blur(40px)",
          }}
        />
      ))}

      {/* Content */}
      <div className="relative z-10">{children}</div>
    </div>
  )
}

// Beam spotlight effect - creates a beam of light that follows cursor
interface BeamSpotlightCardProps extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode
  beamColor?: string
  beamWidth?: number
  borderRadius?: number
}

function BeamSpotlightCard({
  children,
  className,
  beamColor = "rgba(120, 119, 198, 0.5)",
  beamWidth = 200,
  borderRadius = 16,
  ...props
}: BeamSpotlightCardProps) {
  const containerRef = React.useRef<HTMLDivElement>(null)
  const [position, setPosition] = React.useState({ x: 0, y: 0 })
  const [isHovered, setIsHovered] = React.useState(false)

  const handleMouseMove = React.useCallback(
    (e: React.MouseEvent<HTMLDivElement>) => {
      if (!containerRef.current) return

      const rect = containerRef.current.getBoundingClientRect()
      setPosition({
        x: e.clientX - rect.left,
        y: e.clientY - rect.top,
      })
    },
    []
  )

  return (
    <div
      ref={containerRef}
      onMouseMove={handleMouseMove}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
      className={cn(
        "relative overflow-hidden",
        "bg-white dark:bg-neutral-950",
        "border border-neutral-200 dark:border-neutral-800",
        "transition-all duration-500",
        className
      )}
      style={{ borderRadius: `${borderRadius}px` }}
      {...props}
    >
      {/* Vertical beam */}
      <div
        className="absolute pointer-events-none transition-all duration-150"
        style={{
          left: position.x - beamWidth / 2,
          top: 0,
          width: `${beamWidth}px`,
          height: "100%",
          background: `linear-gradient(90deg, transparent, ${beamColor}, transparent)`,
          opacity: isHovered ? 0.6 : 0,
          filter: "blur(20px)",
        }}
      />

      {/* Horizontal beam */}
      <div
        className="absolute pointer-events-none transition-all duration-150"
        style={{
          left: 0,
          top: position.y - beamWidth / 2,
          width: "100%",
          height: `${beamWidth}px`,
          background: `linear-gradient(180deg, transparent, ${beamColor}, transparent)`,
          opacity: isHovered ? 0.4 : 0,
          filter: "blur(20px)",
        }}
      />

      {/* Intersection glow */}
      <div
        className="absolute pointer-events-none transition-all duration-150"
        style={{
          left: position.x,
          top: position.y,
          width: "100px",
          height: "100px",
          transform: "translate(-50%, -50%)",
          background: `radial-gradient(circle, ${beamColor} 0%, transparent 70%)`,
          opacity: isHovered ? 1 : 0,
        }}
      />

      {/* Content */}
      <div className="relative z-10">{children}</div>
    </div>
  )
}

// Gradient follow card - the background gradient follows the cursor
interface GradientFollowCardProps extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode
  gradientColors?: [string, string, string]
  borderRadius?: number
}

function GradientFollowCard({
  children,
  className,
  gradientColors = ["#7877c6", "#5eead4", "#f472b6"],
  borderRadius = 16,
  ...props
}: GradientFollowCardProps) {
  const containerRef = React.useRef<HTMLDivElement>(null)
  const [position, setPosition] = React.useState({ x: 50, y: 50 })
  const [isHovered, setIsHovered] = React.useState(false)

  const handleMouseMove = React.useCallback(
    (e: React.MouseEvent<HTMLDivElement>) => {
      if (!containerRef.current) return

      const rect = containerRef.current.getBoundingClientRect()
      const x = ((e.clientX - rect.left) / rect.width) * 100
      const y = ((e.clientY - rect.top) / rect.height) * 100
      setPosition({ x, y })
    },
    []
  )

  return (
    <div
      ref={containerRef}
      onMouseMove={handleMouseMove}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
      className={cn(
        "relative overflow-hidden",
        "transition-all duration-500",
        className
      )}
      style={{ borderRadius: `${borderRadius}px` }}
      {...props}
    >
      {/* Animated gradient background */}
      <div
        className="absolute inset-0 transition-opacity duration-500"
        style={{
          background: `
            radial-gradient(
              600px circle at ${position.x}% ${position.y}%,
              ${gradientColors[0]}40,
              transparent 40%
            ),
            radial-gradient(
              400px circle at ${position.x + 10}% ${position.y - 10}%,
              ${gradientColors[1]}30,
              transparent 40%
            ),
            radial-gradient(
              300px circle at ${position.x - 10}% ${position.y + 10}%,
              ${gradientColors[2]}20,
              transparent 40%
            )
          `,
          opacity: isHovered ? 1 : 0.3,
        }}
      />

      {/* Base background */}
      <div
        className="absolute inset-0 bg-white/90 dark:bg-neutral-950/90"
        style={{ borderRadius: `${borderRadius}px` }}
      />

      {/* Border */}
      <div
        className="absolute inset-0 pointer-events-none transition-opacity duration-500"
        style={{
          borderRadius: `${borderRadius}px`,
          border: "1px solid",
          borderColor: isHovered
            ? "rgba(255, 255, 255, 0.2)"
            : "rgba(255, 255, 255, 0.1)",
        }}
      />

      {/* Content */}
      <div className="relative z-10">{children}</div>
    </div>
  )
}

// Tilt card with 3D perspective
interface TiltSpotlightCardProps extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode
  maxTilt?: number
  perspective?: number
  scale?: number
  borderRadius?: number
  glareOpacity?: number
  spotlightColor?: string
}

function TiltSpotlightCard({
  children,
  className,
  maxTilt = 10,
  perspective = 1000,
  scale = 1.02,
  borderRadius = 16,
  glareOpacity = 0.2,
  spotlightColor = "rgba(120, 119, 198, 0.3)",
  ...props
}: TiltSpotlightCardProps) {
  const containerRef = React.useRef<HTMLDivElement>(null)
  const [transform, setTransform] = React.useState({
    rotateX: 0,
    rotateY: 0,
    scale: 1,
  })
  const [glarePosition, setGlarePosition] = React.useState({ x: 50, y: 50 })
  const [spotlightPosition, setSpotlightPosition] = React.useState({ x: 0, y: 0 })
  const [isHovered, setIsHovered] = React.useState(false)

  const handleMouseMove = React.useCallback(
    (e: React.MouseEvent<HTMLDivElement>) => {
      if (!containerRef.current) return

      const rect = containerRef.current.getBoundingClientRect()
      const centerX = rect.width / 2
      const centerY = rect.height / 2
      const mouseX = e.clientX - rect.left
      const mouseY = e.clientY - rect.top

      const rotateY = ((mouseX - centerX) / centerX) * maxTilt
      const rotateX = -((mouseY - centerY) / centerY) * maxTilt

      setTransform({ rotateX, rotateY, scale })
      setGlarePosition({
        x: (mouseX / rect.width) * 100,
        y: (mouseY / rect.height) * 100,
      })
      setSpotlightPosition({
        x: mouseX,
        y: mouseY,
      })
    },
    [maxTilt, scale]
  )

  const handleMouseLeave = React.useCallback(() => {
    setTransform({ rotateX: 0, rotateY: 0, scale: 1 })
    setIsHovered(false)
  }, [])

  return (
    <div
      ref={containerRef}
      onMouseMove={handleMouseMove}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={handleMouseLeave}
      className={cn(
        "relative overflow-hidden",
        "bg-white dark:bg-neutral-950",
        "border border-neutral-200 dark:border-neutral-800",
        "transition-[border-color] duration-500",
        isHovered && "border-neutral-300 dark:border-neutral-700",
        className
      )}
      style={{
        borderRadius: `${borderRadius}px`,
        perspective: `${perspective}px`,
        transform: `
          perspective(${perspective}px)
          rotateX(${transform.rotateX}deg)
          rotateY(${transform.rotateY}deg)
          scale(${transform.scale})
        `,
        transition: isHovered
          ? "transform 0.1s ease-out"
          : "transform 0.5s ease-out",
      }}
      {...props}
    >
      {/* Glare effect */}
      <div
        className="absolute inset-0 pointer-events-none transition-opacity duration-300"
        style={{
          background: `
            radial-gradient(
              circle at ${glarePosition.x}% ${glarePosition.y}%,
              rgba(255, 255, 255, ${glareOpacity}) 0%,
              transparent 50%
            )
          `,
          opacity: isHovered ? 1 : 0,
        }}
      />

      {/* Spotlight effect */}
      <div
        className="absolute pointer-events-none transition-opacity duration-300"
        style={{
          left: spotlightPosition.x,
          top: spotlightPosition.y,
          width: "400px",
          height: "400px",
          transform: "translate(-50%, -50%)",
          background: `radial-gradient(circle, ${spotlightColor} 0%, transparent 70%)`,
          opacity: isHovered ? 0.4 : 0,
          filter: "blur(20px)",
        }}
      />

      {/* Content */}
      <div className="relative z-10">{children}</div>
    </div>
  )
}

export {
  SpotlightCard,
  SpotlightCardContent,
  SpotlightCardHeader,
  SpotlightCardTitle,
  SpotlightCardDescription,
  MultiSpotlightCard,
  BeamSpotlightCard,
  GradientFollowCard,
  TiltSpotlightCard,
}
```
---

## Sticky Scroll Cards <a name="sticky-scroll-cards"></a>

Component for sticky-scroll-cards

- **Registry Key**: `sticky-scroll-cards`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/sticky-scroll-cards.json
```

#### Dependencies
- **NPM Packages**:
  - `framer-motion`
  - `lenis`

#### How to Use
```tsx
"use client";

import * as React from "react";
import { cn } from "@/lib/utils";

interface StickyScrollCardsPreviewProps {
  src: string;
  title: string;
  className?: string;
}

export function StickyScrollCardsPreview({
  src,
  title,
  className,
}: StickyScrollCardsPreviewProps) {
  return (
    <div className={cn("relative w-full h-full overflow-hidden", className)}>
      <iframe src={src} className="w-full h-full border-0" title={title} />
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/sticky-scroll-cards.tsx`
```tsx
"use client";

import { cn } from "@/lib/utils";
import { motion, useScroll, useTransform } from "framer-motion";
import ReactLenis from "lenis/react";
import { useEffect, useRef } from "react";

export interface StickyScrollCardItem {
  title: string;
  src: string;
}

const DEFAULT_CARDS: StickyScrollCardItem[] = [
  {
    title: "Misty Alps",
    src: "https://images.unsplash.com/photo-1476514525535-07fb3b4ae5f1?w=1200&q=85",
  },
  {
    title: "Sunlit Grove",
    src: "https://images.unsplash.com/photo-1469474968028-56623f02e42e?w=1200&q=85",
  },
  {
    title: "Turquoise Shore",
    src: "https://images.unsplash.com/photo-1507525428034-b723cf961d3e?w=1200&q=85",
  },
  {
    title: "Mountain Pass",
    src: "https://images.unsplash.com/photo-1482938289607-e9573fc25ebb?w=1200&q=85",
  },
  {
    title: "Rolling Hills",
    src: "https://images.unsplash.com/photo-1501854140801-50d01698950b?w=1200&q=85",
  },
];

// Very subtle tilts — natural scatter without looking messy
const CARD_ROTATIONS = [-1.4, 1.0, -0.8, 1.6, -1.1];

interface StickyScrollCardProps {
  i: number;
  title: string;
  src: string;
  progress: ReturnType<typeof useScroll>["scrollYProgress"];
  range: [number, number];
  targetScale: number;
}

function StickyScrollCard({
  i,
  title,
  src,
  progress,
  range,
  targetScale,
}: StickyScrollCardProps) {
  const scale = useTransform(progress, range, [1, targetScale]);
  const rotation = CARD_ROTATIONS[i % CARD_ROTATIONS.length];

  return (
    <div className="sticky top-0 flex h-screen items-center justify-center">
      <motion.div
        style={{
          scale,
          rotate: rotation,
          top: `calc(-5vh + ${i * 22 + 160}px)`,
          borderRadius: 4,
          boxShadow:
            "0 1px 2px rgba(0,0,0,0.04), 0 4px 12px rgba(0,0,0,0.07), 0 12px 32px rgba(0,0,0,0.10), 0 24px 56px rgba(0,0,0,0.08)",
        }}
        className="relative -top-1/4 origin-top overflow-hidden bg-white"
      >
        {/* 10px border on three sides */}
        <div className="p-[10px] pb-0">
          <div className="w-[460px] overflow-hidden">
            <img
              src={src}
              alt={title}
              className="block h-[290px] w-full object-cover"
              draggable={false}
            />
          </div>
        </div>

        {/* Caption strip — trimmed height */}
        <div className="flex h-[44px] items-center justify-center px-4">
          <p className="text-[11px] font-medium uppercase tracking-[0.18em] text-neutral-400">
            {title}
          </p>
        </div>
      </motion.div>
    </div>
  );
}

interface StickyScrollCardsProps {
  /** Array of card items, each with a title and image src URL */
  cards?: StickyScrollCardItem[];
  /** Hint label shown above the stack */
  hint?: string;
  /** Additional CSS classes for the outer container */
  className?: string;
}

export function StickyScrollCards({
  cards = DEFAULT_CARDS,
  hint = "scroll to explore",
  className,
}: StickyScrollCardsProps) {
  const container = useRef<HTMLDivElement>(null);
  const { scrollYProgress } = useScroll({
    target: container,
    offset: ["start start", "end end"],
  });

  // Hide the native scrollbar while this component is mounted
  useEffect(() => {
    const style = document.createElement("style");
    style.id = "__sticky-scroll-cards-no-bar";
    style.textContent =
      "html { scrollbar-width: none; -ms-overflow-style: none; } html::-webkit-scrollbar { display: none; }";
    document.head.appendChild(style);
    return () => {
      document.getElementById("__sticky-scroll-cards-no-bar")?.remove();
    };
  }, []);

  return (
    <ReactLenis root>
      <main
        ref={container}
        className={cn(
          "relative flex w-full flex-col items-center justify-center pb-[100vh] pt-[50vh]",
          className
        )}
      >
        {/* Hint label */}
        <div className="absolute left-1/2 top-[8%] flex -translate-x-1/2 flex-col items-center gap-3">
          <p className="text-[10px] font-medium uppercase tracking-[0.2em] opacity-30">
            {hint}
          </p>
          <span className="h-12 w-px bg-gradient-to-b from-foreground/30 to-transparent" />
        </div>

        {cards.map((card, i) => {
          const targetScale = Math.max(0.5, 1 - (cards.length - i - 1) * 0.1);
          return (
            <StickyScrollCard
              key={`card_${i}`}
              i={i}
              {...card}
              progress={scrollYProgress}
              range={[i * 0.25, 1]}
              targetScale={targetScale}
            />
          );
        })}
      </main>
    </ReactLenis>
  );
}
```
---

## Testimonial Marquee <a name="testimonial-marquee"></a>

A smooth, infinite scrolling marquee component for showcasing social proof and testimonials.

- **Registry Key**: `testimonial-marquee`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/testimonial-marquee.json
```

#### Dependencies
- **NPM Packages**:
  - `framer-motion`

#### Component Source Code
##### File: `components/ui/testimonial-marquee.tsx`
```tsx
"use client"

import * as React from "react"
import { cn } from "@workspace/ui/lib/utils"

export interface Testimonial {
    name: string
    text: string
    avatar: string
    role?: string
    username?: string
    profileLink?: string
}

export interface TestimonialMarqueeProps {
    items: Testimonial[]
    variant?: "default" | "stacked" | "dual" | "flush" | "flush-dual"
    className?: string
    speed?: number
    containerClassName?: string
}


const MarqueeStyles = React.memo(() => (
    <style>
        {`
        @keyframes marquee-left {
          from { transform: translate3d(0, 0, 0); }
          to { transform: translate3d(-100%, 0, 0); }
        }
        @keyframes marquee-right {
          from { transform: translate3d(-100%, 0, 0); }
          to { transform: translate3d(0, 0, 0); }
        }
        .animate-marquee-left {
           animation: marquee-left var(--duration) linear infinite;
        }
        .animate-marquee-right {
           animation: marquee-right var(--duration) linear infinite;
        }
        `}
    </style>
))
MarqueeStyles.displayName = "MarqueeStyles"

const MarqueeRow = React.memo(({
    children,
    direction = "left",
    speed = 40,
    className,
    pauseOnHover = true
}: {
    children: React.ReactNode,
    direction?: "left" | "right"
    speed?: number,
    className?: string,
    pauseOnHover?: boolean
}) => {
    return (
        <div className={cn("group flex overflow-hidden p-2 [--gap:1rem]", className)}>
            <div
                className={cn("flex shrink-0 justify-start [gap:var(--gap)] min-w-full pr-[var(--gap)] will-change-transform [backface-visibility:hidden]",
                    direction === "left" ? "animate-marquee-left" : "animate-marquee-right",
                    pauseOnHover && "group-hover:[animation-play-state:paused]"
                )}
                style={{
                    "--duration": `${speed}s`,
                } as React.CSSProperties}
            >
                {children}
            </div>
            <div
                aria-hidden="true"
                className={cn("flex shrink-0 justify-start [gap:var(--gap)] min-w-full pr-[var(--gap)] will-change-transform [backface-visibility:hidden]",
                    direction === "left" ? "animate-marquee-left" : "animate-marquee-right",
                    pauseOnHover && "group-hover:[animation-play-state:paused]"
                )}
                style={{
                    "--duration": `${speed}s`,
                } as React.CSSProperties}
            >
                {children}
            </div>
        </div>
    )
})
MarqueeRow.displayName = "MarqueeRow"


const TestimonialCard = React.memo(({ item, variant = "default" }: { item: Testimonial, variant?: "default" | "flush" }) => {
    if (variant === "flush") {
        return (
            <div className="relative group flex h-auto w-[350px] shrink-0 flex-col justify-between overflow-hidden rounded-none border-r border-border bg-black/5 dark:bg-white/5 p-6 transition-all hover:bg-black/10 dark:hover:bg-white/10 transform-gpu [backface-visibility:hidden]">
                <div className="absolute inset-0 bg-gradient-to-br from-black/5 dark:from-white/5 to-transparent opacity-0 transition-opacity group-hover:opacity-100" />

                <div className="relative z-10 flex flex-col gap-4">
                    <p className="text-sm leading-relaxed text-muted-foreground line-clamp-4">
                        &quot;{item.text}&quot;
                    </p>

                    <div className="flex items-center gap-3 pt-2">
                        <div className="h-10 w-10 shrink-0 overflow-hidden rounded-full border border-border">
                            <img src={item.avatar} alt={item.name} className="h-full w-full object-cover" loading="eager" />
                        </div>
                        <div className="flex flex-col">
                            <span className="text-sm font-medium text-foreground">{item.name}</span>
                            {item.username && (
                                <span className="text-xs text-muted-foreground">@{item.username}</span>
                            )}
                        </div>
                    </div>
                </div>
            </div>
        )
    }

    return (
        <div className="relative group flex h-auto w-[350px] shrink-0 flex-col justify-between overflow-hidden rounded-2xl border border-border bg-black/5 dark:bg-white/5 p-6 transition-all hover:bg-black/10 dark:hover:bg-white/10 hover:shadow-xl hover:shadow-black/5 hover:-translate-y-1 transform-gpu [backface-visibility:hidden]">
            <div className="absolute inset-0 bg-gradient-to-br from-black/5 dark:from-white/5 to-transparent opacity-0 transition-opacity group-hover:opacity-100" />

            <div className="relative z-10 flex flex-col gap-4">
                <p className="text-sm leading-relaxed text-muted-foreground line-clamp-4">
                    &quot;{item.text}&quot;
                </p>

                <div className="flex items-center gap-3 pt-2">
                    <div className="h-10 w-10 shrink-0 overflow-hidden rounded-full border border-border">
                        <img src={item.avatar} alt={item.name} className="h-full w-full object-cover" loading="eager" />
                    </div>
                    <div className="flex flex-col">
                        <span className="text-sm font-medium text-foreground">{item.name}</span>
                        {item.username && (
                            <span className="text-xs text-muted-foreground">@{item.username}</span>
                        )}
                    </div>
                </div>
            </div>
        </div>
    )
})
TestimonialCard.displayName = "TestimonialCard"

export function TestimonialMarquee({ items, variant = "default", className, speed = 30, containerClassName }: TestimonialMarqueeProps) {
    // Combine custom className with container styling if needed
    const cnContainer = cn(containerClassName, className)

    const itemsToDisplay = React.useMemo(() => {
        let result = [...items]
        // Ensure we have enough items to fill the width for smooth animation
        // 10 items is a safe heuristic for most screen sizes with 350px cards
        while (result.length < 10) {
            result = [...result, ...items]
        }
        return result
    }, [items])

    return (
        <React.Fragment>
            <MarqueeStyles />
            {variant === "dual" ? (
                <div className={cn("flex flex-col gap-4 py-8 overflow-hidden", containerClassName)}>
                    <MarqueeRow speed={speed} direction="left">
                        {itemsToDisplay.slice(0, Math.ceil(itemsToDisplay.length / 2)).map((item, i) => <TestimonialCard key={`row1-${i}`} item={item} />)}
                    </MarqueeRow>
                    <MarqueeRow speed={speed} direction="right">
                        {itemsToDisplay.slice(Math.ceil(itemsToDisplay.length / 2)).map((item, i) => <TestimonialCard key={`row2-${i}`} item={item} />)}
                    </MarqueeRow>
                </div>
            ) : variant === "stacked" ? (
                <div className={cn("flex flex-col gap-2 py-8 overflow-hidden h-[600px] justify-center rotate-[-2deg] scale-110", containerClassName)}>
                    <div className="absolute inset-0 z-10 bg-gradient-to-r from-background via-transparent to-background pointer-events-none" />
                    <MarqueeRow speed={speed * 1.5} direction="left" className="[--gap:0.75rem]">
                        {itemsToDisplay.slice(0, Math.ceil(itemsToDisplay.length / 3)).map((item, i) => <TestimonialCard key={`s-row1-${i}`} item={item} />)}
                    </MarqueeRow>
                    <MarqueeRow speed={speed * 1.2} direction="right" className="[--gap:0.75rem]">
                        {itemsToDisplay.slice(Math.ceil(itemsToDisplay.length / 3), Math.ceil(itemsToDisplay.length / 3) * 2).map((item, i) => <TestimonialCard key={`s-row2-${i}`} item={item} />)}
                    </MarqueeRow>
                    <MarqueeRow speed={speed * 1.5} direction="left" className="[--gap:0.75rem]">
                        {itemsToDisplay.slice(Math.ceil(itemsToDisplay.length / 3) * 2).map((item, i) => <TestimonialCard key={`s-row3-${i}`} item={item} />)}
                    </MarqueeRow>
                </div>
            ) : variant === "flush" ? (
                <div className={cn("overflow-hidden border-y border-border bg-background relative", cnContainer)}>
                    <MarqueeRow speed={speed} direction="left" className="[--gap:0rem] p-0">
                        {itemsToDisplay.map((item, i) => <TestimonialCard key={`flush-${i}`} item={item} variant="flush" />)}
                    </MarqueeRow>
                    <div className="pointer-events-none absolute inset-y-0 left-0 w-1/3 bg-gradient-to-r from-background to-transparent"></div>
                    <div className="pointer-events-none absolute inset-y-0 right-0 w-1/3 bg-gradient-to-l from-background to-transparent"></div>
                </div>
            ) : variant === "flush-dual" ? (
                <div className={cn("flex flex-col overflow-hidden border-y border-border bg-background relative", containerClassName)}>
                    <MarqueeRow speed={speed} direction="left" className="[--gap:0rem] p-0 border-b border-border">
                        {itemsToDisplay.slice(0, Math.ceil(itemsToDisplay.length / 2)).map((item, i) => <TestimonialCard key={`fd-row1-${i}`} item={item} variant="flush" />)}
                    </MarqueeRow>
                    <MarqueeRow speed={speed} direction="right" className="[--gap:0rem] p-0">
                        {itemsToDisplay.slice(Math.ceil(itemsToDisplay.length / 2)).map((item, i) => <TestimonialCard key={`fd-row2-${i}`} item={item} variant="flush" />)}
                    </MarqueeRow>
                    <div className="pointer-events-none absolute inset-y-0 left-0 w-1/3 bg-gradient-to-r from-background to-transparent z-10"></div>
                    <div className="pointer-events-none absolute inset-y-0 right-0 w-1/3 bg-gradient-to-l from-background to-transparent z-10"></div>
                </div>
            ) : (
                <div className={cn("py-8 overflow-hidden", cnContainer)}>
                    <MarqueeRow speed={speed} direction="left">
                        {itemsToDisplay.map((item, i) => <TestimonialCard key={`default-${i}`} item={item} />)}
                    </MarqueeRow>
                </div>
            )}
        </React.Fragment>
    )
}
```
---

## Text Animate <a name="text-animate"></a>

- **Registry Key**: `text-animate`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/text-animate.json
```

#### Dependencies
- **NPM Packages**:
  - `framer-motion`

#### How to Use
```tsx
"use client"

import React, { useState } from "react"
import { TextAnimate } from "@workspace/ui/components/text-animate"
import { RotateCcw } from "lucide-react"

export function TextAnimatePreview({ children, ...props }: React.ComponentProps<typeof TextAnimate>) {
    const [key, setKey] = useState(0)

    return (
        <div className="flex w-full items-center justify-center">
            <button
                onClick={() => setKey((prev) => prev + 1)}
                className="absolute top-4 right-4 z-10 p-2 text-muted-foreground hover:text-foreground transition-all cursor-pointer bg-background/50 rounded-md border border-border hover:bg-background"
                aria-label="Reload animation"
            >
                <RotateCcw className="w-4 h-4" />
            </button>
            <TextAnimate key={key} {...props}>
                {children}
            </TextAnimate>
        </div>
    )
}
```
#### Component Source Code
##### File: `components/ui/text-animate.tsx`
```tsx
"use client"

import { cn } from "@/lib/utils"
import { AnimatePresence, motion, useInView, MotionProps, Variants } from "framer-motion"
import { ElementType, useRef } from "react"

type AnimationType =
  | "fadeIn"
  | "blurIn"
  | "blurInUp"
  | "blurInDown"
  | "slideUp"
  | "slideDown"
  | "slideLeft"
  | "slideRight"
  | "scaleUp"
  | "scaleDown"

interface TextAnimateProps extends MotionProps {
  /**
   * The text to animate
   */
  children: string
  /**
   * The class name for the wrapper element
   */
  className?: string
  /**
   * The class name for the segmented elements (words or characters)
   */
  segmentClassName?: string
  /**
   * The base component to use for the wrapper
   */
  as?: ElementType
  /**
   * The base delay for the animation
   */
  delay?: number
  /**
   * The duration of the animation per item
   */
  duration?: number
  /**
   * The type of animation to perform
   */
  animation?: AnimationType
  /**
   * How to split the text
   */
  by?: "text" | "word" | "character"
  /**
   * Whether to start the animation when the element comes into view
   */
  startOnView?: boolean
  /**
   * Whether to run the animation only once
   */
  once?: boolean
}

export function TextAnimate({
  children,
  delay = 0,
  duration = 0.3,
  className,
  segmentClassName,
  as: Component = "p",
  startOnView = true,
  once = true,
  by = "word",
  animation = "fadeIn",
  ...props
}: TextAnimateProps) {
  const ref = useRef(null)
  const isInView = useInView(ref, { once })

  const segments =
    by === "character"
      ? children.split("")
      : by === "word"
      ? children.split(" ")
      : [children]

  const containerVariants: Variants = {
    hidden: { opacity: 0 },
    show: {
      opacity: 1,
      transition: {
        staggerChildren: 0.1,
        delayChildren: delay,
      },
    },
    exit: {
      opacity: 0,
      transition: {
        staggerChildren: 0.05,
        staggerDirection: -1,
      },
    },
  }

  const itemVariants: Record<
    AnimationType,
    Variants
  > = {
    fadeIn: {
      hidden: { opacity: 0 },
      show: {
        opacity: 1,
        transition: { duration },
      },
    },
    blurIn: {
      hidden: { opacity: 0, filter: "blur(10px)" },
      show: {
        opacity: 1,
        filter: "blur(0px)",
        transition: { duration },
      },
    },
    blurInUp: {
      hidden: { opacity: 0, filter: "blur(10px)", y: 20 },
      show: {
        opacity: 1,
        filter: "blur(0px)",
        y: 0,
        transition: { duration },
      },
    },
    blurInDown: {
      hidden: { opacity: 0, filter: "blur(10px)", y: -20 },
      show: {
        opacity: 1,
        filter: "blur(0px)",
        y: 0,
        transition: { duration },
      },
    },
    slideUp: {
      hidden: { y: 20, opacity: 0 },
      show: {
        y: 0,
        opacity: 1,
        transition: { duration },
      },
    },
    slideDown: {
      hidden: { y: -20, opacity: 0 },
      show: {
        y: 0,
        opacity: 1,
        transition: { duration },
      },
    },
    slideLeft: {
      hidden: { x: 20, opacity: 0 },
      show: {
        x: 0,
        opacity: 1,
        transition: { duration },
      },
    },
    slideRight: {
      hidden: { x: -20, opacity: 0 },
      show: {
        x: 0,
        opacity: 1,
        transition: { duration },
      },
    },
    scaleUp: {
      hidden: { scale: 0.5, opacity: 0 },
      show: {
        scale: 1,
        opacity: 1,
        transition: { duration },
      },
    },
    scaleDown: {
      hidden: { scale: 1.5, opacity: 0 },
      show: {
        scale: 1,
        opacity: 1,
        transition: { duration },
      },
    },
  }

  const finalVariants = itemVariants[animation]

  // Use the 'as' prop to dynmically render the motion component
  const MotionComponent = motion.create(Component)

  return (
    <AnimatePresence mode="popLayout">
        <MotionComponent
          ref={ref}
          className={cn("whitespace-pre-wrap", className)}
          initial="hidden"
          animate={startOnView ? (isInView ? "show" : "hidden") : "show"}
          exit="exit"
          variants={containerVariants}
          {...props}
        >
          {segments.map((segment, i) => (
            <motion.span
              key={`${by}-${i}-${segment}`}
              className={cn("inline-block", segmentClassName)}
              variants={finalVariants}
            >
              {segment}
              {by === "word" && i < segments.length - 1 && (
                <span className="inline-block">&nbsp;</span>
              )}
            </motion.span>
          ))}
        </MotionComponent>
    </AnimatePresence>
  )
}
```
---

## Text Repel <a name="text-repel"></a>

Component for text-repel

- **Registry Key**: `text-repel`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/text-repel.json
```

#### Component Source Code
##### File: `components/ui/text-repel.tsx`
```tsx
"use client";

import { cn } from "@workspace/ui/lib/utils";
import {
    motion,
    useMotionValue,
    useSpring,
    useTransform,
    type MotionValue,
} from "framer-motion";
import { useEffect, useRef } from "react";

interface TextRepelProps {
    /** The text to display */
    text: string;
    /** Additional CSS classes for the container */
    className?: string;
    /** CSS classes applied to each letter */
    letterClassName?: string;
    /** Cursor influence radius in pixels */
    radius?: number;
    /** Maximum displacement strength in pixels */
    strength?: number;
    /** Interaction mode — push letters away or pull them toward the cursor */
    mode?: "repel" | "attract";
    /** Spring stiffness — higher = snappier return */
    stiffness?: number;
    /** Spring damping — lower = bouncier return */
    damping?: number;
    /** Spring mass — higher = heavier feel */
    mass?: number;
}

function RepelLetter({
    letter,
    mouseX,
    mouseY,
    radius,
    strength,
    mode,
    stiffness,
    damping,
    mass,
    className,
}: {
    letter: string;
    mouseX: MotionValue<number>;
    mouseY: MotionValue<number>;
    radius: number;
    strength: number;
    mode: "repel" | "attract";
    stiffness: number;
    damping: number;
    mass: number;
    className?: string;
}) {
    const ref = useRef<HTMLSpanElement>(null);
    const originX = useRef(0);
    const originY = useRef(0);

    const x = useMotionValue(0);
    const y = useMotionValue(0);
    const springX = useSpring(x, { stiffness, damping, mass });
    const springY = useSpring(y, { stiffness, damping, mass });

    // Subtle tilt proportional to horizontal displacement
    const rotate = useTransform(springX, (v) => v * 0.3);

    // Capture original position relative to the container
    useEffect(() => {
        const capture = () => {
            if (!ref.current) return;
            const container = ref.current.closest("[data-text-repel]");
            if (!container) return;
            const cr = container.getBoundingClientRect();
            const lr = ref.current.getBoundingClientRect();
            originX.current = lr.left - cr.left + lr.width / 2;
            originY.current = lr.top - cr.top + lr.height / 2;
        };

        const raf = requestAnimationFrame(capture);
        window.addEventListener("resize", capture);
        return () => {
            cancelAnimationFrame(raf);
            window.removeEventListener("resize", capture);
        };
    }, []);

    // React to cursor position changes via motion value subscriptions
    useEffect(() => {
        const update = () => {
            const mx = mouseX.get();
            const my = mouseY.get();
            const dx = originX.current - mx;
            const dy = originY.current - my;
            const distance = Math.sqrt(dx * dx + dy * dy);

            if (distance < radius && distance > 0) {
                // Quadratic falloff for natural-feeling force
                const force = ((1 - distance / radius) ** 2) * strength;
                const angle = Math.atan2(dy, dx);
                const dir = mode === "attract" ? -1 : 1;
                x.set(Math.cos(angle) * force * dir);
                y.set(Math.sin(angle) * force * dir);
            } else {
                x.set(0);
                y.set(0);
            }
        };

        const unsub1 = mouseX.on("change", update);
        const unsub2 = mouseY.on("change", update);
        return () => {
            unsub1();
            unsub2();
        };
    }, [mouseX, mouseY, radius, strength, mode, x, y]);

    if (letter === " ") {
        return <span className="inline-block whitespace-pre"> </span>;
    }

    return (
        <motion.span
            ref={ref}
            className={cn(
                "inline-block whitespace-pre will-change-transform",
                className
            )}
            style={{ x: springX, y: springY, rotate }}
            aria-hidden
        >
            {letter}
        </motion.span>
    );
}

export function TextRepel({
    text,
    className,
    letterClassName,
    radius = 120,
    strength = 45,
    mode = "repel",
    stiffness = 180,
    damping = 14,
    mass = 0.4,
}: TextRepelProps) {
    const containerRef = useRef<HTMLDivElement>(null);
    const mouseX = useMotionValue(-9999);
    const mouseY = useMotionValue(-9999);

    return (
        <div
            ref={containerRef}
            data-text-repel
            className={cn(
                "inline-flex flex-wrap items-center justify-center cursor-default select-none",
                className
            )}
            onMouseMove={(e) => {
                const rect = containerRef.current?.getBoundingClientRect();
                if (!rect) return;
                mouseX.set(e.clientX - rect.left);
                mouseY.set(e.clientY - rect.top);
            }}
            onMouseLeave={() => {
                mouseX.set(-9999);
                mouseY.set(-9999);
            }}
            aria-label={text}
        >
            {text.split("").map((letter, i) => (
                <RepelLetter
                    key={i}
                    letter={letter}
                    mouseX={mouseX}
                    mouseY={mouseY}
                    radius={radius}
                    strength={strength}
                    mode={mode}
                    stiffness={stiffness}
                    damping={damping}
                    mass={mass}
                    className={letterClassName}
                />
            ))}
        </div>
    );
}
```
---

## Webgl Liquid <a name="webgl-liquid"></a>

Premium WebGL liquid hero background with customizable flow, palette, reveal, and grain.

- **Registry Key**: `webgl-liquid`
#### Installation
```bash
npx shadcn@latest add https://www.componentry.fun/r/webgl-liquid.json
```

#### Dependencies
- **NPM Packages**:
  - `clsx`
  - `tailwind-merge`

#### How to Use
```tsx
"use client";

import { useEffect, useMemo, useState } from "react";
import WebGLLiquid from "@workspace/ui/components/webgl-liquid";
import { cn } from "@/lib/utils";
import {
  WEBGL_LIQUID_DEFAULT_CONFIG,
  type WebGLLiquidConfig,
  usePlaygroundStore,
} from "@/hooks/use-playground-store";

const PRESETS: Array<{ name: string; config: WebGLLiquidConfig }> = [
  {
    name: "Default",
    config: WEBGL_LIQUID_DEFAULT_CONFIG,
  },
  {
    name: "Midnight",
    config: {
      ...WEBGL_LIQUID_DEFAULT_CONFIG,
      title: "Midnight Drift",
      subtitle: "Depth by Design",
      colorDeep: "#02040b",
      colorMid: "#11315e",
      colorHighlight: "#67d5ff",
      speed: 0.9,
      flowStrength: 0.9,
      contrast: 1.2,
    },
  },
  {
    name: "Aurora",
    config: {
      ...WEBGL_LIQUID_DEFAULT_CONFIG,
      title: "Aurora Pulse",
      subtitle: "Luminous Gradient",
      colorDeep: "#070519",
      colorMid: "#3954d9",
      colorHighlight: "#7dffe5",
      flowStrength: 1.3,
      grain: 0.06,
      contrast: 1.15,
    },
  },
  {
    name: "Rose Gold",
    config: {
      ...WEBGL_LIQUID_DEFAULT_CONFIG,
      title: "Rose Current",
      subtitle: "Luxury Atmosphere",
      colorDeep: "#1a0a12",
      colorMid: "#7f2f5d",
      colorHighlight: "#ffd9b5",
      speed: 0.85,
      flowStrength: 0.75,
      opacity: 0.92,
    },
  },
  {
    name: "Kinetic",
    config: {
      ...WEBGL_LIQUID_DEFAULT_CONFIG,
      title: "Kinetic Wave",
      subtitle: "Momentum Forward",
      colorDeep: "#040506",
      colorMid: "#146684",
      colorHighlight: "#c6ffff",
      speed: 1.55,
      flowStrength: 1.6,
      grain: 0.08,
      contrast: 1.25,
    },
  },
];

const SectionTitle = ({ children }: { children: React.ReactNode }) => (
  <div className="mb-3 text-[10px] font-mono uppercase tracking-widest text-muted-foreground">{children}</div>
);

const Input = ({ className, ...props }: React.InputHTMLAttributes<HTMLInputElement>) => (
  <input
    className={cn(
      "h-10 w-full rounded-md border border-border/70 bg-transparent px-3 text-sm text-foreground placeholder:text-muted-foreground/70 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-zinc-400/25",
      className,
    )}
    {...props}
  />
);

const TextArea = ({ className, ...props }: React.TextareaHTMLAttributes<HTMLTextAreaElement>) => (
  <textarea
    className={cn(
      "min-h-24 w-full resize-none rounded-md border border-border/70 bg-transparent px-3 py-2.5 text-sm text-foreground placeholder:text-muted-foreground/70 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-zinc-400/25",
      className,
    )}
    {...props}
  />
);

const Slider = ({
  value,
  min,
  max,
  step,
  onChange,
  label,
  unit = "",
}: {
  value: number;
  min: number;
  max: number;
  step: number;
  onChange: (val: number) => void;
  label: string;
  unit?: string;
}) => (
  <div className="space-y-2">
    <div className="flex items-center justify-between text-sm">
      <span className="text-foreground/90">{label}</span>
      <span className="font-mono text-muted-foreground">
        {Number(value).toFixed(step < 0.1 ? 2 : 1)}
        {unit}
      </span>
    </div>
    <input
      type="range"
      min={min}
      max={max}
      step={step}
      value={value}
      onChange={(e) => onChange(parseFloat(e.target.value))}
      className="h-2 w-full cursor-pointer appearance-none rounded-full bg-zinc-300/70 dark:bg-zinc-700/70
      [&::-webkit-slider-thumb]:h-4 [&::-webkit-slider-thumb]:w-4 [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:rounded-full [&::-webkit-slider-thumb]:border [&::-webkit-slider-thumb]:border-border [&::-webkit-slider-thumb]:bg-white dark:[&::-webkit-slider-thumb]:bg-zinc-100"
    />
  </div>
);

const Switch = ({
  checked,
  onChange,
  label,
}: {
  checked: boolean;
  onChange: (val: boolean) => void;
  label: string;
}) => (
  <label className="flex items-center justify-between rounded-md border border-border/70 p-2.5">
    <span className="text-sm text-foreground/90">{label}</span>
    <button
      type="button"
      role="switch"
      aria-checked={checked}
      onClick={() => onChange(!checked)}
      className={cn(
        "inline-flex h-6 w-10 items-center rounded-full border border-border transition-colors",
        checked ? "bg-zinc-900 dark:bg-zinc-100" : "bg-zinc-200 dark:bg-zinc-800",
      )}
    >
      <span
        className={cn(
          "mx-[2px] h-5 w-5 rounded-full bg-white transition-transform dark:bg-zinc-900",
          checked ? "translate-x-4" : "translate-x-0.5",
        )}
      />
    </button>
  </label>
);

function normalizeHexColor(value: string) {
  const trimmed = value.trim();
  if (!/^#?[0-9a-fA-F]{6}$/.test(trimmed)) {
    return null;
  }
  return `#${trimmed.replace("#", "").toLowerCase()}`;
}

const ColorPicker = ({
  value,
  onChange,
  label,
}: {
  value: string;
  onChange: (val: string) => void;
  label: string;
}) => {
  const [draft, setDraft] = useState(value);

  useEffect(() => {
    setDraft(value);
  }, [value]);

  return (
    <label className="flex items-center gap-3 rounded-md border border-border/70 p-2.5">
      <span className="relative h-8 w-8 overflow-hidden rounded border border-border">
        <span aria-hidden="true" className="absolute inset-0" style={{ backgroundColor: value }} />
        <input
          type="color"
          value={value}
          onInput={(e) => onChange((e.target as HTMLInputElement).value)}
          onChange={(e) => onChange(e.target.value)}
          aria-label={`${label} color picker`}
          className="absolute inset-0 h-full w-full cursor-pointer opacity-0"
        />
      </span>
      <span className="min-w-0 flex-1">
        <span className="mb-1 block text-xs font-semibold text-foreground/90">{label}</span>
        <input
          value={draft}
          onChange={(e) => {
            const next = e.target.value;
            setDraft(next);
            const normalized = normalizeHexColor(next);
            if (normalized) {
              onChange(normalized);
            }
          }}
          placeholder="#000000"
          className="h-8 w-full rounded border border-border/70 bg-transparent px-2.5 font-mono text-xs text-foreground placeholder:text-muted-foreground/70 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-zinc-400/25"
        />
      </span>
    </label>
  );
};

function generateCode(config: WebGLLiquidConfig) {
  const props = Object.entries(config)
    .filter(([, value]) => value !== undefined && value !== null)
    .map(([key, value]) => {
      if (typeof value === "string") {
        return `${key}="${value}"`;
      }
      if (typeof value === "boolean") {
        return value ? `${key}` : `${key}={false}`;
      }
      return `${key}={${value}}`;
    })
    .join("\n    ");

  return `<WebGLLiquid \n    ${props}\n/>`;
}

export function WebGLLiquidPlayground() {
  const config = usePlaygroundStore((state) => state.webglLiquidConfig);
  const renderVersion = usePlaygroundStore((state) => state.webglLiquidRenderVersion);

  useEffect(() => {
    const code = generateCode(config);
    const timeoutId = setTimeout(() => {
      usePlaygroundStore.getState().setCode(code);
    }, 250);

    return () => clearTimeout(timeoutId);
  }, [config]);

  return (
    <div className="relative h-full w-full bg-[#f3f4f6] dark:bg-[#080808]">
      <div className="relative h-full w-full overflow-hidden rounded-none">
        <WebGLLiquid
          key={renderVersion}
          {...config}
          className="h-full w-full !min-h-full"
          style={{ minHeight: "100%" }}
        />
      </div>
    </div>
  );
}

export function WebGLLiquidPersonalizePanel() {
  const config = usePlaygroundStore((state) => state.webglLiquidConfig);
  const activePreset = usePlaygroundStore((state) => state.activeWebglLiquidPreset);
  const setConfig = usePlaygroundStore((state) => state.setWebglLiquidConfig);
  const setActivePreset = usePlaygroundStore((state) => state.setActiveWebglLiquidPreset);
  const updateConfig = usePlaygroundStore((state) => state.updateWebglLiquidConfig);
  const resetPreview = usePlaygroundStore((state) => state.resetWebglLiquidPreview);
  const resetConfig = usePlaygroundStore((state) => state.resetWebglLiquidConfig);

  const selectedPresetConfig = useMemo(
    () => PRESETS.find((preset) => preset.name === activePreset)?.config,
    [activePreset],
  );

  const handleChange = (key: keyof WebGLLiquidConfig, value: string | number | boolean) => {
    updateConfig({ [key]: value } as Partial<WebGLLiquidConfig>);

    if (activePreset === "Custom") {
      return;
    }

    const matchedPreset = PRESETS.find(
      (preset) => JSON.stringify(preset.config) === JSON.stringify({ ...config, [key]: value }),
    );
    setActivePreset(matchedPreset?.name ?? "Custom");
  };

  const handlePresetChange = (presetName: string) => {
    const preset = PRESETS.find((item) => item.name === presetName);
    if (!preset) {
      return;
    }

    setConfig({ ...preset.config });
    setActivePreset(presetName);
    resetPreview();
  };

  return (
    <div className="h-full overflow-auto bg-[#f3f4f6] dark:bg-[#080808] [&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]">
      <div className="space-y-6 px-4 pb-10 pt-20">
        <header className="space-y-2">
          <h2 className="text-2xl font-bold tracking-tighter text-foreground">Personalize</h2>
          <p className="text-sm leading-relaxed text-muted-foreground/90">
            Craft premium liquid motion by tuning typography, palette, and shader dynamics.
          </p>
        </header>

        <div className="flex items-center justify-between">
          <div className="text-sm text-muted-foreground/90">
            Current preset: <span className="font-mono text-foreground">{activePreset}</span>
          </div>
          <button
            type="button"
            onClick={resetConfig}
            className="rounded-md border border-border/40 bg-white/50 px-3 py-1.5 text-[10px] font-mono uppercase tracking-widest text-muted-foreground transition-colors hover:text-foreground dark:bg-white/[0.03]"
          >
            Reset
          </button>
        </div>

        <div>
          <SectionTitle>Presets</SectionTitle>
          <div className="grid grid-cols-5 gap-1.5">
            {PRESETS.map((preset) => {
              const isActive =
                activePreset === preset.name ||
                (activePreset !== "Custom" && selectedPresetConfig && preset.name === activePreset);

              return (
                <button
                  key={preset.name}
                  type="button"
                  onClick={() => handlePresetChange(preset.name)}
                  className={cn(
                    "rounded-md border p-1.5 text-center transition-colors",
                    isActive
                      ? "border-zinc-500/70 bg-zinc-100 dark:border-zinc-500/80 dark:bg-zinc-900/70"
                      : "border-border/70 bg-transparent",
                  )}
                >
                  <div
                    className="mb-1.5 h-4 rounded-sm border border-white/20"
                    style={{
                      background: `linear-gradient(125deg, ${preset.config.colorDeep} 0%, ${preset.config.colorMid} 55%, ${preset.config.colorHighlight} 100%)`,
                    }}
                  />
                  <span className="block truncate text-[10px] font-mono uppercase tracking-widest text-foreground/90">
                    {preset.name}
                  </span>
                </button>
              );
            })}
          </div>
        </div>

        <div>
          <SectionTitle>Typography</SectionTitle>
          <div className="grid grid-cols-1 gap-2.5">
            <Input
              value={config.title}
              onChange={(e) => handleChange("title", e.target.value)}
              placeholder="Headline line one"
            />
            <Input
              value={config.subtitle}
              onChange={(e) => handleChange("subtitle", e.target.value)}
              placeholder="Headline line two"
            />
            <TextArea
              value={config.description}
              onChange={(e) => handleChange("description", e.target.value)}
              placeholder="Support text"
            />
          </div>
        </div>

        <div>
          <SectionTitle>Palette</SectionTitle>
          <div className="grid grid-cols-3 gap-2.5">
            <ColorPicker label="Deep Base" value={config.colorDeep} onChange={(v) => handleChange("colorDeep", v)} />
            <ColorPicker label="Mid Tone" value={config.colorMid} onChange={(v) => handleChange("colorMid", v)} />
            <ColorPicker
              label="Highlight"
              value={config.colorHighlight}
              onChange={(v) => handleChange("colorHighlight", v)}
            />
          </div>
        </div>

        <div>
          <SectionTitle>Motion</SectionTitle>
          <div className="grid grid-cols-2 gap-x-4 gap-y-3">
            <Slider
              label="Speed"
              min={0.2}
              max={2.5}
              step={0.1}
              value={config.speed}
              onChange={(v) => handleChange("speed", v)}
              unit="x"
            />
            <Slider
              label="Flow Strength"
              min={0.2}
              max={2}
              step={0.1}
              value={config.flowStrength}
              onChange={(v) => handleChange("flowStrength", v)}
            />
            <Slider
              label="Grain"
              min={0}
              max={0.16}
              step={0.01}
              value={config.grain}
              onChange={(v) => handleChange("grain", v)}
            />
            <Slider
              label="Contrast"
              min={0.8}
              max={1.8}
              step={0.05}
              value={config.contrast}
              onChange={(v) => handleChange("contrast", v)}
            />
            <Slider
              label="Opacity"
              min={0.4}
              max={1}
              step={0.01}
              value={config.opacity}
              onChange={(v) => handleChange("opacity", v)}
            />
            <Slider
              label="Delay"
              min={0}
              max={2400}
              step={100}
              value={config.delayMs}
              onChange={(v) => handleChange("delayMs", v)}
              unit="ms"
            />
            <div className="col-span-2">
              <Switch label="Reveal Wipe" checked={config.reveal} onChange={(v) => handleChange("reveal", v)} />
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
```
#### Component Source Code
##### File: `components/ui/webgl-liquid.tsx`
```tsx
"use client";

import { useEffect, useMemo, useRef, useState } from "react";
import { cn } from "@/lib/utils";
import { WebGLErrorBoundary, WebGLFallback } from "./webgl-error-boundary";

const VERTEX_SHADER = `
attribute vec2 position;
void main() {
  gl_Position = vec4(position, 0.0, 1.0);
}
`;

const FRAGMENT_SHADER = `
precision highp float;

uniform vec2 u_res;
uniform float u_time;
uniform vec3 u_colorDeep;
uniform vec3 u_colorMid;
uniform vec3 u_colorHighlight;
uniform float u_speed;
uniform float u_flowStrength;
uniform float u_grain;
uniform float u_contrast;
uniform float u_opacity;
uniform float u_reveal;

float hash(vec2 p) {
  return fract(sin(dot(p, vec2(127.1, 311.7))) * 43758.5453123);
}

float noise(vec2 p) {
  vec2 i = floor(p);
  vec2 f = fract(p);
  float a = hash(i);
  float b = hash(i + vec2(1.0, 0.0));
  float c = hash(i + vec2(0.0, 1.0));
  float d = hash(i + vec2(1.0, 1.0));
  vec2 u = f * f * (3.0 - 2.0 * f);
  return mix(a, b, u.x) + (c - a) * u.y * (1.0 - u.x) + (d - b) * u.x * u.y;
}

float fbm(vec2 p) {
  float v = 0.0;
  float a = 0.5;
  mat2 rot = mat2(0.86, 0.51, -0.51, 0.86);
  for (int i = 0; i < 6; i++) {
    v += a * noise(p);
    p = rot * p * 2.0;
    a *= 0.5;
  }
  return v;
}

vec3 applyContrast(vec3 c, float contrast) {
  return clamp((c - 0.5) * contrast + 0.5, 0.0, 1.0);
}

void main() {
  vec2 uv = gl_FragCoord.xy / u_res;
  float t = u_time * (0.14 * u_speed);
  vec2 aspect = vec2(u_res.x / max(u_res.y, 1.0), 1.0);
  vec2 p = (uv - 0.5) * aspect;

  vec2 flowP = vec2(p.x * 1.1, p.y - t * 0.35);
  float n1 = fbm(flowP * 2.8 + vec2(0.0, t * 0.2));
  float n2 = fbm((flowP + n1 * 0.45) * 4.0 - vec2(0.0, t * 0.35));
  float n3 = fbm((flowP + n2 * 0.4) * 6.5 + vec2(t * 0.15, 0.0));

  float structure = n3 * 1.15 + (n2 - 0.5) * 0.5;
  structure += (n1 - 0.5) * 0.3 * u_flowStrength;

  float lowBand = smoothstep(0.18, 0.6, structure);
  float highBand = smoothstep(0.62, 1.08, structure);
  vec3 col = mix(u_colorDeep, u_colorMid, lowBand);
  col = mix(col, u_colorHighlight, highBand);

  float glow = smoothstep(0.52, 0.95, structure) * (0.35 + 0.5 * u_flowStrength);
  col += glow * u_colorHighlight * 0.35;

  float verticalMask = smoothstep(1.05, 0.05, uv.y);
  verticalMask = pow(verticalMask, 1.1);

  float vignette = smoothstep(1.28, 0.36, length(uv - 0.5));
  col *= mix(0.9, 1.05, vignette);

  col = applyContrast(col, u_contrast);

  float dither = (hash(gl_FragCoord.xy + t * 10.0) - 0.5) * u_grain;
  col += dither;

  float alpha = verticalMask * smoothstep(0.08, 0.95, structure);
  alpha *= smoothstep(0.0, 0.28, u_reveal - uv.x);
  alpha *= u_opacity;

  gl_FragColor = vec4(clamp(col, 0.0, 1.0), clamp(alpha, 0.0, 1.0));
}
`;

const HEX_COLOR_REGEX = /^#?[0-9a-fA-F]{6}$/;
const FALLBACK_DEEP = "#04050b";
const FALLBACK_MID = "#134d93";
const FALLBACK_HIGHLIGHT = "#8cecff";

function sanitizeHexColor(value: string, fallback: string) {
  const trimmed = value.trim();
  if (!HEX_COLOR_REGEX.test(trimmed)) {
    return fallback;
  }
  return trimmed.startsWith("#") ? trimmed : `#${trimmed}`;
}

function hexToRgb01(hex: string): [number, number, number] {
  const normalized = sanitizeHexColor(hex, FALLBACK_DEEP).replace("#", "");
  const r = parseInt(normalized.slice(0, 2), 16) / 255;
  const g = parseInt(normalized.slice(2, 4), 16) / 255;
  const b = parseInt(normalized.slice(4, 6), 16) / 255;
  return [r, g, b];
}

export interface WebGLLiquidProps extends React.HTMLAttributes<HTMLDivElement> {
  title?: string;
  subtitle?: string;
  description?: string;
  colorDeep?: string;
  colorMid?: string;
  colorHighlight?: string;
  speed?: number;
  flowStrength?: number;
  grain?: number;
  contrast?: number;
  opacity?: number;
  reveal?: boolean;
  delayMs?: number;
  revealDuration?: number;
  children?: React.ReactNode;
}

const LIQUID_HEADLINE_CLASS =
  "pb-[0.08em] text-[11cqi] md:text-[7cqi] lg:text-[5.5cqi] leading-[0.92] tracking-[-0.03em] font-semibold text-white";

export function WebGLLiquid({
  title = "Fluid Motion",
  subtitle = "Premium Presence",
  description = "A cinematic liquid field tuned for modern hero sections with polished depth and restrained motion.",
  colorDeep = FALLBACK_DEEP,
  colorMid = FALLBACK_MID,
  colorHighlight = FALLBACK_HIGHLIGHT,
  speed = 1,
  flowStrength = 1,
  grain = 0.05,
  contrast = 1.1,
  opacity = 0.95,
  reveal = true,
  delayMs = 0,
  revealDuration = 1.2,
  className,
  children,
  style,
  ...props
}: WebGLLiquidProps) {
  const canvasRef = useRef<HTMLCanvasElement | null>(null);
  const hostRef = useRef<HTMLDivElement | null>(null);
  const [hasWebGLError, setHasWebGLError] = useState(false);

  const settings = useMemo(
    () => ({
      colorDeep,
      colorMid,
      colorHighlight,
      speed,
      flowStrength,
      grain,
      contrast,
      opacity,
      reveal,
      delayMs,
      revealDuration,
    }),
    [
      colorDeep,
      colorMid,
      colorHighlight,
      speed,
      flowStrength,
      grain,
      contrast,
      opacity,
      reveal,
      delayMs,
      revealDuration,
    ],
  );

  useEffect(() => {
    if (hasWebGLError) {
      return;
    }

    const canvas = canvasRef.current;
    const host = hostRef.current;
    if (!canvas || !host) {
      return;
    }

    try {
      const gl = canvas.getContext("webgl", { antialias: true, alpha: true });
      if (!gl) {
        setHasWebGLError(true);
        return;
      }

      const compileShader = (type: number, source: string) => {
        const shader = gl.createShader(type);
        if (!shader) {
          return null;
        }
        gl.shaderSource(shader, source);
        gl.compileShader(shader);
        if (!gl.getShaderParameter(shader, gl.COMPILE_STATUS)) {
          gl.deleteShader(shader);
          return null;
        }
        return shader;
      };

      const vertexShader = compileShader(gl.VERTEX_SHADER, VERTEX_SHADER);
      const fragmentShader = compileShader(gl.FRAGMENT_SHADER, FRAGMENT_SHADER);
      if (!vertexShader || !fragmentShader) {
        setHasWebGLError(true);
        return;
      }

      const program = gl.createProgram();
      if (!program) {
        gl.deleteShader(vertexShader);
        gl.deleteShader(fragmentShader);
        setHasWebGLError(true);
        return;
      }

      gl.attachShader(program, vertexShader);
      gl.attachShader(program, fragmentShader);
      gl.linkProgram(program);
      if (!gl.getProgramParameter(program, gl.LINK_STATUS)) {
        gl.deleteProgram(program);
        gl.deleteShader(vertexShader);
        gl.deleteShader(fragmentShader);
        setHasWebGLError(true);
        return;
      }

      gl.useProgram(program);

      const positionLocation = gl.getAttribLocation(program, "position");
      const quadBuffer = gl.createBuffer();
      gl.bindBuffer(gl.ARRAY_BUFFER, quadBuffer);
      gl.bufferData(
        gl.ARRAY_BUFFER,
        new Float32Array([-1, -1, 1, -1, -1, 1, 1, 1]),
        gl.STATIC_DRAW,
      );
      gl.enableVertexAttribArray(positionLocation);
      gl.vertexAttribPointer(positionLocation, 2, gl.FLOAT, false, 0, 0);

      const uRes = gl.getUniformLocation(program, "u_res");
      const uTime = gl.getUniformLocation(program, "u_time");
      const uColorDeep = gl.getUniformLocation(program, "u_colorDeep");
      const uColorMid = gl.getUniformLocation(program, "u_colorMid");
      const uColorHighlight = gl.getUniformLocation(program, "u_colorHighlight");
      const uSpeed = gl.getUniformLocation(program, "u_speed");
      const uFlowStrength = gl.getUniformLocation(program, "u_flowStrength");
      const uGrain = gl.getUniformLocation(program, "u_grain");
      const uContrast = gl.getUniformLocation(program, "u_contrast");
      const uOpacity = gl.getUniformLocation(program, "u_opacity");
      const uReveal = gl.getUniformLocation(program, "u_reveal");

      if (
        !uRes ||
        !uTime ||
        !uColorDeep ||
        !uColorMid ||
        !uColorHighlight ||
        !uSpeed ||
        !uFlowStrength ||
        !uGrain ||
        !uContrast ||
        !uOpacity ||
        !uReveal
      ) {
        gl.deleteBuffer(quadBuffer);
        gl.deleteProgram(program);
        gl.deleteShader(vertexShader);
        gl.deleteShader(fragmentShader);
        setHasWebGLError(true);
        return;
      }

      const resize = () => {
        const dpr = Math.min(window.devicePixelRatio || 1, 2);
        const { width, height } = host.getBoundingClientRect();
        canvas.width = Math.max(1, Math.floor(width * dpr));
        canvas.height = Math.max(1, Math.floor(height * dpr));
        gl.viewport(0, 0, canvas.width, canvas.height);
        gl.uniform2f(uRes, canvas.width, canvas.height);
      };

      resize();
      const resizeObserver = new ResizeObserver(resize);
      resizeObserver.observe(host);

      let rafId = 0;
      const start = performance.now();

      const render = (now: number) => {
        const elapsedSec = Math.max(0, (now - start - settings.delayMs) / 1000);
        const revealProgress = settings.reveal
          ? Math.min(1, elapsedSec / Math.max(settings.revealDuration, 0.05))
          : 1;

        const deep = hexToRgb01(settings.colorDeep);
        const mid = hexToRgb01(settings.colorMid);
        const highlight = hexToRgb01(settings.colorHighlight);

        gl.clearColor(0, 0, 0, 0);
        gl.clear(gl.COLOR_BUFFER_BIT);

        gl.uniform1f(uTime, elapsedSec);
        gl.uniform3f(uColorDeep, deep[0], deep[1], deep[2]);
        gl.uniform3f(uColorMid, mid[0], mid[1], mid[2]);
        gl.uniform3f(uColorHighlight, highlight[0], highlight[1], highlight[2]);
        gl.uniform1f(uSpeed, settings.speed);
        gl.uniform1f(uFlowStrength, settings.flowStrength);
        gl.uniform1f(uGrain, settings.grain);
        gl.uniform1f(uContrast, settings.contrast);
        gl.uniform1f(uOpacity, settings.opacity);
        gl.uniform1f(uReveal, revealProgress);
        gl.drawArrays(gl.TRIANGLE_STRIP, 0, 4);

        rafId = requestAnimationFrame(render);
      };

      rafId = requestAnimationFrame(render);

      return () => {
        cancelAnimationFrame(rafId);
        resizeObserver.disconnect();
        gl.deleteBuffer(quadBuffer);
        gl.deleteProgram(program);
        gl.deleteShader(vertexShader);
        gl.deleteShader(fragmentShader);
      };
    } catch {
      setHasWebGLError(true);
      return;
    }
  }, [hasWebGLError, settings]);

  const fallbackContent = (
    <div
      className={cn(
        "relative flex min-h-screen w-full items-center overflow-hidden bg-[#02040b] text-white",
        className,
      )}
      style={{ containerType: "size", ...style }}
      {...props}
    >
      <WebGLFallback className="absolute inset-0 h-full w-full" />
      {(title || subtitle || description || children) && (
        <div className="relative z-10 mx-auto w-full max-w-[1240px] px-6 py-20 md:px-10 md:py-28">
          <div className="max-w-[760px]">
            {title && <h1 className={LIQUID_HEADLINE_CLASS}>{title}</h1>}
            {subtitle && (
              <h2 className="mt-2 text-[11cqi] md:text-[7cqi] lg:text-[5.5cqi] leading-[0.9] tracking-[-0.03em] font-bold text-white/95">
                {subtitle}
              </h2>
            )}
            {description && (
              <p className="mt-6 max-w-[620px] text-base leading-relaxed text-white/75 md:text-xl">
                {description}
              </p>
            )}
            {children && <div className="mt-10">{children}</div>}
          </div>
        </div>
      )}
    </div>
  );

  if (hasWebGLError) {
    return fallbackContent;
  }

  return (
    <WebGLErrorBoundary fallback={fallbackContent}>
      <div
        ref={hostRef}
        className={cn(
          "relative flex min-h-screen w-full items-center overflow-hidden bg-[#02040b] text-white",
          className,
        )}
        style={{ containerType: "size", ...style }}
        {...props}
      >
        <canvas
          ref={canvasRef}
          aria-hidden="true"
          className="pointer-events-none absolute inset-0 h-full w-full"
          style={{ width: "100%", height: "100%", display: "block" }}
        />

        <div className="pointer-events-none absolute inset-0 bg-gradient-to-r from-black/35 via-black/15 to-transparent" />
        <div className="pointer-events-none absolute inset-0 bg-[radial-gradient(circle_at_65%_40%,rgba(255,255,255,0.16),transparent_45%)]" />

        {(title || subtitle || description || children) && (
          <div className="relative z-10 mx-auto w-full max-w-[1240px] px-6 py-20 md:px-10 md:py-28">
            <div className="max-w-[760px]">
              {title && <h1 className={LIQUID_HEADLINE_CLASS}>{title}</h1>}
              {subtitle && (
                <h2 className="mt-2 text-[11cqi] md:text-[7cqi] lg:text-[5.5cqi] leading-[0.9] tracking-[-0.03em] font-bold text-white/95">
                  {subtitle}
                </h2>
              )}
              {description && (
                <p className="mt-6 max-w-[620px] text-base leading-relaxed text-white/75 md:text-xl">
                  {description}
                </p>
              )}
              {children && <div className="mt-10">{children}</div>}
            </div>
          </div>
        )}
      </div>
    </WebGLErrorBoundary>
  );
}

export default WebGLLiquid;
```
##### File: `components/ui/webgl-error-boundary.tsx`
```tsx
"use client";

import { cn } from "@/lib/utils";
import * as React from "react";

interface WebGLErrorBoundaryProps {
  children: React.ReactNode;
  fallback?: React.ReactNode;
  onError?: (error: Error, errorInfo: React.ErrorInfo) => void;
}

interface WebGLErrorBoundaryState {
  hasError: boolean;
}

export class WebGLErrorBoundary extends React.Component<
  WebGLErrorBoundaryProps,
  WebGLErrorBoundaryState
> {
  public state: WebGLErrorBoundaryState = { hasError: false };

  static getDerivedStateFromError(): WebGLErrorBoundaryState {
    return { hasError: true };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    this.props.onError?.(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback ?? <WebGLFallback />;
    }
    return this.props.children;
  }
}

interface WebGLFallbackProps {
  className?: string;
  message?: string;
}

export function WebGLFallback({
  className,
  message = "Interactive WebGL content is unavailable on this device/browser.",
}: WebGLFallbackProps) {
  return (
    <div
      className={cn(
        "flex h-full w-full items-center justify-center bg-gradient-to-br from-zinc-950 via-slate-900 to-zinc-900 px-4 text-center text-sm text-white/75",
        className,
      )}
      role="status"
      aria-live="polite"
    >
      <p>{message}</p>
    </div>
  );
}
```
---
