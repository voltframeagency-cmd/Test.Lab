# Chamaac UI Creative Developer Toolbox Reference Guide

A complete, structured developer guide for **Chamaac UI** (chamaac.com) — a set of premium React WebGL shaders, animated interactive components, and micro-interaction icons built with Three.js, React Three Fiber (R3F), Framer Motion, and Tailwind CSS.

## Table of Contents

- [WebGL Shaders and Backgrounds](#webgl-shaders-and-backgrounds)
  - [Astral Flow](#astral-flow)
  - [Electric Mist](#electric-mist)
  - [Grid Bloom](#grid-bloom)
  - [Light Speed](#light-speed)
  - [Liquid Chrome](#liquid-chrome)
  - [Nebula](#nebula)
  - [Synthesis](#synthesis)
  - [Water Caustic](#water-caustic)
  - [Waves](#waves)
- [Interactive UI Components](#interactive-ui-components)
  - [AI Input](#ai-input)
  - [Carousel](#carousel)
  - [Dancing Letters](#dancing-letters)
  - [Dock](#dock)
  - [Feature Steps](#feature-steps)
  - [Focus Button](#focus-button)
  - [Gauge](#gauge)
  - [Gif Text](#gif-text)
  - [Glowing Border Button](#glowing-border-button)
  - [Hover Arrow Button](#hover-arrow-button)
  - [How It Works](#how-it-works)
  - [Interactive Grid Background](#interactive-grid-background)
  - [Neo Brutalist Button](#neo-brutalist-button)
  - [Premium Button](#premium-button)
  - [Shimmer Button](#shimmer-button)
  - [Slide Up Button](#slideup-button)
  - [Stats Cards](#stats-cards)
  - [Text Loop](#text-loop)
- [Animated Icons](#animated-icons)
  - [Activity Icon](#activity-icon)
  - [Adjustments Icon](#adjustments-icon)
  - [Alert Icon](#alert-circle-icon)
  - [Arrow Down Icon](#arrow-down-icon)
  - [Arrow Down Left Icon](#arrow-down-left-icon)
  - [Arrow Down Right Icon](#arrow-down-right-icon)
  - [Arrow Left Icon](#arrow-left-icon)
  - [Arrow Right Icon](#arrow-right-icon)
  - [Arrow Up Icon](#arrow-up-icon)
  - [Arrow Up Left Icon](#arrow-up-left-icon)
  - [Arrow Up Right Icon](#arrow-up-right-icon)
  - [Battery Icon](#battery-icon)
  - [Bell Icon](#bell-icon)
  - [Calendar Icon](#calendar-icon)
  - [Cart Icon](#cart-icon)
  - [Check Icon](#check-icon)
  - [Chevron Down Icon](#chevron-down-icon)
  - [Chevron Left Icon](#chevron-left-icon)
  - [Chevron Right Icon](#chevron-right-icon)
  - [Chevron Up Icon](#chevron-up-icon)
  - [Chevrons Down Icon](#chevrons-down-icon)
  - [Chevrons Left Icon](#chevrons-left-icon)
  - [Chevrons Right Icon](#chevrons-right-icon)
  - [Chevrons Up Icon](#chevrons-up-icon)
  - [Clock Icon](#clock-icon)
  - [Close Icon](#close-icon)
  - [Cloud Icon](#cloud-icon)
  - [Copy Icon](#copy-icon)
  - [Credit Card Icon](#credit-card-icon)
  - [Crown Icon](#crown-icon)
  - [Download Icon](#download-icon)
  - [External Link Icon](#external-link-icon)
  - [Eye Icon](#eye-icon)
  - [Gift Icon](#gift-icon)
  - [Globe Icon](#globe-icon)
  - [Heart Icon](#heart-icon)
  - [Hourglass Icon](#hourglass-icon)
  - [Layers Icon](#layers-icon)
  - [Lightbulb Icon](#lightbulb-icon)
  - [Location Icon](#location-icon)
  - [Lock Icon](#lock-icon)
  - [Mail Icon](#mail-icon)
  - [Menu Icon](#menu-icon)
  - [Message Icon](#message-icon)
  - [Moon Icon](#moon-icon)
  - [Music Icon](#music-icon)
  - [Refresh Icon](#refresh-icon)
  - [Rocket Icon](#rocket-icon)
  - [Scan Icon](#scan-icon)
  - [Search Icon](#search-icon)
  - [Send Icon](#send-icon)
  - [Settings Icon](#settings-icon)
  - [Shield Icon](#shield-icon)
  - [Sparkle Icon](#sparkle-icon)
  - [Star Icon](#star-icon)
  - [Sun Icon](#sun-icon)
  - [Thumbs Up Icon](#thumbs-up-icon)
  - [Trash Icon](#trash-icon)
  - [Upload Icon](#upload-icon)
  - [Volume Icon](#volume-icon)
  - [Wavy Icon](#wavy-icon)
  - [Wifi Icon](#wifi-icon)
  - [Zap Icon](#zap-icon)

---

## WebGL Shaders and Backgrounds <a name="webgl-shaders-and-backgrounds"></a>

### Astral Flow <a name="astral-flow"></a>

A majestic, constantly breathing radial shader with deep cosmic wisps.

- **Registry Name**: `astral-flow`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/astral-flow.json
```

#### Dependencies

- **NPM Packages**:
  - `three`
  - `@react-three/fiber`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/astral-flow/astral-flow.tsx`

```tsx
"use client";

import { Canvas, useFrame } from "@react-three/fiber";
import { useRef, useMemo, useEffect } from "react";
import * as THREE from "three";
import { cn } from "@/lib/utils";

const vertexShader = `
  varying vec2 vUv;
  void main() {
    vUv = uv;
    gl_Position = vec4(position, 1.0);
  }
`;

const fragmentShader = `
  uniform float uTime;
  uniform vec2 uResolution;
  uniform vec3 uColor1;
  uniform vec3 uColor2;
  uniform vec3 uColor3;
  uniform float uFlowMin;
  uniform float uFlowMax;

  varying vec2 vUv;

  // Modern Simplex/Perlin Noise Hash for smooth wispy smoke
  vec3 mod289(vec3 x) { return x - floor(x * (1.0 / 289.0)) * 289.0; }
  vec2 mod289(vec2 x) { return x - floor(x * (1.0 / 289.0)) * 289.0; }
  vec3 permute(vec3 x) { return mod289(((x*34.0)+1.0)*x); }

  float snoise(vec2 v) {
      const vec4 C = vec4(0.211324865405187,  // (3.0-sqrt(3.0))/6.0
                          0.366025403784439,  // 0.5*(sqrt(3.0)-1.0)
                         -0.577350269189626,  // -1.0 + 2.0 * C.x
                          0.024390243902439); // 1.0 / 41.0
      vec2 i  = floor(v + dot(v, C.yy) );
      vec2 x0 = v -   i + dot(i, C.xx);
      vec2 i1;
      i1 = (x0.x > x0.y) ? vec2(1.0, 0.0) : vec2(0.0, 1.0);
      vec4 x12 = x0.xyxy + C.xxzz;
      x12.xy -= i1;
      i = mod289(i); // Avoid truncation effects in permutation
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

  // Fractional Brownian Motion (fbm) to create wispy fractal smoke
  float fbm(vec2 x) {
      float v = 0.0;
      float a = 0.5;
      vec2 shift = vec2(100.0);
      // Rotate to add spiral variation
      mat2 rot = mat2(cos(0.5), sin(0.5), -sin(0.5), cos(0.50));
      for (int i = 0; i < 5; ++i) { // Restored to 5 layers for maximum elegance and detail
          v += a * snoise(x);
          x = rot * x * 2.0 + shift;
          a *= 0.5;
      }
      return v;
  }

  void main() {
      // Fix aspect ratio for smooth scaling
      vec2 uv = vUv;
      vec2 p = uv * 2.0 - 1.0;
      p.x *= uResolution.x / uResolution.y;

      // Map the sine wave (-1 to 1) to the desired flow range (uFlowMin to uFlowMax)
      float midpoint = (uFlowMax + uFlowMin) / 2.0;
      float amplitude = (uFlowMax - uFlowMin) / 2.0;
      
      // Oscillate structure time to prevent radial stretching (starts at uFlowMin)
      float flowTime = (midpoint + amplitude * sin(uTime * 0.5 - 1.5708)) * 0.25;
      
      // Endlessly rotating drift to keep smoke fresh without math precision breakdown
      // By using a trig function rather than linear addition, coordinates never exceed float16 bounds!
      vec2 drift = vec2(sin(uTime * 0.15), cos(uTime * 0.15));

      // Soft radial flow outward from center (averts sharp pinch singularity)
      float radius = length(p);
      vec2 dir = p / (radius + 0.1); 

      // Primary Flow (breathing outwards and inwards smoothly)
      vec2 q = vec2(0.);
      q.x = fbm( p - dir * flowTime );
      q.y = fbm( p - dir * flowTime + vec2(1.0));

      // Secondary Flow (whispy detail pushing through the smoke continuously)
      vec2 r = vec2(0.);
      r.x = fbm( p + 1.0 * q + vec2(1.7, 9.2) - dir * flowTime * 0.8 + drift );
      r.y = fbm( p + 1.0 * q + vec2(8.3, 2.8) - dir * flowTime * 0.6 + drift );

      // Final Shape Density Map
      float f = fbm(p + r);

      // Color mapping mixing based on density depth
      vec3 color = mix(
          uColor1, // Darkest base
          uColor2, // Mid-tone
          clamp((f * f) * 4.0, 0.0, 1.0)
      );

      color = mix(
          color,
          uColor3, // High-light wisps
          clamp(length(q), 0.0, 1.0)
      );

      // Enhance the wispy brightness dynamically
      color += uColor3 * (f * f * f * 1.5) * 0.;

      // Darken edges slightly for vignette
      float vignette = length(uv - 0.5);
      color = mix(color, vec3(0.0), smoothstep(0.4, 1.5, vignette));

      // Atmospheric fade-in depth
      float opacity = smoothstep(0.1, 0.9, f + 0.4);

      gl_FragColor = vec4(color, opacity);
  }
`;

interface AstralFlowProps {
  className?: string;
  speed?: number;
  color1?: string;
  color2?: string;
  color3?: string;
  flowMin?: number;
  flowMax?: number;
}

const Effect = ({
  speed,
  color1,
  color2,
  color3,
  flowMin,
  flowMax,
}: Required<Omit<AstralFlowProps, "className">>) => {
  const material = useRef<THREE.ShaderMaterial>(null);

  const uniforms = useMemo(
    () => ({
      uTime: { value: 0 },
      uResolution: { value: new THREE.Vector2() },
      uColor1: { value: new THREE.Color(color1) },
      uColor2: { value: new THREE.Color(color2) },
      uColor3: { value: new THREE.Color(color3) },
      uFlowMin: { value: flowMin },
      uFlowMax: { value: flowMax },
    }),
    [] // Initialize only once to prevent shader recreation
  );

  // Smoothly update uniform values
  useEffect(() => {
    if (material.current) {
      material.current.uniforms.uColor1.value.set(color1);
      material.current.uniforms.uColor2.value.set(color2);
      material.current.uniforms.uColor3.value.set(color3);
      material.current.uniforms.uFlowMin.value = flowMin;
      material.current.uniforms.uFlowMax.value = flowMax;
    }
  }, [color1, color2, color3, flowMin, flowMax]);

  useFrame((state) => {
    if (material.current) {
      material.current.uniforms.uTime.value =
        (state.clock.getElapsedTime() * speed) % (40 * Math.PI);
      material.current.uniforms.uResolution.value.set(
        state.size.width * state.viewport.dpr,
        state.size.height * state.viewport.dpr
      );
    }
  });

  return (
    <mesh>
      <planeGeometry args={[2, 2]} />
      <shaderMaterial
        ref={material}
        vertexShader={vertexShader}
        fragmentShader={fragmentShader}
        uniforms={uniforms}
        transparent={true}
      />
    </mesh>
  );
};

export default function AstralFlow({
  className,
  speed = 1.5,
  color1 = "#05070a", // Deep void blue-black
  color2 = "#2e1a38", // Moody dark plum/purple
  color3 = "#a0769a", // Glowing ethereal mauve/silver
  flowMin = 3.0,
  flowMax = 7.0,
}: AstralFlowProps) {
  return (
    <div
      className={cn(
        "absolute inset-0 w-full h-full pointer-events-none overflow-hidden bg-[#05070a]",
        className
      )}
    >
      <Canvas camera={{ position: [0, 0, 1] }} dpr={1}>
        <Effect
          speed={speed}
          color1={color1}
          color2={color2}
          color3={color3}
          flowMin={flowMin}
          flowMax={flowMax}
        />
      </Canvas>
      <div
        className="absolute inset-0 pointer-events-none opacity-[0.4] mix-blend-overlay"
        style={{
          backgroundImage: `url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E")`,
        }}
      />
    </div>
  );
}
```

---

### Electric Mist <a name="electric-mist"></a>

A high-energy glowing lightning shader with waves and smoke effects.

- **Registry Name**: `electric-mist`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/electric-mist.json
```

#### Dependencies

- **NPM Packages**:
  - `three`
  - `@react-three/fiber`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/electric-mist/electric-mist.tsx`

```tsx
"use client";

import { Canvas, useFrame } from "@react-three/fiber";
import { useRef, useMemo, useEffect } from "react";
import * as THREE from "three";
import { cn } from "@/lib/utils";

const vertexShader = `
  varying vec2 vUv;
  void main() {
    vUv = uv;
    gl_Position = vec4(position, 1.0);
  }
`;

const fragmentShader = `
  uniform float uTime;
  uniform vec2 uResolution;
  uniform vec3 uColor;
  uniform float uSpeed;
  uniform float uDetail;
  uniform float uDistortion;
  uniform float uBrightness;
  varying vec2 vUv;

  #define time uTime * 0.2

  mat2 makem2(in float theta){
      float c = cos(theta);
      float s = sin(theta);
      return mat2(c,-s,s,c);
  }

  float hash(vec2 p) {
      return fract(sin(dot(p, vec2(12.9898, 78.233))) * 43758.5453);
  }

  float noise( in vec2 x, float detail ){
      x *= detail; 
      vec2 p = floor(x);
      vec2 f = fract(x);
      f = f * f * (3.0 - 2.0 * f);
      float a = hash(p + vec2(0.0, 0.0));
      float b = hash(p + vec2(1.0, 0.0));
      float c = hash(p + vec2(0.0, 1.0));
      float d = hash(p + vec2(1.0, 1.0));
      return mix(mix(a, b, f.x), mix(c, d, f.x), f.y);
  }

  mat2 m2 = mat2( 0.80,  0.60, -0.60,  0.80 );

  float fbm( in vec2 p, float detail, int octaves )
  { 
      float z=2.;
      float rz = 0.;
      for (int i= 0; i < 7; i++ )
      {
          if(i >= octaves) break;
          rz += abs((noise(p, detail)-0.5)*4.)/z;
          z = z*2.;
          p = p*2.;
          p *= m2;
      }
      return rz;
  }

 void main() {
    vec2 p = vUv * 2.0 - 1.0;
    p.x *= uResolution.x/uResolution.y;
    vec2 bp = p;
    p += 5.;
    p *= 0.5;

    float rb = fbm(p*.5 + time*.17, uDetail, 3) * .1;
    // rb = sqrt(rb);
    p *= makem2(rb*.2 + atan(p.y,p.x) * uDistortion);

    float rz = fbm(p*.9 - time*.7, uDetail, 5);
    
    rz *= 12.0; 

    rz *= abs(sin(bp.x*0.5 - time*4.0 - 2.0)) * 1.0;

    vec3 col = uColor / (uBrightness - rz);

    gl_FragColor = vec4(sqrt(abs(col)), 1.0);
}
`;

interface ElectricMistProps {
  className?: string;
  speed?: number;
  color?: string;
  detail?: number;
  distortion?: number;
  brightness?: number;
}

const Effect = ({
  speed,
  color,
  detail,
  distortion,
  brightness,
}: Required<Omit<ElectricMistProps, "className">>) => {
  const material = useRef<THREE.ShaderMaterial>(null);

  const uniforms = useMemo(
    () => ({
      uTime: { value: 0 },
      uResolution: { value: new THREE.Vector2() },
      uColor: { value: new THREE.Color(color) },
      uSpeed: { value: speed },
      uDetail: { value: detail },
      uDistortion: { value: distortion },
      uBrightness: { value: brightness },
    }),
    []
  );

  useEffect(() => {
    if (material.current) {
      material.current.uniforms.uColor.value.set(color);
      material.current.uniforms.uSpeed.value = speed;
      material.current.uniforms.uDetail.value = detail;
      material.current.uniforms.uDistortion.value = distortion;
      material.current.uniforms.uBrightness.value = brightness;
    }
  }, [color, speed, detail, distortion, brightness]);

  useFrame((state) => {
    if (material.current) {
      material.current.uniforms.uTime.value =
        state.clock.getElapsedTime() * speed;
      material.current.uniforms.uResolution.value.set(
        state.size.width,
        state.size.height
      );
    }
  });

  return (
    <mesh>
      <planeGeometry args={[2, 2]} />
      <shaderMaterial
        ref={material}
        vertexShader={vertexShader}
        fragmentShader={fragmentShader}
        uniforms={uniforms}
      />
    </mesh>
  );
};

export default function ElectricMist({
  className,
  speed = 1.0,
  color = "#191970",
  detail = 1.5,
  distortion = 3.0,
  brightness = 1.0,
}: ElectricMistProps) {
  return (
    <div
      className={cn(
        "absolute inset-0 w-full h-full pointer-events-none bg-[#05050f] overflow-hidden",
        className
      )}
    >
      <Canvas
        camera={{ position: [0, 0, 1] }}
        dpr={1}
        gl={{ antialias: false, powerPreference: "high-performance" }}
      >
        <Effect
          speed={speed}
          color={color}
          detail={detail}
          distortion={distortion}
          brightness={brightness}
        />
      </Canvas>
    </div>
  );
}
```

---

### Grid Bloom <a name="grid-bloom"></a>

A mesmerizing grid pattern with pulsing wave interference.

- **Registry Name**: `grid-bloom`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/grid-bloom.json
```

#### Dependencies

- **NPM Packages**:
  - `three`
  - `@react-three/fiber`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/grid-bloom/grid-bloom.tsx`

```tsx
"use client";

import { useRef, useMemo, useEffect } from "react";
import { Canvas, useFrame, useThree } from "@react-three/fiber";
import * as THREE from "three";
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}

const vertexShader = `
varying vec2 vUv;
void main() {
  vUv = uv;
  gl_Position = vec4(position, 1.0);
}
`;

const fragmentShader = `
uniform float iTime;
uniform vec2 iResolution;
uniform vec3 uColor;
uniform float uSpeed;
uniform float uGridScale;
uniform float uRotationSpeed;
uniform float uFadeFalloff;
uniform float uDistortionAmount;
uniform float uFlowSpeedX;
uniform float uFlowSpeedY;
uniform float uHoverRepulsionRadius;
uniform float uHoverRepulsionStrength;
uniform float uHoverLightRadius;
uniform float uMouseActive;
uniform vec2 iMouse;
varying vec2 vUv;

// Simplex 2D noise
vec3 permute(vec3 x) { return mod(((x*34.0)+10.0)*x, 289.0); }
float snoise(vec2 v) {
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
  vec3 m = max(0.5 - vec3(dot(x0,x0), dot(x12.xy,x12.xy),
    dot(x12.zw,x12.zw)), 0.0);
  m = m*m ;
  m = m*m ;
  vec3 x = 2.0 * fract(p * C.www) - 1.0;
  vec3 h = abs(x) - 0.5;
  vec3 ox = floor(x + 0.5);
  vec3 a0 = x - ox;
  m *= 1.792843 - 0.853735 * ( a0*a0 + h*h );
  vec3 g;
  g.x  = a0.x  * x0.x  + h.x  * x0.y;
  g.yz = a0.yz * x12.xz + h.yz * x12.yw;
  return 130.0 * dot(m, g);
}

void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    vec2 unrotatedP = (fragCoord.xy - 0.5 * iResolution.xy) / iResolution.y;

    // Mouse math (distance in unrotated screen space)
    vec2 mouseP = (iMouse.xy - 0.5 * iResolution.xy) / iResolution.y;
    vec2 mouseDir = unrotatedP - mouseP;
    float mouseDist = length(mouseDir);
    float mouseInfluence = smoothstep(uHoverRepulsionRadius, 0.0, mouseDist) * uMouseActive;

    // Apply soft rotation
    float rot = iTime * uRotationSpeed * 0.3;
    mat2 m = mat2(cos(rot), -sin(rot), sin(rot), cos(rot));
    vec2 p = m * unrotatedP;

    // Organic fluid distortion
    float noiseDist = snoise(p * 1.5 + iTime * uSpeed * 0.15);
    vec2 distortedPos = p + vec2(noiseDist * uDistortionAmount);

    // Mouse lens distortion (push grid away from mouse fluidly)
    vec2 rotatedMouseDir = m * mouseDir;
    distortedPos += rotatedMouseDir * mouseInfluence * uHoverRepulsionStrength;

    // Apply grid scaling
    vec2 gridPos = distortedPos * uGridScale;
    
    // Flowing motion
    gridPos.x += iTime * uSpeed * uFlowSpeedX;
    gridPos.y += iTime * uSpeed * uFlowSpeedY;

    vec2 cell = fract(gridPos);
    vec2 cellCenter = abs(cell - 0.5);

    // Modern glowing lines: thin and smooth
    float lineWidth = 0.015;
    float smoothEdge = 0.03;
    vec2 lines = smoothstep(0.5 - lineWidth - smoothEdge, 0.5 - lineWidth, cellCenter);
    float gridAlpha = max(lines.x, lines.y);
    
    // Add pulsing node intersections
    float intersections = lines.x * lines.y;
    
    // Randomized glowing orbs at some intersections
    float glowMask = snoise(floor(gridPos) * 0.4 + iTime * uSpeed * 0.4);
    float glow = smoothstep(0.2, 0.5, cellCenter.x) * smoothstep(0.2, 0.5, cellCenter.y);
    glow *= smoothstep(0.3, 0.8, glowMask);

    // Elegant moire/interference pulse
    float pulseDist = length(p);
    float pulse = 0.5 + 0.5 * sin(pulseDist * 8.0 - iTime * uSpeed * 1.5 + noiseDist * 2.0);

    // Assemble the bloom layers
    float finalAlpha = (gridAlpha * 0.3) + (intersections * 0.8) + (glow * 0.6);
    
    // Airborne brightness modulation
    finalAlpha *= (0.6 + 0.4 * snoise(p * 4.0 - iTime * uSpeed * 0.5));
    finalAlpha += finalAlpha * pulse * 0.4;

    // Mouse glow interaction
    float mouseGlow = smoothstep(uHoverLightRadius, 0.0, mouseDist) * 0.6 * uMouseActive;
    finalAlpha += mouseGlow * gridAlpha; // Illuminate the grid directly

    // Sophisticated vignette fading
    float vignette = 1.0 - smoothstep(0.1, uFadeFalloff, pulseDist);
    
    // Overall ambient breathing breathing
    float breathing = 0.8 + 0.2 * sin(iTime * uSpeed * 0.8);

    fragColor = vec4(uColor, clamp(finalAlpha * vignette * breathing, 0.0, 1.0));
}

void main() {
    mainImage(gl_FragColor, vUv * iResolution);
}
`;

interface ShaderPlaneProps {
  color: string;
  speed: number;
  gridScale: number;
  rotationSpeed: number;
  fadeFalloff: number;
  distortionAmount: number;
  flowSpeedX: number;
  flowSpeedY: number;
  hoverLightRadius: number;
  hoverRepulsionRadius: number;
  hoverRepulsionStrength: number;
  enableMouseInteraction: boolean;
}

const ShaderPlane = ({
  color,
  speed,
  gridScale,
  rotationSpeed,
  fadeFalloff,
  distortionAmount,
  flowSpeedX,
  flowSpeedY,
  hoverLightRadius,
  hoverRepulsionRadius,
  hoverRepulsionStrength,
  enableMouseInteraction,
}: ShaderPlaneProps) => {
  const materialRef = useRef<THREE.ShaderMaterial>(null);
  const gl = useThree((s) => s.gl);
  const mouseRef = useRef({
    x: -1000,
    y: -1000,
    targetX: -1000,
    targetY: -1000,
    active: 0,
    targetActive: 0,
  });

  const uniforms = useMemo(
    () => ({
      iTime: { value: 0 },
      iResolution: { value: new THREE.Vector2() },
      iMouse: { value: new THREE.Vector2(-1000, -1000) },
      uMouseActive: { value: 0 },
      uColor: { value: new THREE.Color(color) },
      uSpeed: { value: speed },
      uGridScale: { value: gridScale },
      uRotationSpeed: { value: rotationSpeed },
      uFadeFalloff: { value: fadeFalloff },
      uDistortionAmount: { value: distortionAmount },
      uFlowSpeedX: { value: flowSpeedX },
      uFlowSpeedY: { value: flowSpeedY },
      uHoverLightRadius: { value: hoverLightRadius },
      uHoverRepulsionRadius: { value: hoverRepulsionRadius },
      uHoverRepulsionStrength: { value: hoverRepulsionStrength },
    }),
    // eslint-disable-next-line react-hooks/exhaustive-deps
    []
  );

  useEffect(() => {
    uniforms.uColor.value.set(color);
  }, [color, uniforms]);
  useEffect(() => {
    uniforms.uSpeed.value = speed;
  }, [speed, uniforms]);
  useEffect(() => {
    uniforms.uGridScale.value = gridScale;
  }, [gridScale, uniforms]);
  useEffect(() => {
    uniforms.uRotationSpeed.value = rotationSpeed;
  }, [rotationSpeed, uniforms]);
  useEffect(() => {
    uniforms.uFadeFalloff.value = fadeFalloff;
  }, [fadeFalloff, uniforms]);
  useEffect(() => {
    uniforms.uDistortionAmount.value = distortionAmount;
  }, [distortionAmount, uniforms]);
  useEffect(() => {
    uniforms.uFlowSpeedX.value = flowSpeedX;
  }, [flowSpeedX, uniforms]);

  useEffect(() => {
    uniforms.uFlowSpeedY.value = flowSpeedY;
  }, [flowSpeedY, uniforms]);
  useEffect(() => {
    uniforms.uHoverLightRadius.value = hoverLightRadius;
  }, [hoverLightRadius, uniforms]);
  useEffect(() => {
    uniforms.uHoverRepulsionRadius.value = hoverRepulsionRadius;
  }, [hoverRepulsionRadius, uniforms]);
  useEffect(() => {
    uniforms.uHoverRepulsionStrength.value = hoverRepulsionStrength;
  }, [hoverRepulsionStrength, uniforms]);

  useEffect(() => {
    if (!enableMouseInteraction) {
      mouseRef.current.targetActive = 0;
      mouseRef.current.active = 0;
      return;
    }

    const handlePointerMove = (e: PointerEvent) => {
      const rect = gl.domElement.getBoundingClientRect();
      const inside =
        e.clientX >= rect.left &&
        e.clientX <= rect.right &&
        e.clientY >= rect.top &&
        e.clientY <= rect.bottom;

      mouseRef.current.targetActive = inside ? 1 : 0;

      if (inside) {
        mouseRef.current.targetX = e.clientX - rect.left;
        mouseRef.current.targetY = rect.bottom - e.clientY;
      }
    };

    const handlePointerLeave = () => {
      mouseRef.current.targetActive = 0;
    };

    window.addEventListener("pointermove", handlePointerMove);
    document.addEventListener("pointerleave", handlePointerLeave);

    return () => {
      window.removeEventListener("pointermove", handlePointerMove);
      document.removeEventListener("pointerleave", handlePointerLeave);
    };
  }, [gl.domElement, enableMouseInteraction]);

  useFrame((state) => {
    if (materialRef.current) {
      // Smooth mouse interpolation (spring-like physics follow)
      mouseRef.current.x +=
        (mouseRef.current.targetX - mouseRef.current.x) * 0.1;
      mouseRef.current.y +=
        (mouseRef.current.targetY - mouseRef.current.y) * 0.1;

      // Faster fade out physics for the light
      mouseRef.current.active +=
        (mouseRef.current.targetActive - mouseRef.current.active) * 0.15;

      materialRef.current.uniforms.iTime.value = state.clock.elapsedTime;
      materialRef.current.uniforms.iResolution.value.set(
        state.size.width * state.viewport.dpr,
        state.size.height * state.viewport.dpr
      );
      materialRef.current.uniforms.iMouse.value.set(
        mouseRef.current.x * state.viewport.dpr,
        mouseRef.current.y * state.viewport.dpr
      );
      materialRef.current.uniforms.uMouseActive.value = mouseRef.current.active;
    }
  });

  return (
    <mesh>
      <planeGeometry args={[2, 2]} />
      {/* eslint-disable react/no-unknown-property */}
      <shaderMaterial
        ref={materialRef}
        vertexShader={vertexShader}
        fragmentShader={fragmentShader}
        uniforms={uniforms}
        depthWrite={false}
        depthTest={false}
        transparent={true}
        blending={THREE.AdditiveBlending}
      />
      {/* eslint-enable react/no-unknown-property */}
    </mesh>
  );
};

export interface GridBloomProps {
  className?: string;
  /** Bloom color */
  color?: string;
  /** Overall animation speed multiplier */
  speed?: number;
  /** Density of the grid (higher = more tiles) */
  gridScale?: number;
  /** Speed of the slow grid rotation */
  rotationSpeed?: number;
  /** Controls how quickly the bloom fades out to the edges. Default: 10.0. Lower = sharper fade. Higher = softer/no fade. */
  fadeFalloff?: number;
  /** Amount of noise-based distortion applied to the grid lines. Default: 0.08. Setting to 0.0 gives rigid, straight lines. */
  distortionAmount?: number;
  /** Horizontal scrolling speed of the grid. Default: -0.2 */
  flowSpeedX?: number;
  /** Vertical scrolling speed of the grid. Default: -0.4 */
  flowSpeedY?: number;
  /** Radius of the light illumination under the mouse. Default: 0.5. Higher = larger light aura. */
  hoverLightRadius?: number;
  /** Radius of the structural push effect from the mouse. Default: 0.6. */
  hoverRepulsionRadius?: number;
  /** Strength of the geometric push effect from the mouse. Default: 0.3. Setting to 0.0 disables the warp. */
  hoverRepulsionStrength?: number;
  /** Whether mouse hover interaction (light aura + repulsion) is enabled. Default: true. */
  enableMouseInteraction?: boolean;
}

export default function GridBloom({
  className,
  color = "#e040fb",
  speed = 1.0,
  gridScale = 12.0,
  rotationSpeed = 0.0,
  fadeFalloff = 10.0,
  distortionAmount = 0.05,
  flowSpeedX = -0.2,
  flowSpeedY = -0.4,
  hoverLightRadius = 0.5,
  hoverRepulsionRadius = 1.0,
  hoverRepulsionStrength = 0.6,
  enableMouseInteraction = true,
}: GridBloomProps) {
  return (
    <div
      className={cn(
        "w-full h-full absolute inset-0 pointer-events-none",
        className
      )}
    >
      <Canvas
        camera={{ position: [0, 0, 1] }}
        gl={{ antialias: false, alpha: true }}
        dpr={[1, 2]}
      >
        <ShaderPlane
          color={color}
          speed={speed}
          gridScale={gridScale}
          rotationSpeed={rotationSpeed}
          fadeFalloff={fadeFalloff}
          distortionAmount={distortionAmount}
          flowSpeedX={flowSpeedX}
          flowSpeedY={flowSpeedY}
          hoverLightRadius={hoverLightRadius}
          hoverRepulsionRadius={hoverRepulsionRadius}
          hoverRepulsionStrength={hoverRepulsionStrength}
          enableMouseInteraction={enableMouseInteraction}
        />
      </Canvas>
    </div>
  );
}
```

---

### Light Speed <a name="light-speed"></a>

A Warp/Light-Speed hyperspace animation inspired by the Ducky3D Blender tutorial.

- **Registry Name**: `light-speed`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/light-speed.json
```

#### Dependencies

- **NPM Packages**:
  - `three`
  - `@react-three/fiber`
  - `@react-three/postprocessing`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/light-speed/light-speed.tsx`

```tsx
"use client";

import React, { useRef, useMemo } from "react";
import { Canvas, useFrame } from "@react-three/fiber";
import * as THREE from "three";
import { EffectComposer, Bloom } from "@react-three/postprocessing";
import { cn } from "@/lib/utils";

export interface LightSpeedProps {
  /**
   * Number of particles (stars/lines).
   * @default 2000
   */
  particleCount?: number;
  /**
   * Base speed of the warp effect.
   * @default 4
   */
  speed?: number;
  /**
   * Base color of the emitted light streaks.
   * @default "#33b2ff"
   */
  lightColor?: string;
  /**
   * Intensity of the bloom glow.
   * @default 3.0
   */
  intensity?: number;
  /**
   * Extent of the cylinder radius in which particles spawn.
   * @default 25
   */
  radius?: number;
  /**
   * Length of the cylinder before particles loop back.
   * @default 150
   */
  cylinderLength?: number;
  className?: string;
}

function Particles({
  count,
  baseSpeed,
  lightColor,
  intensity,
  radius,
  cylinderLength,
}: {
  count: number;
  baseSpeed: number;
  lightColor: string;
  intensity: number;
  radius: number;
  cylinderLength: number;
}) {
  const meshRef = useRef<THREE.InstancedMesh>(null);

  // Object3D to help apply transforms to individual instanced items
  const dummy = useMemo(() => new THREE.Object3D(), []);

  // Initialize particle properties
  const particles = useMemo(() => {
    const temp = [];
    for (let i = 0; i < count; i++) {
      const angle = Math.random() * Math.PI * 2;
      // Start further away from the center to leave a "tunnel" for the camera
      const r = 2 + Math.random() * (radius - 2);
      const x = Math.cos(angle) * r;
      const y = Math.sin(angle) * r;
      let z = (Math.random() - 0.5) * cylinderLength;
      let speedMultiplier = 0.5 + Math.random() * 0.5;

      // Pre-warm the particle simulation by 1.5 seconds
      for (let j = 0; j < 90; j++) {
        z += baseSpeed * speedMultiplier * (1 / 60) * 50;
        if (z > 5) {
          z = -cylinderLength / 2;
          speedMultiplier = 0.5 + Math.random() * 0.5;
        }
      }

      temp.push({
        x,
        y,
        z,
        // Individual random speed multiplier to give depth variation
        speedMultiplier,
        // Individual random length to look more organic
        length: 1 + Math.random() * 2,
        angle,
        radius: r,
      });
    }
    return temp;
  }, [count, radius, cylinderLength]);

  const bloomColor = useMemo(() => {
    const color = new THREE.Color(lightColor);
    color.multiplyScalar(intensity);
    return color;
  }, [lightColor, intensity]);

  useFrame((state, delta) => {
    if (!meshRef.current) return;

    // We update each particle's matrix and apply rotation/translation
    particles.forEach((particle, i) => {
      // Move particle towards the camera (+Z)
      // delta makes the movement frame-rate independent.
      const moveDistance = baseSpeed * particle.speedMultiplier * delta * 50;
      particle.z += moveDistance;

      // If a particle passes the camera (z > 5), loop it back to the far end
      if (particle.z > 5) {
        particle.z = -cylinderLength / 2;
        particle.speedMultiplier = 0.5 + Math.random() * 0.5;
      }

      // We place the dummy at the particle coordinates
      dummy.position.set(particle.x, particle.y, particle.z);
      // We scale the length on the Z-axis to mimic motion blur/stretched UV spheres
      // The faster it moves, the more stretched it appears
      const stretchZ =
        particle.length + baseSpeed * particle.speedMultiplier * 0.5;
      // X and Y are scaled small to look thin (like streaks)
      dummy.scale.set(0.04, 0.04, stretchZ);

      dummy.updateMatrix();
      meshRef.current!.setMatrixAt(i, dummy.matrix);
    });

    // Tell Three.js the matrix data has been updated and deserves a re-render
    meshRef.current.instanceMatrix.needsUpdate = true;
  });

  return (
    <instancedMesh
      ref={meshRef}
      args={[undefined, undefined, count]}
      frustumCulled={false}
    >
      <sphereGeometry args={[1, 8, 8]} />
      <meshBasicMaterial
        color={bloomColor}
        toneMapped={false}
        transparent
        opacity={0.9}
      />
    </instancedMesh>
  );
}

export function LightSpeed({
  particleCount = 1000,
  speed = 2.4,
  lightColor = "#b026ff",
  intensity = 3.0,
  radius = 25,
  cylinderLength = 150,
  className,
}: LightSpeedProps) {
  return (
    <div
      className={cn(
        "absolute inset-0 w-full h-full pointer-events-none overflow-hidden bg-[#05070b]",
        className
      )}
    >
      {/* 
        Camera looks down -Z axis. FOV 90 for a wider warping look.
        It simulates a camera traveling in a cylinder (as per the Ducky3D video). 
      */}
      <Canvas camera={{ position: [0, 0, 5], fov: 90 }} dpr={[1, 2]}>
        {/* The fog acts as the "Volume cube" from the tutorial to fade out clipping edges in the distance */}
        <fogExp2 attach="fog" args={["#000000", 0.025]} />
        <color attach="background" args={["#000000"]} />

        <Particles
          count={particleCount}
          baseSpeed={speed}
          lightColor={lightColor}
          intensity={intensity}
          radius={radius}
          cylinderLength={cylinderLength}
        />

        <EffectComposer>
          {/* Bloom takes any value > 1 and makes it emit a neon outer glow */}
          <Bloom
            luminanceThreshold={0.1}
            luminanceSmoothing={0.9}
            intensity={1.5}
            mipmapBlur
          />
        </EffectComposer>
      </Canvas>
    </div>
  );
}

export default LightSpeed;
```

---

### Liquid Chrome <a name="liquid-chrome"></a>

A smooth, mesmerizing liquid metal shader effect.

- **Registry Name**: `liquid-chrome`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/liquid-chrome.json
```

#### Dependencies

- **NPM Packages**:
  - `three`
  - `@react-three/fiber`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/liquid-chrome/liquid-chrome.tsx`

```tsx
"use client";

import { Canvas, useFrame } from "@react-three/fiber";
import { useRef, useMemo } from "react";
import * as THREE from "three";
import { cn } from "@/lib/utils";

const vertexShader = `
  varying vec2 vUv;
  void main() {
    vUv = uv;
    gl_Position = vec4(position, 1.0);
  }
`;

const fragmentShader = `
  uniform float uTime;
  uniform float uTimeScale;
  uniform vec3 uColor;
  uniform vec3 uColor2;
  varying vec2 vUv;

  // Simplex 2D noise
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
    vec3 m = max(0.5 - vec3(dot(x0,x0), dot(x12.xy,x12.xy),
      dot(x12.zw,x12.zw)), 0.0);
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

  void main() {
    vec2 uv = vUv;
    
    // Domain Warping for "Silk/Liquid Metal" look
    // Layer 1: Base Flow
    vec2 q = vec2(0.0);
    q.x = snoise(uv * 1.5 + uTime * uTimeScale);
    q.y = snoise(uv * 1.5 + uTime * (uTimeScale * 1.2));
    
    // Layer 2: Distortion
    vec2 r = vec2(0.0);
    
    r.x = snoise(uv * 2.0 + 1.0 * q + vec2(1.7, 9.2) + uTime * (uTimeScale * 1.5));
    r.y = snoise(uv * 2.0 + 1.0 * q + vec2(8.3, 2.8) + uTime * (uTimeScale * 1.3));
    
    // Final Noise Value (The height/pattern)
    float f = snoise(uv * 3.0 + r);
    
    // Mix factor - swirl the two colors together
    float mixFactor = smoothstep(-1.0, 1.0, f + q.x); // Based on flow height
    
    vec3 col = mix(uColor, uColor2, mixFactor);
    
    // Bump Map / Normals for Lighting
    float eps = 0.001;
    float fx = snoise(uv * 3.0 + r + vec2(eps, 0.0)) - f;
    float fy = snoise(uv * 3.0 + r + vec2(0.0, eps)) - f;
    vec3 normal = normalize(vec3(fx * 20.0, fy * 20.0, 1.0));
    
    // Lighting
    vec3 lightPos = vec3(0.5, 0.5, 2.0);
    vec3 lightDir = normalize(lightPos);
    
    // Diffuse
    float diff = max(dot(normal, lightDir), 0.0);
    
    // Specular (High gloss)
    vec3 viewDir = vec3(0.0, 0.0, 1.0);
    vec3 reflectDir = reflect(-lightDir, normal);
    float spec = pow(max(dot(viewDir, reflectDir), 0.0), 64.0);
    
    // Fresnel / Rim Light
    float fresnel = pow(1.0 - max(dot(normal, viewDir), 0.0), 4.0);
    
    // Combine Lighting
    col = col * (0.6 + 0.4 * diff); // Base color
    col += vec3(1.0) * spec * 0.8; // White intense specular
    col += uColor2 * fresnel * 1.0; // Glowing rim using the secondary color
    
    // Tone mapping to prevent washout
    col = col / (1.0 + col * 0.2);

    gl_FragColor = vec4(col, 1.0);
  }
`;

const LiquidEffect = ({
  speed = 0.35,
  timeScale = 0.225,
  color = "#C0C0C0",
  color2 = "#4A4A4A",
}: {
  speed?: number;
  timeScale?: number;
  color?: string;
  color2?: string;
}) => {
  const material = useRef<THREE.ShaderMaterial>(null);

  const uniforms = useMemo(
    () => ({
      uTime: { value: 0 },
      uTimeScale: { value: timeScale },
      uColor: { value: new THREE.Color(color) },
      uColor2: { value: new THREE.Color(color2) },
    }),

    []
  );

  useFrame((state) => {
    if (material.current) {
      material.current.uniforms.uTime.value =
        state.clock.getElapsedTime() * speed;
      material.current.uniforms.uTimeScale.value = timeScale;
      material.current.uniforms.uColor.value.set(color);
      material.current.uniforms.uColor2.value.set(color2);
    }
  });

  return (
    <mesh>
      <planeGeometry args={[2, 2]} />
      <shaderMaterial
        ref={material}
        vertexShader={vertexShader}
        fragmentShader={fragmentShader}
        uniforms={uniforms}
      />
    </mesh>
  );
};

interface LiquidChromeProps {
  className?: string;
  speed?: number; // Overall speed of the animation
  timeScale?: number; // Scale of the time-based noise movement
  color?: string;
  color2?: string;
}

export default function LiquidChrome({
  className,
  speed = 0.35,
  timeScale = 0.225,
  color = "#C0C0C0",
  color2 = "#4A4A4A",
}: LiquidChromeProps) {
  return (
    <div
      className={cn(
        "absolute inset-0 w-full h-full pointer-events-none",
        className
      )}
    >
      <Canvas camera={{ position: [0, 0, 1] }} dpr={[1, 2]}>
        <LiquidEffect
          speed={speed}
          timeScale={timeScale}
          color={color}
          color2={color2}
        />
      </Canvas>
    </div>
  );
}
```

---

### Nebula <a name="nebula"></a>

A deep space nebula effect with fractional distortion.

- **Registry Name**: `nebula`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/nebula.json
```

#### Dependencies

- **NPM Packages**:
  - `three`
  - `@react-three/fiber`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/nebula/nebula.tsx`

```tsx
"use client";

import React, { useRef, useMemo } from "react";
import { Canvas, useFrame, useThree } from "@react-three/fiber";
import * as THREE from "three";
import { cn } from "@/lib/utils";

const vertexShader = `
  varying vec2 vUv;
  void main() {
    vUv = uv;
    gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
  }
`;

const fragmentShader = `
uniform float uTime;
uniform vec3 uColor1;
uniform vec3 uColor2;
uniform vec3 uColor3;
uniform float uSpeed;

varying vec2 vUv;

// 2D Random
float random(in vec2 st) {
    return fract(sin(dot(st.xy, vec2(12.9898, 78.233))) * 43758.5453123);
}

// 2D Noise based on Morgan McGuire @morgan3d
// https://www.shadertoy.com/view/4dS3Wd
float noise(in vec2 st) {
    vec2 i = floor(st);
    vec2 f = fract(st);

    // Four corners in 2D of a tile
    float a = random(i);
    float b = random(i + vec2(1.0, 0.0));
    float c = random(i + vec2(0.0, 1.0));
    float d = random(i + vec2(1.0, 1.0));

    // Smooth Interpolation

    // Cubic Hermine Curve.  Same as SmoothStep()
    vec2 u = f * f * (3.0 - 2.0 * f);
    // u = smoothstep(0.,1.,f);

    // Mix 4 coorners percentages
    return mix(a, b, u.x) +
        (c - a) * u.y * (1.0 - u.x) +
        (d - b) * u.x * u.y;
}

#define OCTAVES 6
float fbm(in vec2 st) {
    // Initial values
    float value = 0.0;
    float amplitude = .5;
    float frequency = 0.;
    //
    // Loop of octaves
    for (int i = 0; i < OCTAVES; i++) {
        value += amplitude * noise(st);
        st *= 2.;
        amplitude *= .5;
    }
    return value;
}

void main() {
    vec2 st = vUv * 3.0;
    float time = uTime * uSpeed;
    
    // Domain Warping
    vec2 q = vec2(0.);
    q.x = fbm(st + 0.00 * time); 
    q.y = fbm(st + vec2(1.0));

    vec2 r = vec2(0.);
    r.x = fbm(st + 1.0 * q + vec2(1.7, 9.2) + 0.15 * time); 
    r.y = fbm(st + 1.0 * q + vec2(8.3, 2.8) + 0.126 * time);

    float f = fbm(st + r);

    // Color Mixing
    vec3 color = mix(uColor3, uColor2, clamp((f * f) * 4.0, 0.0, 1.0));
    color = mix(color, uColor1, clamp(length(q), 0.0, 1.0));
    color = mix(color, vec3(1.0), clamp(length(r.x), 0.0, 1.0));

    // Darken edges
    float vignette = 1.0 - smoothstep(0.5, 1.5, length(vUv - 0.5));
    color *= vignette;


    gl_FragColor = vec4((f * f * f + .6 * f * f + .5 * f) * color, 1.0);
}
`;

const NebulaMaterial = ({
  speed,
  color1,
  color2,
  color3,
}: {
  speed: number;
  color1: string;
  color2: string;
  color3: string;
}) => {
  const materialRef = useRef<THREE.ShaderMaterial>(null);
  const lastTimeRef = useRef(0);

  const uniforms = useMemo(
    () => ({
      uTime: { value: 0 },
      uSpeed: { value: speed },
      uColor1: { value: new THREE.Color(color1) },
      uColor2: { value: new THREE.Color(color2) },
      uColor3: { value: new THREE.Color(color3) },
    }),

    [] // Initialize once
  );

  // Update uniforms when props change
  React.useEffect(() => {
    uniforms.uSpeed.value = speed;
    uniforms.uColor1.value.set(color1);
    uniforms.uColor2.value.set(color2);
    uniforms.uColor3.value.set(color3);
  }, [speed, color1, color2, color3, uniforms]);

  useFrame((state) => {
    if (!materialRef.current) return;
    // Cap render to ~30 fps — nebula moves slowly so this is imperceptible
    const elapsed = state.clock.getElapsedTime();
    if (elapsed - lastTimeRef.current < 1 / 30) return;
    lastTimeRef.current = elapsed;
    materialRef.current.uniforms.uTime.value = elapsed;
  });

  const { viewport } = useThree();

  return (
    <mesh>
      <planeGeometry args={[viewport.width, viewport.height]} />
      <shaderMaterial
        ref={materialRef}
        vertexShader={vertexShader}
        fragmentShader={fragmentShader}
        uniforms={uniforms}
      />
    </mesh>
  );
};

interface NebulaProps {
  className?: string;
  speed?: number;
  color1?: string; // Highlights/Fracture
  color2?: string; // Nebula main
  color3?: string; // Deep space
}

export default function Nebula({
  className,
  speed = 2.0,
  color1 = "#5efff4", // Cyan (Highlight)
  color2 = "#763b65", // Magenta-ish (Nebula)
  color3 = "#1a0b2e", // Deep purple (Deep Space)
}: NebulaProps) {
  return (
    <div
      className={cn(
        "absolute inset-0 w-full h-full pointer-events-none",
        className
      )}
    >
      <Canvas
        camera={{ position: [0, 0, 1] }}
        dpr={[1, 2]}
        gl={{ antialias: false, powerPreference: "high-performance" }}
      >
        <NebulaMaterial
          speed={speed}
          color3={color3}
          color2={color2}
          color1={color1}
        />
      </Canvas>
    </div>
  );
}
```

---

### Synthesis <a name="synthesis"></a>

A professional, multi-layered cosmic flow background with extensive warping customization.

- **Registry Name**: `synthesis`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/synthesis.json
```

#### Dependencies

- **NPM Packages**:
  - `three`
  - `@react-three/fiber`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/synthesis/synthesis.tsx`

```tsx
"use client";

import { Canvas, useFrame } from "@react-three/fiber";
import { useMemo, useRef, useEffect } from "react";
import * as THREE from "three";
import { cn } from "@/lib/utils";

const vertexShader = `
  varying vec2 vUv;
  void main() {
    vUv = uv;
    gl_Position = vec4(position, 1.0);
  }
`;

const fragmentShader = `
  uniform vec2 uResolution;
  uniform float uTime;
  uniform vec3 uColor1;
  uniform vec3 uColor2;
  uniform vec3 uColor3;
  uniform float uScale;
  uniform float uComplexity;
  uniform float uDistortion;
  uniform float uGlowIntensity;
  uniform float uFlowFrequency;
  uniform float uContrast;

  mat2 rot(float a) {
      float s = sin(a), c = cos(a);
      return mat2(c, -s, s, c);
  }

  void main() {
    float minRes = min(uResolution.x, uResolution.y);
    vec2 uv = (gl_FragCoord.xy * 2.0 - uResolution.xy) / minRes;
    
    vec2 p = uv * uScale;
    float t = uTime;
    
    // Multi-layered domain warping with dynamic complexity
    for(float i = 1.0; i < 20.0; i++) {
        if(i >= uComplexity) break;
        p *= rot(t * 0.08 + i * 0.15);
        p += vec2(
            sin(p.x * i + t),
            cos(p.x * i - t)
        ) * (uDistortion / i);
    }
    
    // Wave flow blending with dynamic frequency
    float flow1 = 0.5 + 0.5 * sin(p.x * (uFlowFrequency * 0.8) + t);
    float flow2 = 0.5 + 0.5 * sin(p.y * uFlowFrequency + t * 1.1);
    
    // Mix theme colors (color1, color2, color3)
    vec3 color = mix(uColor1, uColor2, flow1);
    color = mix(color, uColor3, flow2);
    
    // Core glow
    float dist = length(uv);
    float glow = exp(-dist * 1.5);
    color += uColor3 * glow * uGlowIntensity;
    
    // Dynamic contrast and saturation mapping
    color = smoothstep(0.0, uContrast, color);
    
    gl_FragColor = vec4(color, 1.0);
  }
`;

interface SynthesisProps {
  className?: string;
  speed?: number;
  color1?: string;
  color2?: string;
  color3?: string;
  scale?: number;
  complexity?: number;
  distortion?: number;
  glowIntensity?: number;
  flowFrequency?: number;
  contrast?: number;
  backgroundColor?: string;
}

const Effect = ({
  speed,
  color1,
  color2,
  color3,
  scale,
  complexity,
  distortion,
  glowIntensity,
  flowFrequency,
  contrast,
}: Required<Omit<SynthesisProps, "className" | "backgroundColor">>) => {
  const materialRef = useRef<THREE.ShaderMaterial>(null);

  const uniforms = useMemo(
    () => ({
      uTime: { value: 0 },
      uResolution: { value: new THREE.Vector2() },
      uColor1: { value: new THREE.Color(color1) },
      uColor2: { value: new THREE.Color(color2) },
      uColor3: { value: new THREE.Color(color3) },
      uScale: { value: scale },
      uComplexity: { value: complexity },
      uDistortion: { value: distortion },
      uGlowIntensity: { value: glowIntensity },
      uFlowFrequency: { value: flowFrequency },
      uContrast: { value: contrast },
    }),
    []
  );

  useEffect(() => {
    if (materialRef.current) {
      materialRef.current.uniforms.uColor1.value.set(color1);
      materialRef.current.uniforms.uColor2.value.set(color2);
      materialRef.current.uniforms.uColor3.value.set(color3);
      materialRef.current.uniforms.uScale.value = scale;
      materialRef.current.uniforms.uComplexity.value = complexity;
      materialRef.current.uniforms.uDistortion.value = distortion;
      materialRef.current.uniforms.uGlowIntensity.value = glowIntensity;
      materialRef.current.uniforms.uFlowFrequency.value = flowFrequency;
      materialRef.current.uniforms.uContrast.value = contrast;
    }
  }, [
    color1,
    color2,
    color3,
    scale,
    complexity,
    distortion,
    glowIntensity,
    flowFrequency,
    contrast,
  ]);

  useFrame((state) => {
    if (materialRef.current) {
      materialRef.current.uniforms.uTime.value =
        state.clock.getElapsedTime() * speed;
      materialRef.current.uniforms.uResolution.value.set(
        state.size.width,
        state.size.height
      );
    }
  });

  return (
    <mesh>
      <planeGeometry args={[2, 2]} />
      <shaderMaterial
        ref={materialRef}
        vertexShader={vertexShader}
        fragmentShader={fragmentShader}
        uniforms={uniforms}
      />
    </mesh>
  );
};

export default function Synthesis({
  className,
  speed = 0.4,
  color1 = "#0f172a",
  color2 = "#3b0764",
  color3 = "#0ea5e9",
  scale = 1.0,
  complexity = 6.0,
  distortion = 0.6,
  glowIntensity = 0.4,
  flowFrequency = 3.0,
  contrast = 1.2,
  backgroundColor = "#000000",
}: SynthesisProps) {
  return (
    <div
      className={cn(
        "absolute inset-0 w-full h-full pointer-events-none overflow-hidden",
        className
      )}
      style={{ backgroundColor }}
    >
      <Canvas
        camera={{ position: [0, 0, 1] }}
        dpr={1}
        gl={{ antialias: false, powerPreference: "high-performance" }}
      >
        <Effect
          speed={speed}
          color1={color1}
          color2={color2}
          color3={color3}
          scale={scale}
          complexity={complexity}
          distortion={distortion}
          glowIntensity={glowIntensity}
          flowFrequency={flowFrequency}
          contrast={contrast}
        />
      </Canvas>
    </div>
  );
}
```

---

### Water Caustic <a name="water-caustic"></a>

A stunning, tileable water caustic lighting shader using wave interference.

- **Registry Name**: `water-caustic`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/water-caustic.json
```

#### Dependencies

- **NPM Packages**:
  - `three`
  - `@react-three/fiber`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/water-caustic/water-caustic.tsx`

```tsx
"use client";

import { useRef, useMemo, useEffect } from "react";
import { Canvas, useFrame } from "@react-three/fiber";
import * as THREE from "three";
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}

const vertexShader = `
varying vec2 vUv;
void main() {
  vUv = uv;
  gl_Position = vec4(position, 1.0);
}
`;

const fragmentShader = `
uniform float iTime;
uniform vec2 iResolution;
uniform vec3 uColor;
varying vec2 vUv;

// Tileable 2D Hash function
vec2 hash2(vec2 p, float period) {
    p = mod(p, period);
    p = vec2(dot(p, vec2(127.1, 311.7)), dot(p, vec2(269.5, 183.3)));
    return fract(sin(p) * 43758.5453123);
}

// Tileable Voronoi calculating F2 - F1 for sharp caustic ridges
float causticPattern(vec2 p, float period, float t) {
    vec2 n = floor(p);
    vec2 f = fract(p);
    
    float F1 = 8.0;
    float F2 = 8.0;
    
    for (int j = -1; j <= 1; j++) {
        for (int i = -1; i <= 1; i++) {
            vec2 g = vec2(float(i), float(j));
            vec2 o = hash2(n + g, period);
            
            // Animate with constrained jitter to prevent cell-crossing artifacts
            o = 0.5 + 0.4 * sin(t + 6.2831853 * o);
            
            vec2 r = g + o - f;
            float d = dot(r, r);
            
            if (d < F1) {
                F2 = F1;
                F1 = d;
            } else if (d < F2) {
                F2 = d;
            }
        }
    }
    return F2 - F1;
}

void mainImage(out vec4 fragColor, in vec2 fragCoord) {
    // Normalize coordinates
    vec2 uv = fragCoord.xy / iResolution.xy;
    
    // Grid scale and matching period for perfect tiling
    float period = 4.0;
    vec2 p = uv * period;
    
    // Time scaling for fluid motion
    float t = iTime * 1.0;
    
    // Layer 1: Base large caustics
    float c1 = causticPattern(p, period, t);
    
    // Layer 2: Smaller, faster overlapping caustics
    float c2 = causticPattern(p * 5.0, period * 5.0, t * 1.4);
    
    // Layer 3: Micro details
    float c3 = causticPattern(p * 4.0, period * 4.0, t * 0.7);
    
    // Combine layers beautifully
    // We invert the result (1.0 - c) to make the ridges bright and centers dark
    float caustics = 0.0;
    caustics += (1.0 - c1) * 0.6;
    caustics += (1.0 - c2) * 0.3;
    caustics += (1.0 - c3) * 0.1;
    
    // Sharpen the highlights more gently for a softer, organic look
    caustics = pow(caustics, 3.0);
    
    // Enhance contrast without harsh clipping
    caustics = smoothstep(0.3, 1.0, caustics) * 1.2;
    
    // Use the dynamic unified color from React props
    // A pure, luminous color works perfectly to give a high-end, clean aesthetic over dark backgrounds
    vec3 baseColor = uColor;
    
    vec3 color = baseColor * caustics;
    
    // Softly glow the core of the caustics 
    // using a highly reduced power multiplier so it doesn't get overly intense
    color += baseColor * vec3(0.5) * pow(caustics, 2.0);
    
    // Gentle alpha mapping for subtle translucent blending
    float alpha = clamp(caustics * 0.9, 0.0, 1.0);
    
    fragColor = vec4(color, alpha);
}

void main() {
    mainImage(gl_FragColor, vUv * iResolution);
}
`;

const ShaderPlane = ({ color }: { color: string }) => {
  const materialRef = useRef<THREE.ShaderMaterial>(null);

  const uniforms = useMemo(
    () => ({
      iTime: { value: 0 },
      iResolution: { value: new THREE.Vector2() },
      uColor: { value: new THREE.Color(color) },
    }),
    [color]
  );

  useEffect(() => {
    uniforms.uColor.value.set(color);
  }, [color, uniforms]);

  useFrame((state) => {
    if (materialRef.current) {
      materialRef.current.uniforms.iTime.value = state.clock.elapsedTime;
      materialRef.current.uniforms.iResolution.value.set(
        state.size.width * state.viewport.dpr,
        state.size.height * state.viewport.dpr
      );
    }
  });

  return (
    <mesh>
      <planeGeometry args={[2, 2]} />
      <shaderMaterial
        ref={materialRef}
        vertexShader={vertexShader}
        fragmentShader={fragmentShader}
        uniforms={uniforms}
        depthWrite={false}
        depthTest={false}
        transparent={true}
        blending={THREE.AdditiveBlending}
      />
    </mesh>
  );
};

export interface WaterCausticProps {
  className?: string;
  color?: string;
}

export default function WaterCaustic({
  className,
  color = "#ffffff",
}: WaterCausticProps) {
  return (
    <div
      className={cn(
        "w-full h-full absolute inset-0 pointer-events-none",
        className
      )}
    >
      <Canvas
        camera={{ position: [0, 0, 1] }}
        gl={{ antialias: false, alpha: true }}
        dpr={[1, 2]}
      >
        <ShaderPlane color={color} />
      </Canvas>
    </div>
  );
}
```

---

### Waves <a name="waves"></a>

A smooth, shader-based wave animation component.

- **Registry Name**: `waves`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/waves.json
```

#### Dependencies

- **NPM Packages**:
  - `three`
  - `@react-three/fiber`
  - `@react-three/drei`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/waves/waves.tsx`

```tsx
"use client";

import React, { useRef, useMemo } from "react";
import {
  Canvas,
  useFrame,
  extend,
  ThreeElement,
  useThree,
} from "@react-three/fiber";
import { shaderMaterial } from "@react-three/drei";
import * as THREE from "three";

import { cn } from "@/lib/utils";

const NOISE_GLSL = `
  vec3 mod289(vec3 x) { return x - floor(x * (1.0 / 289.0)) * 289.0; }
  vec4 mod289(vec4 x) { return x - floor(x * (1.0 / 289.0)) * 289.0; }
  vec4 permute(vec4 x) { return mod289(((x*34.0)+1.0)*x); }
  vec4 taylorInvSqrt(vec4 r) { return 1.79284291400159 - 0.85373472095314 * r; }

  float snoise(vec3 v) {
    const vec2 C = vec2(1.0/6.0, 1.0/3.0);
    const vec4 D = vec4(0.0, 0.5, 1.0, 2.0);
    vec3 i  = floor(v + dot(v, C.yyy));
    vec3 x0 = v - i + dot(i, C.xxx);
    vec3 g = step(x0.yzx, x0.xyz);
    vec3 l = 1.0 - g;
    vec3 i1 = min(g.xyz, l.zxy);
    vec3 i2 = max(g.xyz, l.zxy);
    vec3 x1 = x0 - i1 + C.xxx;
    vec3 x2 = x0 - i2 + C.yyy;
    vec3 x3 = x0 - D.yyy;
    i = mod289(i);
    vec4 p = permute(permute(permute(i.z + vec4(0.0, i1.z, i2.z, 1.0)) + i.y + vec4(0.0, i1.y, i2.y, 1.0)) + i.x + vec4(0.0, i1.x, i2.x, 1.0));
    vec4 j = p - 49.0 * floor(p * (1.0/49.0));
    vec4 x_ = floor(j * (1.0/7.0));
    vec4 y_ = floor(j - 7.0 * x_);
    vec4 x = x_ * (1.0/7.0) + 0.5/7.0;
    vec4 y = y_ * (1.0/7.0) + 0.5/7.0;
    vec4 h = 1.0 - abs(x) - abs(y);
    vec4 b0 = vec4(x.xy, y.xy);
    vec4 b1 = vec4(x.zw, y.zw);
    vec4 s0 = floor(b0)*2.0 + 1.0;
    vec4 s1 = floor(b1)*2.0 + 1.0;
    vec4 sh = -step(h, vec4(0.0));
    vec4 a0 = b0.xzyw + s0.xzyw*sh.xxyy;
    vec4 a1 = b1.xzyw + s1.xzyw*sh.zzww;
    vec3 p0 = vec3(a0.xy, h.x);
    vec3 p1 = vec3(a0.zw, h.y);
    vec3 p2 = vec3(a1.xy, h.z);
    vec3 p3 = vec3(a1.zw, h.w);
    vec4 norm = taylorInvSqrt(vec4(dot(p0,p0), dot(p1,p1), dot(p2,p2), dot(p3,p3)));
    p0 *= norm.x; p1 *= norm.y; p2 *= norm.z; p3 *= norm.w;
    vec4 m = max(0.6 - vec4(dot(x0,x0), dot(x1,x1), dot(x2,x2), dot(x3,x3)), 0.0);
    m = m * m;
    return 42.0 * dot(m*m, vec4(dot(p0,x0), dot(p1,x1), dot(p2,x2), dot(p3,x3)));
  }
`;

const WavesMaterial = shaderMaterial(
  {
    u_time: 0,
    u_color_1: new THREE.Color("#071697"),
    u_color_2: new THREE.Color("#00d4ff"),
    u_color_3: new THREE.Color("#000000"),
    u_speed_x: 0.1,
    u_speed_y: 0.1,
    u_amp: 1.5,
    u_noise_freq: 0.2,
  },
  // Vertex Shader
  NOISE_GLSL +
    `
  varying float vNoise;
  varying vec2 vUv;
  uniform float u_time;
  uniform float u_speed_x;
  uniform float u_speed_y;
  uniform float u_amp;
  uniform float u_noise_freq;

  void main() {
    vUv = uv;
    
    // Directional flow
    vec3 noisePos = vec3(position.xy * u_noise_freq - vec2(u_time * u_speed_x, 0.0), u_time * u_speed_y);
    
    float noise = snoise(noisePos);
    vNoise = noise;
    
    vec3 pos = position;
    pos.z += noise * u_amp; 
    
    gl_Position = projectionMatrix * modelViewMatrix * vec4(pos, 1.0);
  }
  `,
  // Fragment Shader
  `
  varying vec2 vUv;
  varying float vNoise;
  uniform vec3 u_color_1;
  uniform vec3 u_color_2;
  uniform vec3 u_color_3;
  
  void main() {
    float n = vNoise * 0.5 + 0.5;
    
    vec3 color;
    
    // Low (Valley) -> Mid -> High (Peak)
    if (n < 0.5) {
      color = mix(u_color_3, u_color_1, smoothstep(0.0, 0.5, n));
    } else {
      color = mix(u_color_1, u_color_2, smoothstep(0.6, 1.0, n));
    }
    
    gl_FragColor = vec4(color, 1.0);
  }
  `
);

extend({ WavesMaterial });

declare module "@react-three/fiber" {
  interface ThreeElements {
    wavesMaterial: ThreeElement<typeof WavesMaterial>;
  }
}

function Scene({
  waveColor1,
  waveColor2,
  waveColor3,
  waveSpeedX,
  waveSpeedY,
  waveAmpX,
}: {
  waveColor1: string;
  waveColor2: string;
  waveColor3: string;
  waveSpeedX: number;
  waveSpeedY: number;
  waveAmpX: number;
}) {
  const matRef = useRef<
    THREE.ShaderMaterial & {
      u_time: number;
      u_color_1: THREE.Color;
      u_color_2: THREE.Color;
      u_color_3: THREE.Color;
      u_speed_x: number;
      u_speed_y: number;
      u_amp: number;
      u_noise_freq: number;
    }
  >(null);

  const { viewport } = useThree();

  useFrame((state) => {
    if (matRef.current) {
      matRef.current.u_time = state.clock.getElapsedTime();
      matRef.current.u_color_1.set(waveColor1);
      matRef.current.u_color_2.set(waveColor2);
      matRef.current.u_color_3.set(waveColor3);
      matRef.current.u_speed_x = waveSpeedX;
      matRef.current.u_speed_y = waveSpeedY;
      matRef.current.u_amp = (waveAmpX / 32.0) * 1.5;
      // Hardcoded freq for now as xGap is removed
      matRef.current.u_noise_freq = 0.25;
    }
  });

  const planeArgs = useMemo<[number, number, number, number]>(
    () => [viewport.width * 1.5, viewport.height * 1.5, 128, 128],
    [viewport.width, viewport.height]
  );

  return (
    <mesh rotation={[-0.2, 0, 0]}>
      <planeGeometry args={planeArgs} />
      <wavesMaterial ref={matRef} key={WavesMaterial.key} transparent={false} />
    </mesh>
  );
}

interface WavesProps {
  className?: string;
  backgroundColor?: string;
  waveColor1?: string;
  waveColor2?: string;
  waveColor3?: string;
  waveSpeedX?: number;
  waveSpeedY?: number;
  waveAmpX?: number;
}

export function Waves({
  className,
  backgroundColor = "#000000",
  waveColor1 = "#071697",
  waveColor2 = "#00d4ff",
  waveColor3 = "#000000",
  waveSpeedX = 0.0125,
  waveSpeedY = 0.005,
  waveAmpX = 32,
}: WavesProps) {
  return (
    <div
      className={cn(
        "absolute inset-0 w-full h-full pointer-events-none z-0",
        className
      )}
    >
      <Canvas camera={{ position: [0, 0, 4] }}>
        <color attach="background" args={[backgroundColor]} />
        <Scene
          waveColor1={waveColor1}
          waveColor2={waveColor2}
          waveColor3={waveColor3}
          waveSpeedX={waveSpeedX * 10.0}
          waveSpeedY={waveSpeedY * 10.0}
          waveAmpX={waveAmpX}
        />
      </Canvas>
    </div>
  );
}

export default Waves;
```

---

## Interactive UI Components <a name="interactive-ui-components"></a>

### AI Input <a name="ai-input"></a>

A polished AI input component with model selection, tools, file uploads, and smooth animations.

- **Registry Name**: `ai-input`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/ai-input.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `clsx`
  - `tailwind-merge`
  - `lucide-react`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/ai-input/ai-input.tsx`

```tsx
"use client";

import { cn } from "@/lib/utils";
import { m, LazyMotion, domMax, AnimatePresence } from "motion/react";
import Image from "next/image";
import React, {
  useState,
  useRef,
  useEffect,
  createContext,
  useContext,
} from "react";
import {
  Mic,
  ArrowUp,
  Sparkles,
  ChevronDown,
  X,
  Plus,
  Check,
  Globe,
  Video,
  Image as ImageIcon,
  Layout,
  BookOpen,
  Paperclip,
  File,
  Square,
  LucideIcon,
} from "lucide-react";

// =============================================================================
// TYPE DEFINITIONS
// =============================================================================

type IconComponent = React.ComponentType<{ className?: string }>;

interface AIInputContextType {
  activeDropdown: "plus" | "tools" | "model" | null;
  setActiveDropdown: (dropdown: "plus" | "tools" | "model" | null) => void;
}

interface Model {
  id: string;
  name: string;
  label: string;
  icon: LucideIcon;
}

interface MenuItem {
  id: string;
  icon: LucideIcon;
  label: string;
}

interface ToolItem {
  icon: LucideIcon;
  label: string;
}

interface Attachment {
  preview: string;
  type: "image" | "file" | "video";
}

interface Message {
  id: string;
  role: "user" | "ai";
  content: string;
  attachments?: Attachment[];
}

interface UploadedFile {
  id: string;
  file: File;
  preview: string;
  type: "image" | "file" | "video";
}

// =============================================================================
// CONSTANTS & DEFAULTS
// =============================================================================

const DEFAULT_MODELS: Model[] = [
  { id: "gpt4o", name: "GPT-4o", label: "GPT-4o", icon: Sparkles },
  { id: "gpt4", name: "GPT-4", label: "GPT-4", icon: Sparkles },
  { id: "claude", name: "Claude 3.5", label: "Claude 3.5", icon: Sparkles },
  {
    id: "claude-opus",
    name: "Claude 4.5 Opus",
    label: "Claude 4.5 Opus",
    icon: Sparkles,
  },
];

const DEFAULT_PLUS_MENU: MenuItem[] = [
  { id: "files", icon: Paperclip, label: "Upload photos & files" },
  { id: "videos", icon: Video, label: "Upload Videos" },
];

const DEFAULT_TOOLS: ToolItem[] = [
  { icon: Globe, label: "Deep Research" },
  { icon: Video, label: "Create videos" },
  { icon: ImageIcon, label: "Create images" },
  { icon: Layout, label: "Canvas" },
  { icon: BookOpen, label: "Guided Learning" },
];

// =============================================================================
// CONTEXT
// =============================================================================

const AIInputContext = createContext<AIInputContextType | undefined>(undefined);

export const useAIInput = () => {
  const context = useContext(AIInputContext);
  if (!context) {
    throw new Error("useAIInput must be used within an AIInput component");
  }
  return context;
};

// =============================================================================
// DROPDOWN COMPONENT
// =============================================================================

interface DropdownItem {
  icon?: IconComponent;
  label: string;
  onClick?: () => void;
}

interface AIInputDropdownProps<T> {
  isOpen: boolean;
  onClose: () => void;
  items: T[];
  renderItem?: (item: T, index: number) => React.ReactNode;
  className?: string;
}

export function AIInputDropdown<T extends DropdownItem>({
  isOpen,
  onClose,
  items,
  renderItem,
  className,
}: AIInputDropdownProps<T>) {
  return (
    <AnimatePresence>
      {isOpen && (
        <>
          <div
            role="button"
            tabIndex={-1}
            aria-label="Dismiss"
            className="fixed inset-0 z-40 bg-transparent"
            onClick={onClose}
            onKeyDown={(e) => {
              if (e.key === "Escape") onClose();
            }}
          />
          <m.div
            initial={{ opacity: 0, scale: 0.9, y: 10 }}
            animate={{ opacity: 1, scale: 1, y: 0 }}
            exit={{ opacity: 0, scale: 0.9, y: 10 }}
            transition={{ type: "spring", duration: 0.3, bounce: 0 }}
            className={cn(
              "absolute bottom-full left-0 mb-2 bg-white dark:bg-[#1a1a1a] border border-black/5 dark:border-white/10 rounded-2xl shadow-xl overflow-hidden z-50 p-1.5",
              className
            )}
          >
            <div className="flex flex-col gap-0.5">
              {items.map((item, index) =>
                renderItem ? (
                  <div key={item.label} role="presentation" onClick={onClose}>
                    {renderItem(item, index)}
                  </div>
                ) : (
                  <button
                    key={item.label}
                    onClick={() => {
                      item.onClick?.();
                      onClose();
                    }}
                    className="flex items-center gap-2 px-2 py-2.5 w-full text-left text-zinc-600 dark:text-zinc-300 hover:bg-zinc-100 dark:hover:bg-white/10 rounded-2xl transition-colors group"
                  >
                    {item.icon && (
                      <item.icon className="w-4 h-4 text-zinc-400 group-hover:text-zinc-600 dark:group-hover:text-zinc-200 transition-colors" />
                    )}
                    <span className="text-sm font-medium">{item.label}</span>
                  </button>
                )
              )}
            </div>
          </m.div>
        </>
      )}
    </AnimatePresence>
  );
}
AIInputDropdown.displayName = "AIInputDropdown";

// =============================================================================
// PILL BUTTON COMPONENT
// =============================================================================

interface AIInputPillButtonProps {
  children: React.ReactNode;
  isActive?: boolean;
  showChevron?: boolean;
  chevronRotated?: boolean;
  showClose?: boolean;
  onClose?: () => void;
  onClick?: () => void;
  layoutId?: string;
  className?: string;
  icon?: IconComponent;
}

export function AIInputPillButton({
  children,
  isActive = false,
  showChevron = false,
  chevronRotated = false,
  showClose = false,
  onClose,
  onClick,
  layoutId,
  className,
  icon: Icon,
}: AIInputPillButtonProps) {
  const baseStyles =
    "flex items-center gap-2 px-3 py-2 rounded-full transition-colors border cursor-pointer";
  const activeStyles =
    "bg-zinc-100 dark:bg-zinc-800 text-zinc-900 dark:text-zinc-100 border-black/10 dark:border-white/10";
  const inactiveStyles =
    "bg-zinc-50 dark:bg-zinc-900 text-zinc-600 dark:text-zinc-300 hover:bg-zinc-100 dark:hover:bg-zinc-800 border-black/5 dark:border-white/5";

  const pillContent = (
    <>
      {Icon && <Icon className="w-4 h-4 text-zinc-500" />}
      {children}
      {showChevron && (
        <ChevronDown
          className={cn(
            "w-4 h-4 text-zinc-400 transition-transform",
            chevronRotated && "rotate-180"
          )}
        />
      )}
    </>
  );

  if (showClose) {
    return (
      <m.div
        layoutId={layoutId}
        layout
        transition={{ duration: 0.3 }}
        className={cn(
          baseStyles,
          isActive ? activeStyles : inactiveStyles,
          className
        )}
      >
        <button
          onClick={onClick}
          className="flex items-center gap-2 cursor-pointer"
        >
          {pillContent}
        </button>
        <button
          onClick={(e) => {
            e.stopPropagation();
            onClose?.();
          }}
          className="ml-1 p-0.5 rounded-full bg-zinc-200 dark:bg-zinc-700 text-zinc-500 dark:text-zinc-400 flex items-center justify-center transition-colors hover:bg-zinc-300 dark:hover:bg-zinc-600 cursor-pointer"
        >
          <X className="w-3 h-3" />
        </button>
      </m.div>
    );
  }

  return (
    <m.button
      layoutId={layoutId}
      layout
      onClick={onClick}
      transition={{ duration: 0.3 }}
      className={cn(
        baseStyles,
        isActive ? activeStyles : inactiveStyles,
        className
      )}
    >
      {pillContent}
    </m.button>
  );
}
AIInputPillButton.displayName = "AIInputPillButton";

// =============================================================================
// MESSAGES AREA COMPONENT
// =============================================================================

interface AIInputMessagesProps {
  messages: Message[];
  hasSubmitted: boolean;
  messagesEndRef: React.RefObject<HTMLDivElement | null>;
}

export function AIInputMessages({
  messages,
  hasSubmitted,
  messagesEndRef,
}: AIInputMessagesProps) {
  return (
    <m.div
      layout
      className={cn(
        "w-full max-w-2xl mx-auto flex flex-col gap-6 overflow-y-auto px-4 hide-scrollbar",
        hasSubmitted ? "flex-1 pt-10" : "hidden"
      )}
    >
      {hasSubmitted && (
        <>
          {messages.map((msg) => (
            <m.div
              initial={{ opacity: 0, y: 20, scale: 0.95 }}
              animate={{ opacity: 1, y: 0, scale: 1 }}
              key={msg.id}
              className={cn(
                "flex flex-col gap-2 max-w-[85%]",
                msg.role === "user" ? "ml-auto items-end" : "items-start"
              )}
            >
              {msg.attachments && msg.attachments.length > 0 && (
                <div className="flex flex-wrap gap-2 justify-end">
                  {msg.attachments.map((attachment, attachIdx) => (
                    <div key={attachIdx} className="relative">
                      {attachment.type === "image" ? (
                        <div className="relative w-20 h-20 rounded-[12px] overflow-hidden border border-black/5 dark:border-white/10">
                          <Image
                            src={attachment.preview}
                            alt="Attachment"
                            fill
                            sizes="80px"
                            className="object-cover"
                          />
                        </div>
                      ) : attachment.type === "video" ? (
                        <div className="relative w-32 h-32 rounded-lg overflow-hidden bg-zinc-200 dark:bg-zinc-700 border border-black/5 dark:border-white/10">
                          <video
                            src={attachment.preview}
                            className="w-full h-full object-cover"
                          />
                        </div>
                      ) : (
                        <div className="w-20 h-20 rounded-lg bg-zinc-100 dark:bg-zinc-800 border border-black/5 dark:border-white/10 flex items-center justify-center">
                          <File className="w-8 h-8 text-zinc-500" />
                        </div>
                      )}
                    </div>
                  ))}
                </div>
              )}
              {msg.content && (
                <div
                  className={cn(
                    "p-2 rounded-[12px]",
                    msg.role === "user"
                      ? "bg-zinc-100 dark:bg-zinc-800 text-zinc-900 dark:text-zinc-100"
                      : "text-zinc-900 dark:text-zinc-100"
                  )}
                >
                  {msg.role === "ai" && (
                    <div className="flex items-center gap-2 mb-2 text-xs font-medium text-neutral-500">
                      <Sparkles className="w-3 h-3" />
                      AI Response
                    </div>
                  )}
                  {msg.content}
                </div>
              )}
            </m.div>
          ))}
          <div className="h-24 flex-shrink-0" />
          <div ref={messagesEndRef} />
        </>
      )}
    </m.div>
  );
}
AIInputMessages.displayName = "AIInputMessages";

// =============================================================================
// FILE PREVIEW COMPONENT
// =============================================================================

interface AIInputFilePreviewProps {
  files: UploadedFile[];
  onRemove: (id: string) => void;
}

export function AIInputFilePreview({
  files,
  onRemove,
}: AIInputFilePreviewProps) {
  return (
    <AnimatePresence>
      {files.length > 0 && (
        <m.div
          layout
          initial={{ opacity: 0, height: 0 }}
          animate={{
            opacity: 1,
            height: "auto",
            transition: { ease: "easeInOut" },
          }}
          exit={{
            opacity: 0,
            height: 0,
            transition: { duration: 0.2, ease: "easeInOut" },
          }}
          className="overflow-hidden"
        >
          <div className="px-4 pt-4 pb-2 flex flex-wrap gap-2">
            {files.map((file) => (
              <m.div
                key={file.id}
                initial={{ opacity: 0, scale: 0.8 }}
                animate={{ opacity: 1, scale: 1 }}
                exit={{ opacity: 0, scale: 0.8 }}
                layout
                className="relative group/file"
              >
                {file.type === "image" ? (
                  <div className="relative w-16 h-16 rounded-[12px] overflow-hidden border border-black/5 dark:border-white/10">
                    <Image
                      src={file.preview}
                      alt={file.file.name}
                      fill
                      sizes="64px"
                      className="object-cover"
                    />
                  </div>
                ) : file.type === "video" ? (
                  <div className="relative w-16 h-16 rounded-lg overflow-hidden border border-black/5 dark:border-white/10 bg-zinc-100 dark:bg-zinc-800 flex items-center justify-center">
                    <video
                      src={file.preview}
                      className="w-full h-full object-cover"
                    />
                  </div>
                ) : (
                  <div className="w-16 h-16 rounded-lg border border-black/5 dark:border-white/10 bg-zinc-100 dark:bg-zinc-800 flex flex-col items-center justify-center gap-1 p-1">
                    <File className="w-5 h-5 text-zinc-500" />
                    <span className="text-[8px] text-zinc-500 truncate w-full text-center">
                      {file.file.name.split(".").pop()?.toUpperCase()}
                    </span>
                  </div>
                )}
                <button
                  onClick={() => onRemove(file.id)}
                  className="absolute -top-1.5 -right-1.5 w-5 h-5 rounded-full dark:bg-zinc-800 bg-zinc-100 text-zinc-500 dark:text-zinc-400 flex items-center justify-center border border-black/5 dark:border-white/10 cursor-pointer"
                >
                  <X className="w-3 h-3" />
                </button>
              </m.div>
            ))}
          </div>
        </m.div>
      )}
    </AnimatePresence>
  );
}
AIInputFilePreview.displayName = "AIInputFilePreview";

// =============================================================================
// MAIN AI INPUT COMPONENT
// =============================================================================

interface AIInputProps {
  models?: Model[];
  tools?: ToolItem[];
  plusMenuItems?: MenuItem[];
  onSubmit?: (message: string, attachments: Attachment[]) => void;
  placeholder?: string;
  className?: string;
}

export function AIInput({
  models = DEFAULT_MODELS,
  tools = DEFAULT_TOOLS,
  plusMenuItems = DEFAULT_PLUS_MENU,
  onSubmit,
  placeholder = "Ask anything...",
  className,
}: AIInputProps) {
  const [value, setValue] = useState<string>("");
  const [messages, setMessages] = useState<Message[]>([]);
  const [hasSubmitted, setHasSubmitted] = useState<boolean>(false);
  const [isListening, setIsListening] = useState<boolean>(false);
  const [selectedTool, setSelectedTool] = useState<ToolItem | null>(null);
  const [selectedModel, setSelectedModel] = useState<Model>(models[0]);
  const [uploadedFiles, setUploadedFiles] = useState<UploadedFile[]>([]);
  const [activeDropdown, setActiveDropdown] = useState<
    "plus" | "tools" | "model" | null
  >(null);

  const fileInputRef = useRef<HTMLInputElement>(null);
  const videoInputRef = useRef<HTMLInputElement>(null);
  const messagesEndRef = useRef<HTMLDivElement>(null);

  const hasText = value.length > 0;

  useEffect(() => {
    if (messagesEndRef.current) {
      messagesEndRef.current.scrollIntoView({ behavior: "smooth" });
    }
  }, [messages]);

  const handleFileSelect = (e: React.ChangeEvent<HTMLInputElement>) => {
    const files = e.target.files;
    if (!files) return;

    const newFiles: UploadedFile[] = Array.from(files).map((file) => {
      const isImage = file.type.startsWith("image/");
      const isVideo = file.type.startsWith("video/");
      return {
        id: `${Date.now()}-${Math.random().toString(36).substr(2, 9)}`,
        file,
        preview: isImage || isVideo ? URL.createObjectURL(file) : "",
        type: isVideo ? "video" : isImage ? "image" : "file",
      };
    });

    setUploadedFiles((prev) => [...prev, ...newFiles]);
    e.target.value = "";
  };

  const removeFile = (id: string) => {
    setUploadedFiles((prev) => {
      const file = prev.find((f) => f.id === id);
      if (file?.preview) URL.revokeObjectURL(file.preview);
      return prev.filter((f) => f.id !== id);
    });
  };

  const handlePlusMenuClick = (itemId: string) => {
    setActiveDropdown(null);
    if (itemId === "files") fileInputRef.current?.click();
    else if (itemId === "videos") videoInputRef.current?.click();
  };

  const handleSubmit = () => {
    if (!value.trim() && uploadedFiles.length === 0) return;

    setHasSubmitted(true);
    const attachments = uploadedFiles.map((file) => ({
      preview: file.preview,
      type: file.type,
    }));

    setMessages((prev) => [
      ...prev,
      {
        id: `msg-${Date.now()}`,
        role: "user",
        content: value,
        attachments: attachments.length > 0 ? attachments : undefined,
      },
    ]);

    if (onSubmit) {
      onSubmit(value, attachments);
    }

    setValue("");
    setUploadedFiles([]);

    // Simulate AI reply (remove in production)
    setTimeout(() => {
      setMessages((prev) => [
        ...prev,
        {
          id: `msg-${Date.now()}-ai`,
          role: "ai",
          content: `Your response content here...`,
        },
      ]);
    }, 500);
  };

  const handleKeyDown = (e: React.KeyboardEvent) => {
    if (e.key === "Enter" && !e.shiftKey) {
      e.preventDefault();
      handleSubmit();
    }
  };

  return (
    <LazyMotion features={domMax}>
      <AIInputContext.Provider value={{ activeDropdown, setActiveDropdown }}>
        <div
          className={cn(
            "w-full h-[100dvh] flex flex-col relative overflow-hidden",
            className
          )}
        >
          <AIInputMessages
            messages={messages}
            hasSubmitted={hasSubmitted}
            messagesEndRef={messagesEndRef}
          />

          <m.div
            layout
            transition={{ type: "spring", damping: 25, stiffness: 200 }}
            className={cn(
              "w-full px-4 flex flex-col z-20",
              hasSubmitted ? "pb-8" : "flex-1 justify-center items-center"
            )}
          >
            <div className="w-full max-w-2xl mx-auto relative group">
              <m.div
                layoutId="input-container"
                layout
                transition={{ duration: 0.3, ease: "easeInOut" }}
                className="relative bg-white dark:bg-[#09090b] rounded-[32px] border border-black/5 dark:border-white/5"
              >
                <input
                  ref={fileInputRef}
                  type="file"
                  multiple
                  accept="image/*,.pdf,.doc,.docx,.txt,.md"
                  className="hidden"
                  onChange={handleFileSelect}
                />
                <input
                  ref={videoInputRef}
                  type="file"
                  multiple
                  accept="video/*"
                  className="hidden"
                  onChange={handleFileSelect}
                />

                <AIInputFilePreview
                  files={uploadedFiles}
                  onRemove={removeFile}
                />

                <div className="p-4 pb-14">
                  <m.textarea
                    layout
                    transition={{ duration: 0.2, ease: "easeInOut" }}
                    value={value}
                    onChange={(e) => setValue(e.target.value)}
                    onKeyDown={handleKeyDown}
                    disabled={isListening}
                    placeholder={isListening ? "Listening..." : placeholder}
                    className="w-full bg-transparent text-lg text-zinc-900 dark:text-zinc-100 placeholder:text-zinc-400 dark:placeholder:text-zinc-500 resize-none outline-none min-h-[40px] max-h-[200px]"
                    rows={1}
                    style={{ minHeight: "44px", height: "auto" }}
                    onInput={(e) => {
                      const target = e.target as HTMLTextAreaElement;
                      target.style.height = "auto";
                      target.style.height = `${target.scrollHeight}px`;
                    }}
                  />
                </div>

                {/* Bottom Controls */}
                <div className="absolute bottom-4 left-4 right-4 flex justify-between items-center z-10">
                  {/* Left Side */}
                  <div className="flex items-center gap-2">
                    <div className="relative">
                      <button
                        onClick={() =>
                          setActiveDropdown(
                            activeDropdown === "plus" ? null : "plus"
                          )
                        }
                        className={cn(
                          "p-2.5 rounded-full transition-colors border",
                          activeDropdown === "plus"
                            ? "bg-zinc-100 dark:bg-zinc-800 text-zinc-900 dark:text-zinc-100 border-black/10 dark:border-white/10"
                            : "bg-zinc-50 dark:bg-zinc-900 text-zinc-500 dark:text-zinc-400 hover:bg-zinc-100 dark:hover:bg-zinc-800 border-black/5 dark:border-white/5"
                        )}
                      >
                        <Plus
                          className={cn(
                            "w-5 h-5 transition-transform",
                            activeDropdown === "plus" && "rotate-45"
                          )}
                        />
                      </button>
                      <AIInputDropdown
                        isOpen={activeDropdown === "plus"}
                        onClose={() => setActiveDropdown(null)}
                        items={plusMenuItems}
                        className="w-56 bottom-full left-0 mb-2"
                        renderItem={(item) => (
                          <button
                            onClick={() => handlePlusMenuClick(item.id)}
                            className="flex items-center gap-2 px-4 py-3 w-full text-left text-zinc-600 dark:text-zinc-300 hover:bg-zinc-100 dark:hover:bg-white/10 rounded-2xl transition-colors group"
                          >
                            <item.icon className="w-4 h-4 text-zinc-400 group-hover:text-zinc-600 dark:group-hover:text-zinc-200 transition-colors" />
                            <span className="text-sm font-medium">
                              {item.label}
                            </span>
                          </button>
                        )}
                      />
                    </div>

                    <div className="relative hidden sm:block">
                      {selectedTool ? (
                        <AIInputPillButton
                          layoutId="tools-pill"
                          icon={selectedTool.icon}
                          isActive={activeDropdown === "tools"}
                          showChevron
                          chevronRotated={activeDropdown === "tools"}
                          showClose
                          onClick={() =>
                            setActiveDropdown(
                              activeDropdown === "tools" ? null : "tools"
                            )
                          }
                          onClose={() => {
                            setSelectedTool(null);
                            setActiveDropdown(null);
                          }}
                        >
                          <span className="text-sm font-medium">
                            {selectedTool.label}
                          </span>
                        </AIInputPillButton>
                      ) : (
                        <AIInputPillButton
                          layoutId="tools-pill"
                          icon={Sparkles}
                          isActive={activeDropdown === "tools"}
                          showChevron
                          chevronRotated={activeDropdown === "tools"}
                          onClick={() =>
                            setActiveDropdown(
                              activeDropdown === "tools" ? null : "tools"
                            )
                          }
                        >
                          <span className="text-sm font-medium">Tools</span>
                        </AIInputPillButton>
                      )}

                      <AIInputDropdown
                        isOpen={activeDropdown === "tools"}
                        onClose={() => setActiveDropdown(null)}
                        items={tools}
                        className="w-64 bottom-full left-0 mb-2"
                        renderItem={(item) => (
                          <button
                            onClick={() => {
                              setSelectedTool(item);
                              setActiveDropdown(null);
                            }}
                            className={cn(
                              "flex items-center gap-3 px-4 py-3 w-full text-left text-zinc-600 dark:text-zinc-300 hover:bg-zinc-100 dark:hover:bg-white/10 rounded-2xl transition-colors group",
                              selectedTool?.label === item.label &&
                                "bg-zinc-100 dark:bg-zinc-800"
                            )}
                          >
                            <item.icon className="w-4 h-4 text-zinc-400 group-hover:text-zinc-600 dark:group-hover:text-zinc-200 transition-colors" />
                            <span className="text-sm font-medium">
                              {item.label}
                            </span>
                          </button>
                        )}
                      />
                    </div>
                  </div>

                  {/* Right Side */}
                  <div className="flex items-center gap-2">
                    <div className="relative">
                      <AIInputPillButton
                        layoutId="model-pill"
                        icon={selectedModel.icon}
                        isActive={activeDropdown === "model"}
                        showChevron
                        chevronRotated={activeDropdown === "model"}
                        onClick={() =>
                          setActiveDropdown(
                            activeDropdown === "model" ? null : "model"
                          )
                        }
                      >
                        <span className="text-sm font-medium">
                          {selectedModel.name}
                        </span>
                      </AIInputPillButton>

                      <AIInputDropdown
                        isOpen={activeDropdown === "model"}
                        onClose={() => setActiveDropdown(null)}
                        items={models}
                        className="w-48 bottom-full right-0 mb-2 p-1"
                        renderItem={(model) => (
                          <button
                            onClick={() => {
                              setSelectedModel(model);
                              setActiveDropdown(null);
                            }}
                            className={cn(
                              "flex items-center gap-3 px-4 py-3 w-full text-left text-zinc-600 dark:text-zinc-300 hover:bg-zinc-100 dark:hover:bg-white/10 rounded-2xl transition-colors group",
                              selectedModel.id === model.id &&
                                "bg-zinc-100 dark:bg-zinc-800"
                            )}
                          >
                            <model.icon className="w-4 h-4 text-zinc-400 group-hover:text-zinc-600 dark:group-hover:text-zinc-200 transition-colors" />
                            <span className="text-sm font-medium">
                              {model.name}
                            </span>
                            {selectedModel.id === model.id && (
                              <Check className="w-4 h-4 ml-auto text-zinc-500" />
                            )}
                          </button>
                        )}
                      />
                    </div>

                    <div className="flex justify-end">
                      <AnimatePresence mode="wait" initial={false}>
                        {hasText ? (
                          <m.div
                            key="active-controls"
                            initial={{ opacity: 0, scale: 0.9 }}
                            animate={{ opacity: 1, scale: 1 }}
                            exit={{ opacity: 0, scale: 0.9 }}
                            transition={{ duration: 0.15 }}
                            className="flex items-center gap-2"
                          >
                            <button
                              onClick={() => setValue("")}
                              className="p-2 text-zinc-400 hover:text-zinc-600 dark:text-zinc-500 dark:hover:text-zinc-300 transition-colors"
                            >
                              <X className="w-4 h-4" />
                            </button>
                            <button
                              onClick={handleSubmit}
                              className="p-2.5 rounded-full bg-zinc-900 dark:bg-zinc-100 text-white dark:text-zinc-900 hover:opacity-90 transition-opacity"
                            >
                              <ArrowUp className="w-5 h-5" />
                            </button>
                          </m.div>
                        ) : (
                          <m.div
                            key="inactive-controls"
                            initial={{ opacity: 0, scale: 0.9 }}
                            animate={{ opacity: 1, scale: 1 }}
                            exit={{ opacity: 0, scale: 0.9 }}
                            transition={{ duration: 0.15 }}
                            className="flex items-center gap-2"
                          >
                            <button
                              onClick={() => setIsListening(!isListening)}
                              className={cn(
                                "p-2 transition-all duration-300 relative cursor-pointer",
                                isListening
                                  ? "text-red-500 dark:text-red-400 bg-red-50 dark:bg-red-900/20 rounded-full"
                                  : "text-zinc-400 hover:text-zinc-600 dark:text-zinc-500 dark:hover:text-zinc-300"
                              )}
                            >
                              {isListening ? (
                                <Square
                                  className="w-4 h-4"
                                  fill="currentColor"
                                />
                              ) : (
                                <Mic className="w-4 h-4" />
                              )}
                              {isListening && (
                                <span className="absolute inset-0 rounded-full animate-ping bg-red-500/20" />
                              )}
                            </button>
                            <button
                              disabled
                              className="p-2.5 rounded-full bg-zinc-100 dark:bg-zinc-800 text-zinc-300 dark:text-zinc-600"
                            >
                              <ArrowUp className="w-4 h-4" />
                            </button>
                          </m.div>
                        )}
                      </AnimatePresence>
                    </div>
                  </div>
                </div>
              </m.div>
            </div>
          </m.div>
        </div>
      </AIInputContext.Provider>
    </LazyMotion>
  );
}
AIInput.displayName = "AIInput";

export default AIInput;
```

---

### Carousel <a name="carousel"></a>

A smooth, auto-playing 3D carousel component with layout animations.

- **Registry Name**: `carousel`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/carousel.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/carousel/carousel.tsx`

```tsx
"use client";

import Image from "next/image";
import { useEffect, useState } from "react";
import { LazyMotion, domMax, m } from "motion/react";
import { cn } from "@/lib/utils";

interface CourselProps {
  images: string[];
  className?: string;
  cardWidth?: string | number;
  cardHeight?: string | number;
  duration?: number;
  rotationAngle?: number;
}

const Coursel = ({
  images,
  className,
  cardWidth = "250px",
  cardHeight = "284px",
  duration = 0.5,
  rotationAngle = 45,
}: CourselProps) => {
  const [currentIndex, setCurrentIndex] = useState<number>(0);
  const items = [];
  const totalItems = images.length;

  // handle next func
  const handleNext = () => {
    setCurrentIndex((prevIndex) => (prevIndex + 1) % totalItems);
  };

  // useeffect to clear the interval
  useEffect(() => {
    // set interval to auto change the images every 1 sec
    const interval = setInterval(() => {
      handleNext();
    }, 3000);

    return () => clearInterval(interval);
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, []);

  // Get previous, current, and next items
  for (let i = 0; i <= 2; i++) {
    const index = (currentIndex + i + totalItems) % totalItems;
    items.push({
      item: images[index],
      index: index,
      position: i,
      isCenter: i === 1,
    });
  }

  return (
    <LazyMotion features={domMax}>
      <div
        className={cn("flex ", className)}
        style={{
          perspective: "1200px",
          transformStyle: "preserve-3d",
        }}
      >
        {items.map((item) => (
          <m.div
            key={item.item}
            layoutId={item.item}
            initial={{
              scale: 0.8,
              rotateY:
                item.position === 0
                  ? rotationAngle
                  : item.position === 2
                    ? -rotationAngle
                    : 0,
            }}
            animate={{
              scale: item.isCenter ? 1 : 0.9,
              opacity: 1,
              rotateY:
                item.position === 0
                  ? rotationAngle
                  : item.position === 2
                    ? -rotationAngle
                    : 0,
            }}
            exit={{
              scale: 0.8,
              opacity: 0,
              rotateY:
                item.position === 0
                  ? rotationAngle
                  : item.position === 2
                    ? -rotationAngle
                    : 0,
            }}
            transition={{
              duration: duration,
            }}
            style={{
              width: cardWidth,
              height: cardHeight,
            }}
            className={cn(
              item.isCenter ? "z-10" : "",
              "relative flex-shrink-0"
            )}
          >
            <Image
              src={item.item}
              alt={`image-${item.index}`}
              fill
              priority={true}
              className="object-cover rounded-[16px]"
              sizes="(max-width: 768px) 100vw, 50vw"
            />
          </m.div>
        ))}
      </div>
    </LazyMotion>
  );
};

export default Coursel;
```

---

### Dancing Letters <a name="dancing-letters"></a>

Physics-based interactive text animations with multiple animation styles.

- **Registry Name**: `dancing-letters`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/dancing-letters.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/dancing-letters/dancing-letters.tsx`

```tsx
"use client";

import { LazyMotion, domAnimation, m } from "motion/react";
import { useState, useCallback, useEffect } from "react";
import { Outfit } from "next/font/google";
import { cn } from "@/lib/utils";

export const dancingFont = Outfit({
  subsets: ["latin"],
  variable: "--font-dancing",
});

interface DancingLettersProps {
  text?: string;
  className?: string;
  letterClassName?: string;
  autoPlay?: boolean;
  autoPlayInterval?: number;
}

// Sleek, physics-based animations
const letterAnimations = [
  // 1. Rubber Band (Snap)
  {
    active: {
      scaleX: [1, 1.25, 0.75, 1.15, 0.95, 1.05, 1],
      scaleY: [1, 0.75, 1.25, 0.85, 1.05, 0.95, 1],
    },
    transition: { duration: 0.8, ease: "easeInOut" },
    transformOrigin: "center center",
  },
  // 2. The Hinge (Falling effect)
  {
    active: {
      rotate: [0, 80, 60, 80, 60, 0],
      y: [0, 10, -5, 5, -2, 0],
      originX: 0,
      originY: 1,
    },
    transition: { duration: 1.2, ease: [0.175, 0.885, 0.32, 1.275] },
    transformOrigin: "bottom left",
  },
  // 3. Squash and Jump
  {
    active: {
      scaleY: [1, 0.6, 1.2, 1],
      y: [0, 20, -40, 0],
    },
    transition: { duration: 0.6, ease: "easeOut" },
    transformOrigin: "bottom center",
  },
  // 4. Falling (Requests)
  {
    active: {
      rotateX: [0, 240, 150, 200, 175, 180, 180, 0],
      scale: [1, 1.1, 1],
    },
    transition: {
      duration: 2,
      ease: "easeOut",
      times: [0, 0.12, 0.24, 0.36, 0.48, 0.6, 0.85, 1],
    },
    transformOrigin: "50% 80%",
  },
  // 5. Elastic Slide
  {
    active: {
      x: [0, -20, 15, -10, 5, 0],
    },
    transition: { duration: 0.8, ease: "easeInOut" },
    transformOrigin: "center center",
  },
  // 6. Impact Shake
  {
    active: {
      x: [0, -5, 5, -5, 5, -2, 2, 0],
      y: [0, -2, 2, -1, 1, 0],
      rotate: [0, -1, 1, -0.5, 0.5, 0],
    },
    transition: { duration: 0.5, ease: "linear" },
    transformOrigin: "center center",
  },
  // 7. Pop (Scale)
  {
    active: {
      scale: [1, 1.4, 1],
    },
    transition: { duration: 0.5, ease: "easeInOut" },
    transformOrigin: "center center",
  },
  // 8. Levitate
  {
    active: {
      y: [0, -30, 0],
      scale: [1, 1.1, 1],
      textShadow: [
        "0px 0px 0px rgba(0,0,0,0)",
        "0px 20px 20px rgba(0,0,0,0.2)",
        "0px 0px 0px rgba(0,0,0,0)",
      ],
    },
    transition: { duration: 1.2, ease: "easeInOut" },
    transformOrigin: "center center",
  },
];

const DancingLetters = ({
  text = "ANIMATE",
  className = "",
  letterClassName = "",
}: DancingLettersProps) => {
  const [activeIndices, setActiveIndices] = useState<Set<number>>(new Set());
  const letters = text.split("");
  const [isLoaded, setIsLoaded] = useState(false);

  useEffect(() => {
    const timer = setTimeout(() => setIsLoaded(true), 1000);
    return () => clearTimeout(timer);
  }, []);

  const handleClick = useCallback((index: number) => {
    setActiveIndices((prev) => {
      const next = new Set(prev);
      if (next.has(index)) {
        next.delete(index);
      }
      // Small timeout to allow state clear if double clicking rapidly
      setTimeout(() => {
        setActiveIndices((prev) => {
          const next = new Set(prev);
          next.add(index);
          return next;
        });
      }, 10);
      return next;
    });
  }, []);

  const handleAnimationComplete = useCallback((index: number) => {
    setActiveIndices((prev) => {
      if (!prev.has(index)) return prev;
      const next = new Set(prev);
      next.delete(index);
      return next;
    });
  }, []);

  return (
    <LazyMotion features={domAnimation}>
      <m.div
        className={cn(
          "flex items-center justify-center select-none",
          dancingFont.variable,
          className
        )}
        style={{ perspective: "1000px" }}
        initial="hidden"
        animate="visible"
        variants={{
          hidden: { opacity: 0, y: 20 },
          visible: {
            opacity: 1,
            y: 0,
            transition: {
              staggerChildren: 0.05,
            },
          },
        }}
      >
        {letters.map((letter, id) => {
          const animIndex = id % letterAnimations.length;
          const anim = letterAnimations[animIndex];
          const isActive = activeIndices.has(id);

          return (
            <m.span
              key={`${letter}-${id}`}
              variants={{
                hidden: { opacity: 0, y: 20, scale: 0.8 },
                visible: {
                  opacity: 1,
                  scale: 1,
                  x: 0,
                  y: 0,
                  rotate: 0,
                  rotateX: 0,
                  rotateY: 0,
                  scaleX: 1,
                  scaleY: 1,
                  textShadow: "0px 0px 0px rgba(0,0,0,0)",
                  transition: { type: "spring", stiffness: 300, damping: 20 },
                },
                active: {
                  ...anim.active,
                  opacity: 1,
                  // @ts-expect-error transition type mismatch
                  transition: anim.transition,
                },
              }}
              animate={isActive ? "active" : isLoaded ? "visible" : undefined}
              onHoverStart={() => {
                if (!isActive) handleClick(id);
              }}
              onClick={() => handleClick(id)}
              onAnimationComplete={(definition) => {
                if (definition === "active") handleAnimationComplete(id);
              }}
              className={cn(
                "relative inline-block text-5xl md:text-7xl lg:text-8xl font-bold text-neutral-900 dark:text-neutral-100 cursor-pointer",
                letterClassName,
                isActive ? "z-10" : "z-0"
              )}
              style={{
                transformOrigin: anim.transformOrigin,
                transformStyle: "preserve-3d",
              }}
            >
              {letter}
            </m.span>
          );
        })}
      </m.div>
    </LazyMotion>
  );
};

export default DancingLetters;
```

---

### Dock <a name="dock"></a>

A navigation dock component with smooth animations and hover effects, perfect for desktop interfaces.

- **Registry Name**: `dock`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/dock.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `clsx`
  - `tailwind-merge`
  - `lucide-react`
  - `next-themes`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/dock/dock.tsx`

```tsx
"use client";

import { cn } from "@/lib/utils";

import {
  easeIn,
  easeOut,
  LazyMotion,
  domAnimation,
  m,
  AnimatePresence,
} from "motion/react";
import Link from "next/link";
import Image from "next/image";
import React, {
  useState,
  useRef,
  createContext,
  useContext,
  useEffect,
} from "react";
import { usePathname } from "next/navigation";
import { useTheme } from "next-themes";
import { X, Menu } from "lucide-react";

// Context to manage dock state
interface DockContextType {
  openDropdowns: Record<string, boolean>;
  hoveredLink: string | null;
  setHoveredLink: (href: string | null) => void;
  handleDropdownEnter: (id: string) => void;
  handleDropdownLeave: (id: string) => void;
  activePage?: string;
  isDark: boolean;
}

const DockContext = createContext<DockContextType | undefined>(undefined);

const useDock = () => {
  const context = useContext(DockContext);
  if (!context) {
    throw new Error("useDock must be used within a Dock component");
  }
  return context;
};

interface DockProps {
  children: React.ReactNode;
  closeDelay?: number;
  bottomOffset?: string;
  activePage?: string;
  className?: string;
}

export const Dock = ({
  children,
  closeDelay = 100,
  bottomOffset = "60px",
  activePage,
  className,
}: DockProps) => {
  const [openDropdowns, setOpenDropdowns] = useState<Record<string, boolean>>(
    {}
  );
  const closeTimeoutsRef = useRef<Record<string, NodeJS.Timeout | null>>({});
  const [hoveredLink, setHoveredLink] = useState<string | null>(null);
  const [isMobileMenuOpen, setIsMobileMenuOpen] = useState(false);

  const { theme } = useTheme();
  const isDark = theme === "dark";

  const handleDropdownEnter = (id: string): void => {
    if (closeTimeoutsRef.current[id]) {
      clearTimeout(closeTimeoutsRef.current[id]!);
    }
    setOpenDropdowns((prev) => ({ ...prev, [id]: true }));
  };

  const handleDropdownLeave = (id: string): void => {
    closeTimeoutsRef.current[id] = setTimeout(() => {
      setOpenDropdowns((prev) => ({ ...prev, [id]: false }));
      setHoveredLink(null);
    }, closeDelay);
  };

  // Prevent scrolling when mobile menu is open
  useEffect(() => {
    if (isMobileMenuOpen) {
      document.body.style.overflow = "hidden";
    } else {
      document.body.style.overflow = "unset";
    }
    return () => {
      document.body.style.overflow = "unset";
    };
  }, [isMobileMenuOpen]);

  return (
    <LazyMotion features={domAnimation}>
      <DockContext.Provider
        value={{
          openDropdowns,
          hoveredLink,
          setHoveredLink,
          handleDropdownEnter,
          handleDropdownLeave,
          activePage,
          isDark,
        }}
      >
        <div className="w-full">
          {/* Desktop Dock */}
          <m.nav
            className="fixed bottom-[60px] left-0 w-full z-50 hidden md:block"
            style={{ bottom: bottomOffset }}
          >
            <div className="px-4 flex justify-center">
              <m.div
                className={cn(
                  "relative flex flex-col items-center justify-center overflow-hidden backdrop-blur-md bg-white dark:bg-black/50 border border-[#E0E0E0] dark:border-neutral-700 p-[3px] rounded-[25px]",
                  className
                )}
                transition={{ duration: 0.2 }}
              >
                {/* Dropdown Contents */}
                {React.Children.map(children, (child) => {
                  if (
                    React.isValidElement(child) &&
                    (child.type as { displayName?: string }).displayName ===
                      "DockItem"
                  ) {
                    return React.cloneElement(
                      child as React.ReactElement<DockItemProps>,
                      { renderType: "content" }
                    );
                  }
                  return null;
                })}

                {/* Navigation Items */}
                <div className="flex items-center gap-[3px] relative z-10">
                  {React.Children.map(children, (child) => {
                    if (React.isValidElement(child)) {
                      return React.cloneElement(
                        child as React.ReactElement<
                          DockItemProps | DockIconProps | DockLinkProps
                        >,
                        { renderType: "trigger" }
                      );
                    }
                    return null;
                  })}
                </div>
              </m.div>
            </div>
          </m.nav>

          {/* Mobile Dock */}
          <div className="md:hidden fixed bottom-8 left-0 w-full z-50 flex justify-center pointer-events-none">
            <div className="pointer-events-auto">
              <AnimatePresence>
                {isMobileMenuOpen && (
                  <m.div
                    initial={{ opacity: 0, y: 20 }}
                    animate={{ opacity: 1, y: 0 }}
                    exit={{ opacity: 0, y: 20 }}
                    transition={{ duration: 0.2 }}
                    className="fixed inset-0 bg-white dark:bg-black z-40 flex flex-col pt-20 px-6 pb-32 overflow-y-auto"
                  >
                    <div className="flex flex-col gap-6">
                      {React.Children.map(children, (child) => {
                        if (!React.isValidElement(child)) return null;

                        // Handle DockLink (Top level links)
                        // eslint-disable-next-line @typescript-eslint/no-explicit-any
                        if ((child.type as any).displayName === "DockLink") {
                          const props = child.props as DockLinkProps;
                          return (
                            <Link
                              href={props.href}
                              className="text-black dark:text-white text-2xl font-medium"
                              onClick={() => setIsMobileMenuOpen(false)}
                            >
                              {props.label}
                            </Link>
                          );
                        }

                        // Handle DockItem (Sections with dropdowns)
                        // eslint-disable-next-line @typescript-eslint/no-explicit-any
                        if ((child.type as any).displayName === "DockItem") {
                          const props = child.props as DockItemProps;
                          return (
                            <div className="flex flex-col gap-4">
                              <span className="text-neutral-500 dark:text-neutral-400 text-lg">
                                {props.label}
                              </span>
                              <div className="flex flex-col gap-4 pl-4 border-l border-neutral-200 dark:border-neutral-800">
                                {React.Children.map(
                                  props.children,
                                  (subChild) => {
                                    if (
                                      React.isValidElement(subChild) &&
                                      // eslint-disable-next-line @typescript-eslint/no-explicit-any
                                      (subChild.type as any).displayName ===
                                        "DockDropdownItem"
                                    ) {
                                      const subProps =
                                        subChild.props as DockDropdownItemProps;
                                      return (
                                        <Link
                                          href={subProps.href}
                                          className="text-black dark:text-white text-xl font-medium flex items-center gap-3"
                                          onClick={() =>
                                            setIsMobileMenuOpen(false)
                                          }
                                        >
                                          {subProps.image && (
                                            <Image
                                              src={subProps.image}
                                              alt=""
                                              width={32}
                                              height={32}
                                              className="rounded-lg object-cover"
                                            />
                                          )}
                                          {subProps.label}
                                        </Link>
                                      );
                                    }
                                    return null;
                                  }
                                )}
                              </div>
                            </div>
                          );
                        }
                        return null;
                      })}
                    </div>
                  </m.div>
                )}
              </AnimatePresence>

              <button
                onClick={() => setIsMobileMenuOpen(!isMobileMenuOpen)}
                className={cn(
                  "flex items-center gap-2 px-6 py-3 rounded-full shadow-lg transition-all duration-300 relative z-50",
                  isMobileMenuOpen
                    ? "bg-transparent border border-black dark:border-white text-black dark:text-white"
                    : "bg-white dark:bg-neutral-900 text-black dark:text-white border border-neutral-200 dark:border-neutral-800"
                )}
              >
                <span className="font-medium text-lg">Menu</span>
                {isMobileMenuOpen ? <X size={20} /> : <Menu size={20} />}
              </button>
            </div>
          </div>
        </div>
      </DockContext.Provider>
    </LazyMotion>
  );
};

interface DockItemProps {
  children: React.ReactNode;
  label: string;
  id?: string;
  renderType?: "content" | "trigger";
  className?: string;
}

export const DockItem = ({
  children,
  label,
  id,
  renderType,
  className,
}: DockItemProps) => {
  const {
    openDropdowns,
    handleDropdownEnter,
    handleDropdownLeave,
    isDark,
    activePage,
  } = useDock();
  const pathname = usePathname();

  const itemId = id || label.toLowerCase().replace(/\s+/g, "-");
  const isOpen = openDropdowns[itemId] || false;

  const isAnyChildActive = React.Children.toArray(children).some((child) => {
    if (
      React.isValidElement<DockDropdownItemProps>(child) &&
      (child.type as { displayName?: string }).displayName ===
        "DockDropdownItem" &&
      child.props.href
    ) {
      const currentPath = activePage !== undefined ? activePage : pathname;
      return currentPath === child.props.href;
    }
    return false;
  });

  if (renderType === "content") {
    return (
      <m.div
        initial={{ opacity: 0, height: 0 }}
        animate={{
          opacity: isOpen ? 1 : 0,
          height: isOpen ? "auto" : 0,
        }}
        transition={{
          duration: 0.3,
          ease: "easeInOut",
        }}
        className={cn(
          "w-full overflow-hidden",
          isOpen ? "pointer-events-auto min-h-[100px]" : "pointer-events-none"
        )}
        onMouseEnter={() => handleDropdownEnter(itemId)}
        onMouseLeave={() => handleDropdownLeave(itemId)}
      >
        <div className="px-[15px] pt-[15px] pb-[30px] flex justify-between items-start w-full min-w-[400px] bg-white dark:bg-transparent">
          <div className="gap-[12.5px] flex flex-col">{children}</div>
          <DockItemImagePreview>{children}</DockItemImagePreview>
        </div>
      </m.div>
    );
  }

  return (
    <m.div
      className={cn(
        "transition-colors duration-200 text-[14px] leading-[10px] flex items-center gap-1 h-[42px] rounded-full cursor-pointer px-[18px]",
        isAnyChildActive
          ? "text-black dark:text-white font-medium"
          : "text-black dark:text-white",
        className
      )}
      onMouseEnter={() => handleDropdownEnter(itemId)}
      onMouseLeave={() => handleDropdownLeave(itemId)}
      animate={{
        backgroundColor:
          isOpen || isAnyChildActive
            ? isDark
              ? "#262626"
              : "#F0F0F0"
            : "transparent",
      }}
      whileHover={{
        backgroundColor:
          isOpen || isAnyChildActive
            ? isDark
              ? "#262626"
              : "#F0F0F0"
            : isDark
              ? "#262626"
              : "#F0F0F0",
      }}
      transition={{ duration: 0.2 }}
    >
      {label}
      <m.svg
        width="16"
        height="16"
        viewBox="0 0 16 16"
        className="text-black dark:text-white"
        xmlns="http://www.w3.org/2000/svg"
        animate={{ rotate: isOpen ? 180 : 0 }}
        transition={{ duration: 0.2 }}
      >
        <path
          fillRule="evenodd"
          clipRule="evenodd"
          d="M8 8.93934L4.53033 5.46967L3.46967 6.53033L6.58578 9.64645C7.36683 10.4275 8.63316 10.4275 9.41421 9.64645L12.5303 6.53033L11.4697 5.46967L8 8.93934Z"
          fill="currentColor"
        ></path>
      </m.svg>
    </m.div>
  );
};
DockItem.displayName = "DockItem";

const DockItemImagePreview = ({ children }: { children: React.ReactNode }) => {
  const { hoveredLink, activePage } = useDock();
  const pathname = usePathname();

  const activeChild = React.Children.toArray(children).find((child) => {
    if (
      React.isValidElement<DockDropdownItemProps>(child) &&
      (child.type as { displayName?: string }).displayName ===
        "DockDropdownItem" &&
      child.props.href
    ) {
      const currentPath = activePage !== undefined ? activePage : pathname;
      return currentPath === child.props.href;
    }
    return false;
  }) as React.ReactElement<DockDropdownItemProps> | undefined;

  const hoveredChild = React.Children.toArray(children).find((child) => {
    return (
      React.isValidElement<DockDropdownItemProps>(child) &&
      (child.type as { displayName?: string }).displayName ===
        "DockDropdownItem" &&
      child.props.href === hoveredLink
    );
  }) as React.ReactElement<DockDropdownItemProps> | undefined;

  const displayImage = hoveredChild?.props.image || activeChild?.props.image;
  const shouldShowImage = hoveredLink || activeChild;

  if (!displayImage) return null;

  return (
    <div className="flex flex-col items-end gap-2">
      <m.img
        key={displayImage}
        initial={{ opacity: 0, scale: 0.8 }}
        animate={{
          opacity: shouldShowImage ? 1 : 0.8,
          scale: shouldShowImage ? 1 : 0.9,
        }}
        transition={{
          ease: shouldShowImage ? easeIn : easeOut,
          duration: 0.2,
        }}
        src={displayImage}
        className="rounded-[15px] w-[80px] h-[80px] object-cover"
        alt=""
      />
    </div>
  );
};

interface DockDropdownItemProps {
  href: string;
  label: string;
  image?: string;
  className?: string;
}

export const DockDropdownItem = ({
  href,
  label,
  className,
}: DockDropdownItemProps) => {
  const { hoveredLink, setHoveredLink, activePage } = useDock();
  const pathname = usePathname();

  const currentPath = activePage !== undefined ? activePage : pathname;
  const isMenuItemActive = currentPath === href;
  const isHovered = hoveredLink === href;

  return (
    <m.a
      href={href}
      whileHover={{ x: 5 }}
      transition={{ duration: 0.1 }}
      onMouseEnter={() => setHoveredLink(href)}
      className={cn(
        "block text-[14px] leading-[10px] transition-colors",
        isMenuItemActive || isHovered
          ? "text-black dark:text-white font-medium"
          : "text-neutral-500 dark:text-[#C1C1C1] hover:text-black dark:hover:text-white",
        className
      )}
    >
      {label}
    </m.a>
  );
};
DockDropdownItem.displayName = "DockDropdownItem";

interface DockIconProps {
  icon: React.ReactNode;
  href: string;
  renderType?: "content" | "trigger";
  className?: string;
}

export const DockIcon = ({
  icon,
  href,
  renderType,
  className,
}: DockIconProps) => {
  const { isDark, activePage } = useDock();
  const pathname = usePathname();

  if (renderType === "content") return null;

  const currentPath = activePage !== undefined ? activePage : pathname;
  const isActive = currentPath === href;

  return (
    <Link href={href}>
      <m.div
        className={cn(
          "flex items-center justify-center w-[56px] h-[42px] rounded-full cursor-pointer",
          className
        )}
        animate={{
          backgroundColor: isActive
            ? isDark
              ? "#262626"
              : "#F0F0F0"
            : "transparent",
        }}
        whileHover={{
          backgroundColor: isDark ? "#262626" : "#F0F0F0",
        }}
        transition={{ duration: 0.2 }}
      >
        {icon}
      </m.div>
    </Link>
  );
};
DockIcon.displayName = "DockIcon";

interface DockLinkProps {
  label: string;
  href: string;
  icon?: React.ReactNode;
  external?: boolean;
  renderType?: "content" | "trigger";
  id?: string;
  className?: string;
}

export const DockLink = ({
  label,
  href,
  icon,
  external,
  renderType,
  className,
}: DockLinkProps) => {
  const { isDark, activePage } = useDock();
  const pathname = usePathname();
  const [isHovered, setIsHovered] = useState(false);

  if (renderType === "content") return null;

  const currentPath = activePage !== undefined ? activePage : pathname;
  const isActive = currentPath === href;

  const linkContent = (
    <>
      {label}
      {icon && (
        <m.div
          initial={{ x: 0, y: 0 }}
          animate={{
            x: isHovered ? 2 : 0,
            y: isHovered ? -2 : 0,
          }}
          transition={{ duration: 0.2 }}
        >
          {icon}
        </m.div>
      )}
    </>
  );

  const baseClassName = cn(
    "transition-colors duration-200 text-[14px] leading-[10px] flex items-center gap-1 h-[42px] rounded-full px-[18px]",
    isActive
      ? "text-black dark:text-white font-medium"
      : "text-black dark:text-white",
    className
  );

  if (external) {
    return (
      <m.a
        href={href}
        target="_blank"
        rel="noopener noreferrer"
        className={baseClassName}
        onMouseEnter={() => setIsHovered(true)}
        onMouseLeave={() => setIsHovered(false)}
        whileHover={{
          backgroundColor: isActive
            ? isDark
              ? "#404040"
              : "#E0E0E0"
            : isDark
              ? "#262626"
              : "#F0F0F0",
        }}
        transition={{ duration: 0.2 }}
      >
        {linkContent}
      </m.a>
    );
  }

  return (
    <m.div
      className="inline-block rounded-full"
      animate={{
        backgroundColor: isActive
          ? isDark
            ? "#262626"
            : "#F0F0F0"
          : "transparent",
      }}
      whileHover={{
        backgroundColor: isDark ? "#262626" : "#F0F0F0",
      }}
      transition={{ duration: 0.2 }}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      <Link href={href} className={baseClassName}>
        {linkContent}
      </Link>
    </m.div>
  );
};
DockLink.displayName = "DockLink";

export default Dock;
```

---

### Feature Steps <a name="feature-steps"></a>

A dynamic feature showcase with auto-playing steps and synchronized image transitions.

- **Registry Name**: `feature-steps`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/feature-steps.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/feature-steps/feature-steps.tsx`

```tsx
"use client";

import React, { useState, useEffect } from "react";
import { LazyMotion, domMax, m } from "motion/react";
import Image from "next/image";
import { cn } from "@/lib/utils";

interface Feature {
  step?: string;
  title: string;
  content: string;
  image: string;
}

interface FeatureStepsProps {
  features: Feature[];
  className?: string;
  autoPlayInterval?: number;
  imageClassName?: string;
}

export default function FeatureSteps({
  features,
  className,
  autoPlayInterval = 3000,
  imageClassName = "h-[400px]",
}: FeatureStepsProps) {
  const [state, setState] = useState({ feature: 0, tick: 0 });

  useEffect(() => {
    const timer = setInterval(() => {
      setState((prev) => ({
        feature: (prev.feature + 1) % features.length,
        tick: prev.tick + 1,
      }));
    }, autoPlayInterval);

    return () => clearInterval(timer);
  }, [autoPlayInterval, state.feature, features.length]);

  const currentFeature = state.feature;
  const progressKey = state.tick;

  return (
    <LazyMotion features={domMax}>
      <div
        className={cn(
          "flex flex-col md:flex-row w-full md:items-stretch max-w-[1440px] mx-auto",
          className
        )}
      >
        {/* Left Column: Feature List */}
        <div className="flex flex-col w-full md:w-1/2 border border-black/10 dark:border-white/10 divide-y divide-black/10 dark:divide-white/10">
          {features.map((feature, index) => (
            <m.div
              key={feature.title}
              layoutId={feature.title}
              onClick={() => {
                setState((prev) => ({ feature: index, tick: prev.tick + 1 }));
              }}
              className="p-4 md:p-10 relative cursor-pointer"
            >
              <h3 className="text-base md:text-lg leading-none text-black dark:text-white">
                {feature.title}
              </h3>
              <p className="mt-2 text-sm  text-neutral-500">
                {feature.content}
              </p>
              {index === currentFeature && (
                <m.div
                  key={progressKey}
                  className="absolute h-[1px] bottom-0 left-0 bg-black dark:bg-white"
                  initial={{ width: "0%" }}
                  animate={{ width: "100%" }}
                  transition={{
                    duration: autoPlayInterval / 1000,
                    ease: "easeIn",
                  }}
                />
              )}
            </m.div>
          ))}
        </div>

        {/* Right Column: Image Display */}
        <div
          className={cn(
            "w-full md:w-1/2 border border-black/10 dark:border-white/10 md:border-l-0 border-t-0 md:border-t relative p-4 md:p-5 overflow-hidden",
            imageClassName
          )}
        >
          <m.div
            key={currentFeature}
            initial={{ y: "100%" }}
            animate={{ y: 0 }}
            transition={{ duration: 0.3, ease: "easeInOut" }}
            className="relative w-full h-full"
          >
            <Image
              src={features[currentFeature].image}
              alt={features[currentFeature].title}
              fill
              sizes="(max-width: 768px) 100vw, 50vw"
              className="rounded object-cover"
            />
          </m.div>
        </div>
      </div>
    </LazyMotion>
  );
}
```

---

### Focus Button <a name="focus-button"></a>

A button with a focus ring animation effect.

- **Registry Name**: `focus-button`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/focus-button.json
```

#### Dependencies

- **NPM Packages**:
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/focus-button/focus-button.tsx`

```tsx
"use client";

import React from "react";
import { cn } from "@/lib/utils";

interface FocusButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  children: React.ReactNode;
  className?: string;
  dashColor?: string;
}

const FocusButton = ({
  children,
  className,
  dashColor,
  ...props
}: FocusButtonProps) => {
  return (
    <button
      className={cn(
        "group relative px-6 py-3 text-black dark:text-white leading-none text-[16px] tracking-normal cursor-pointer",
        className
      )}
      {...props}
    >
      {/* Main Border */}
      <div className="absolute inset-0 border border-gray-200 dark:border-white/20" />

      {/* Corner Dashes */}
      <CornerDashes color={dashColor} />

      {/* Content */}
      <span className="relative z-10">{children}</span>
    </button>
  );
};

const CornerDashes = ({ color }: { color?: string }) => {
  return (
    <>
      {/* Top Right Corner */}
      <div
        className={cn(
          "absolute top-0 right-0 w-2 h-2 border-t border-r border-black dark:border-white transition-all duration-300 group-hover:w-full group-hover:h-full group-hover:dark:border-white group-hover:border-black"
        )}
        style={color ? { borderColor: color } : undefined}
      />

      {/* Bottom Left Corner */}
      <div
        className={cn(
          "absolute bottom-0 left-0 w-2 h-2 border-b border-l border-black dark:border-white transition-all duration-300 group-hover:w-full group-hover:h-full group-hover:dark:border-white group-hover:border-black"
        )}
        style={color ? { borderColor: color } : undefined}
      />
    </>
  );
};

export default FocusButton;
```

---

### Gauge <a name="gauge"></a>

A customizable semi-circular gauge component for visualizing metrics and performance scores.

- **Registry Name**: `gauge`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/gauge.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/gauge/gauge.tsx`

```tsx
"use client";

import { useEffect, useState } from "react";
import {
  LazyMotion,
  domAnimation,
  m,
  useMotionValue,
  useTransform,
  animate,
} from "motion/react";
import { cn } from "@/lib/utils";

interface GaugeProps {
  value?: number;
  min?: number;
  max?: number;
  size?: number; // Diameter of the gauge
  gap?: number; // Gap between bars in degrees
  thickness?: number; // Thickness of the bars
  activeColor?: string;
  inactiveColor?: string;
  showValue?: boolean;
  label?: string;
  className?: string;
  delay?: number;
}

const Gauge = ({
  value = 70,
  min = 0,
  max = 100,
  size = 400,
  gap = 4,
  thickness = 10,
  activeColor = "bg-blue-600",
  inactiveColor = "bg-blue-100",
  showValue = true,
  label = "Performance",
  className,
  delay = 25,
}: GaugeProps) => {
  const [isMounted, setIsMounted] = useState(false);

  const count = useMotionValue(0);
  const rounded = useTransform(count, (latest) => Math.round(latest));

  const [dims, setDims] = useState({ size, thickness });

  useEffect(() => {
    const timer = setTimeout(() => {
      setIsMounted(true);
    }, 100); // Small delay to ensure initial render is processed
    return () => clearTimeout(timer);
  }, []);

  const { size: currentSize, thickness: currentThickness } = dims;
  const radius = currentSize / 2;
  // Calculate percentage for display
  const percentage = Math.round(((value - min) / (max - min)) * 100);

  const totalBars = Math.floor(180 / gap);

  useEffect(() => {
    if (isMounted) {
      const animation = animate(count, percentage, {
        duration: (totalBars * delay) / 1000,
      });
      return animation.stop;
    }
  }, [isMounted, percentage, totalBars, delay, count]);

  useEffect(() => {
    const handleResize = () => {
      if (window.innerWidth < 640) {
        setDims({ size: 280, thickness: 6 });
      } else {
        setDims({ size, thickness });
      }
    };

    handleResize();
    window.addEventListener("resize", handleResize);
    return () => window.removeEventListener("resize", handleResize);
  }, [size, thickness]);

  return (
    <LazyMotion features={domAnimation}>
      <div
        className={cn("relative flex justify-center items-center", className)}
        style={{ width: currentSize, height: currentSize / 2 }}
      >
        <m.div className="absolute bottom-0 left-1/2">
          {Array.from({ length: totalBars }).map((_, index) => {
            const barRotation = index * gap - 90;
            const isActive =
              isMounted && index < (percentage / 100) * totalBars;

            return (
              <div
                key={index}
                className={cn(
                  "absolute rounded-full transition-colors duration-300",
                  isActive ? `${activeColor} ` : inactiveColor
                )}
                style={{
                  width: currentThickness,
                  height: currentSize / 8, // Proportional height
                  left: "50%",
                  top: "50%",
                  transform: `translate(-50%, -50%) rotate(${barRotation}deg) translateY(-${radius}px)`,
                  transitionDelay: `${index * delay}ms`,
                }}
              />
            );
          })}

          {showValue && (
            <div
              className="absolute left-1/2 -translate-x-1/2 flex flex-col items-center"
              style={{
                bottom: 0, // Align to bottom of the container
              }}
            >
              <h2 className="flex text-black dark:text-white font-bold leading-none text-[32px] md:text-[48px]">
                <m.span>{rounded}</m.span>%
              </h2>
              {label && (
                <p className="text-black dark:text-white font-medium font-sm md:font-base">
                  {label}
                </p>
              )}
            </div>
          )}
        </m.div>
      </div>
    </LazyMotion>
  );
};

export default Gauge;
```

---

### Gif Text <a name="gif-text"></a>

A stunning text effect that uses a GIF as the fill color.

- **Registry Name**: `gif-text`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/gif-text.json
```

#### Dependencies

- **NPM Packages**:
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/gif-text/gif-text.tsx`

```tsx
"use client";

import { cn } from "@/lib/utils";
import { useEffect, useState } from "react";

interface GifTextProps {
  /**
   * The text to display
   */
  text?: string;
  /**
   * The source URL for the background image/gif
   */
  gif?: string;
  /**
   * Class for the text element
   */
  className?: string;
  /**
   * Class for the container (e.g. height, background)
   */
  containerClassName?: string;
}

const GifText = ({
  text = "CHAMAAC",
  gif = "https://assets.amarn.me/gif-text.gif",
  className,
  containerClassName,
}: GifTextProps) => {
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    if (!gif) return;

    setLoading(true);
    const img = new Image();
    img.src = gif;
    img.onload = () => setLoading(false);

    return () => {
      img.onload = null;
    };
  }, [gif]);

  return (
    <div
      className={cn(
        "flex flex-col items-center justify-center p-4 bg-white dark:bg-black",
        containerClassName
      )}
    >
      <h2
        className={cn(
          "text-[clamp(80px,12vw,150px)] font-extrabold select-none text-center leading-tight uppercase transition-colors duration-300 ",
          loading
            ? "text-neutral-400 animate-pulse duration-100"
            : "text-transparent bg-clip-text bg-cover bg-center bg-no-repeat",
          className
        )}
        style={{
          backgroundImage: loading ? "none" : `url(${gif})`,
          WebkitBackgroundClip: loading ? "none" : "text",
          backgroundClip: loading ? "none" : "text",
        }}
      >
        {text}
      </h2>
    </div>
  );
};

export default GifText;
```

---

### Glowing Border Button <a name="glowing-border-button"></a>

A stylish button with a glowing border animation.

- **Registry Name**: `glowing-border-button`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/glowing-border-button.json
```

#### Dependencies

- **NPM Packages**:
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/glowing-border-button/glowing-border-button.tsx`

```tsx
"use client";

import React from "react";
import { cn } from "@/lib/utils";

interface GlowingBorderButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  className?: string; // Explicitly kept if needed, though extended types usually cover it
  children: React.ReactNode;
}

const GlowingBorderButton = ({
  className,
  children = "Book a Call",
  ...props
}: GlowingBorderButtonProps) => {
  return (
    <button
      className={cn(
        "glowing-border-button group relative h-[60px] px-4 cursor-pointer border-0 bg-transparent p-0 text-[20px] font-bold outline-none",
        className
      )}
      {...props}
    >
      <div
        className={cn(
          "relative h-full w-full overflow-hidden rounded-[18px] p-[2px] transition-all duration-300 ease-in-out",
          "dark:bg-zinc-800 bg-zinc-200"
        )}
      >
        {/* Rotating Border Beam */}
        <div className="absolute inset-0 overflow-hidden rounded-[18px]">
          <div
            className={cn(
              "absolute left-1/2 top-1/2 h-[500%] w-[80px] -translate-x-1/2 -translate-y-1/2 animate-[spin_3s_linear_infinite]",
              "[background:linear-gradient(to_right,transparent_20%,#50C878_50%,#50C878_60%,transparent_80%)]",
              "blur-[2px]"
            )}
          ></div>
        </div>

        {/* Inner Content */}
        <div
          className={cn(
            "content relative z-10 flex h-full w-full items-center justify-center gap-2 rounded-[16px] transition-all duration-300 ease-in-out bg-white dark:bg-black px-11"
          )}
        >
          <span className="dark:text-white text-black transition-colors duration-300">
            {children}
          </span>
        </div>
      </div>
    </button>
  );
};

export default GlowingBorderButton;
```

---

### Hover Arrow Button <a name="hover-arrow-button"></a>

A button with a smooth hover arrow swap animation.

- **Registry Name**: `hover-arrow-button`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/hover-arrow-button.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `clsx`
  - `tailwind-merge`
  - `@tabler/icons-react`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/hover-arrow-button/hover-arrow-button.tsx`

```tsx
"use client";

import React from "react";
import { IconArrowRight } from "@tabler/icons-react";
import { LazyMotion, domAnimation, m } from "motion/react";
import { cn } from "@/lib/utils";

interface HoverArrowButtonProps extends React.ComponentProps<typeof m.button> {
  text?: string;
  duration?: number;
  iconSize?: number;
}

export default function HoverArrowButton({
  text = "Get Started",
  duration = 0.3,
  iconSize = 24,
  className,
  ...props
}: HoverArrowButtonProps) {
  return (
    <LazyMotion features={domAnimation}>
      <m.button
        className={cn(
          "group relative inline-flex items-center justify-center overflow-hidden rounded-full bg-black px-6 py-3 text-white transition-all  dark:bg-white dark:text-black  text-base font-medium cursor-pointer",
          className
        )}
        whileHover="hover"
        initial="initial"
        whileTap="tap"
        variants={{
          initial: { scale: 1 },
          hover: { scale: 1 },
        }}
        {...props}
      >
        <m.div
          className="flex items-center justify-center overflow-hidden"
          variants={{
            initial: { width: 0, opacity: 0 },
            hover: { width: "auto", opacity: 1 },
          }}
          transition={{
            duration: duration,
            ease: [0.165, 0.84, 0.44, 1],
          }}
        >
          <m.div
            className="flex items-center justify-center"
            variants={{
              initial: { x: "-200%", opacity: 0 },
              hover: { x: 0, opacity: 1 },
            }}
            transition={{
              duration: duration,
              ease: [0.165, 0.84, 0.44, 1],
            }}
          >
            <IconArrowRight
              size={iconSize}
              className="text-white dark:text-black"
            />
          </m.div>
        </m.div>

        <span className="mx-2">{text}</span>

        <m.div
          className="flex items-center justify-center overflow-hidden"
          variants={{
            initial: { width: "auto", opacity: 1 },
            hover: { width: 0, opacity: 0 },
          }}
          transition={{
            duration: duration,
            ease: [0.165, 0.84, 0.44, 1],
          }}
        >
          <m.div
            className="flex items-center justify-center"
            variants={{
              initial: { x: 0, opacity: 1 },
              hover: { x: "200%", opacity: 0 },
            }}
            transition={{
              duration: duration,
              ease: [0.165, 0.84, 0.44, 1],
            }}
          >
            <IconArrowRight
              size={iconSize}
              className="text-white dark:text-black"
            />
          </m.div>
        </m.div>
      </m.button>
    </LazyMotion>
  );
}
```

---

### How It Works <a name="how-it-works"></a>

A visual flow component showing a step-by-step process with connected cards.

- **Registry Name**: `how-it-works`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/how-it-works.json
```

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/how-it-works/how-it-works.tsx`

```tsx
"use client";

import React from "react";
import { LazyMotion, domAnimation, m } from "motion/react";

interface CardProps {
  number: string;
  title: string;
  description: string;
  colorTheme?: "orange" | "blue" | "purple";
  className?: string;
  rotate?: string;
  colors?: {
    bg: string;
    text: string;
    border: string;
  };
}

const Pin = ({ className }: { className?: string }) => (
  <svg
    xmlns="http://www.w3.org/2000/svg"
    width="24"
    height="24"
    viewBox="0 0 24 24"
    fill="currentColor"
    className={className}
  >
    <path stroke="none" d="M0 0h24v24H0z" fill="none" />
    <path d="M16 3a1 1 0 0 1 .117 1.993l-.117 .007v4.764l1.894 3.789a1 1 0 0 1 .1 .331l.006 .116v2a1 1 0 0 1 -.883 .993l-.117 .007h-4v4a1 1 0 0 1 -1.993 .117l-.007 -.117v-4h-4a1 1 0 0 1 -.993 -.883l-.007 -.117v-2a1 1 0 0 1 .06 -.34l.046 -.107l1.894 -3.791v-4.762a1 1 0 0 1 -.117 -1.993l.117 -.007h8z" />
  </svg>
);

const Card = ({
  number,
  title,
  description,
  colorTheme = "blue",
  className,
  rotate,
  colors: customColors,
}: CardProps) => {
  const defaultBgColors = {
    orange: "bg-orange-50 dark:bg-orange-500/10",
    blue: "bg-blue-50 dark:bg-blue-500/10",
    purple: "bg-purple-50 dark:bg-purple-500/10",
  };
  const defaultTextColors = {
    orange: "text-orange-500 dark:text-orange-400",
    blue: "text-blue-600 dark:text-blue-400",
    purple: "text-purple-600 dark:text-purple-400",
  };
  const defaultBorderColors = {
    orange: "border-orange-100 dark:border-orange-500/20",
    blue: "border-blue-100 dark:border-blue-500/20",
    purple: "border-purple-100 dark:border-purple-500/20",
  };

  const bgColor = customColors?.bg || defaultBgColors[colorTheme];
  const textColor = customColors?.text || defaultTextColors[colorTheme];
  const borderColor = customColors?.border || defaultBorderColors[colorTheme];

  return (
    <div
      className={`relative w-full md:w-[280px] transition-transform duration-300 hover:z-30 hover:scale-105 ${rotate} ${className}`}
    >
      <div className="bg-white dark:bg-neutral-900 p-2 rounded-[25px] shadow-[0px_10px_20px_0px_#D3D3D3] dark:shadow-none border border-neutral-100 dark:border-neutral-800">
        <Pin className={`w-8 h-8 ${textColor} z-20 mb-6 mx-auto`} />
        <div
          className={`${bgColor} border ${borderColor} rounded-[15px] p-[15px] h-full flex flex-col relative overflow-hidden`}
        >
          <span
            className={`${textColor} text-4xl font-handwriting mb-5`}
            style={{
              fontFamily: '"Comic Sans MS", "Chalkboard SE", sans-serif',
            }}
          >
            {number}
          </span>
          <h3 className="text-2xl font-semibold text-neutral-800 dark:text-neutral-100 leading-none mb-[10px]">
            {title}
          </h3>
          <p className="text-neutral-500 dark:text-neutral-400 text-sm/5 tracking-tight">
            {description}
          </p>
        </div>
      </div>
    </div>
  );
};

export interface Step {
  title: string;
  description: string;
  colorTheme?: "orange" | "blue" | "purple";
  colors?: {
    bg: string;
    text: string;
    border: string;
  };
}

export interface StepPosition {
  className?: string;
  rotate?: string;
}

export interface HowItWorksProps {
  features?: Step[];
  className?: string;
  stepPositions?: StepPosition[];
}

const DEFAULT_CARD_POSITIONS: StepPosition[] = [
  { className: "md:absolute md:top-0 md:left-[15%]", rotate: "rotate-8" },
  {
    className: "md:absolute md:top-[120px] md:right-[15%]",
    rotate: "-rotate-8",
  },
  { className: "md:absolute md:top-[450px] md:left-[15%]", rotate: "rotate-8" },
  {
    className: "md:absolute md:top-[570px] md:right-[10%]",
    rotate: "-rotate-8",
  },
  { className: "md:absolute md:top-[850px] md:left-[15%]", rotate: "rotate-8" },
];

export default function HowItWorks({
  features,
  className,
  stepPositions,
}: HowItWorksProps) {
  const defaultFeatures: Step[] = [
    {
      title: "Create Account",
      description:
        "Sign up in minutes. Enter your details and verify your email to get started.",
      colorTheme: "orange",
    },
    {
      title: "Verify Identity",
      description:
        "Complete your profile verification to ensure secure transactions and compliance.",
      colorTheme: "blue",
    },
    {
      title: "Select Plan",
      description:
        "Choose from a variety of investment plans tailored to your financial goals.",
      colorTheme: "purple",
    },
    {
      title: "Analyze & Invest",
      description:
        "Review returns and make your first investment with confidence.",
      colorTheme: "orange",
    },
    {
      title: "Track Growth",
      description:
        "Monitor your portfolio in real-time and watch your wealth grow over time.",
      colorTheme: "blue",
    },
  ];

  const data = features && features.length > 0 ? features : defaultFeatures;
  const positions = stepPositions || DEFAULT_CARD_POSITIONS;

  let height = 1130;
  if (data.length === 1) height = 400;
  else if (data.length === 2) height = 450;
  else if (data.length === 3) height = 800;
  else if (data.length === 4) height = 900;
  else height = 1130;

  return (
    <LazyMotion features={domAnimation}>
      <div
        className={`bg-white dark:bg-black max-md:pt-10 max-md:pb-25 md:py-20 px-8 relative ${className}`}
      >
        <div
          className="absolute inset-0 pointer-events-none opacity-[0.08] dark:opacity-[0.15]"
          style={{
            backgroundImage: "linear-gradient(#000 1px, transparent 1px)",
            backgroundSize: "100% 32px",
            marginTop: "4px",
          }}
        ></div>
        <div
          className="absolute inset-0 pointer-events-none opacity-0 dark:opacity-[0.1]"
          style={{
            backgroundImage: "linear-gradient(#fff 1px, transparent 1px)",
            backgroundSize: "100% 32px",
            marginTop: "4px",
          }}
        ></div>
        <div className="from-background pointer-events-none absolute inset-y-0 left-0 w-1/2 bg-gradient-to-r"></div>
        <div className="from-background pointer-events-none absolute inset-y-0 right-0 w-1/2 bg-gradient-to-l"></div>

        <div className="max-w-6xl mx-auto relative z-10">
          <div
            className="relative w-full max-w-[1000px] mx-auto flex flex-col space-y-8 md:space-y-0 md:block h-auto md:h-[var(--md-height)]"
            style={{ "--md-height": `${height}px` } as React.CSSProperties}
          >
            {data.length > 1 && (
              <svg
                className="absolute top-0 left-0 w-full h-full pointer-events-none hidden md:block z-0"
                viewBox={`0 0 1000 ${height}`}
                preserveAspectRatio="none"
              >
                {(() => {
                  const pathD = data.reduce((acc, _, index) => {
                    if (index >= data.length - 1) return acc;
                    if (index === 0)
                      return "M 290 150 C 500 150, 550 270, 710 270"; // 1 -> 2
                    if (index === 1)
                      return acc + " C 850 270, 500 350, 290 450"; // 2 -> 3
                    if (index === 2)
                      return acc + " C 290 600, 550 720, 750 720"; // 3 -> 4
                    if (index === 3)
                      return acc + " C 950 720, 500 800, 290 850"; // 4 -> 5
                    return acc;
                  }, "");
                  return (
                    <m.path
                      d={pathD}
                      stroke="currentColor"
                      className="text-neutral-300 dark:text-neutral-700"
                      strokeWidth="2"
                      strokeDasharray="8 6"
                      fill="none"
                      strokeLinecap="round"
                      vectorEffect="non-scaling-stroke"
                      initial={{ strokeDashoffset: 0 }}
                      animate={{
                        strokeDashoffset: -140, // Multiple of 14 (8+6) for seamless loop
                      }}
                      transition={{
                        duration: 3,
                        repeat: Infinity,
                        ease: "linear",
                      }}
                    />
                  );
                })()}
              </svg>
            )}

            {data.map((step, index) => {
              const position = positions[index % positions.length];

              return (
                <Card
                  key={step.title}
                  number={`0${index + 1}`}
                  title={step.title}
                  description={step.description}
                  colorTheme={step.colorTheme || "blue"}
                  colors={step.colors}
                  rotate={position.rotate}
                  className={position.className}
                />
              );
            })}
          </div>
        </div>
      </div>
    </LazyMotion>
  );
}
```

---

### Interactive Grid Background <a name="interactive-grid-background"></a>

A highly interactive, mouse-sensitive grid background.

- **Registry Name**: `interactive-grid-background`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/interactive-grid-background.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/interactive-grid/interactive-grid-background.tsx`

```tsx
"use client";

import React, { useEffect, useRef } from "react";
import { cn } from "@/lib/utils";

interface InteractiveGridBackgroundProps
  extends React.HTMLProps<HTMLDivElement> {
  gridGap?: number;
  dotSize?: number;
  color?: string;
  highlightColor?: string;
  radius?: number;
}

export function InteractiveGridBackground({
  className,
  children,
  gridGap = 40,
  dotSize = 1.5,
  color = "#737373", // zinc-700
  highlightColor = "#FFFF00",
  radius = 300,
  ...props
}: InteractiveGridBackgroundProps) {
  const canvasRef = useRef<HTMLCanvasElement>(null);
  const containerRef = useRef<HTMLDivElement>(null);
  const mouseRef = useRef({ x: -1000, y: -1000 });
  const requestRef = useRef<number | undefined>(undefined);

  useEffect(() => {
    const canvas = canvasRef.current;
    const container = containerRef.current;
    if (!canvas || !container) return;

    const ctx = canvas.getContext("2d");
    if (!ctx) return;

    // Mouse move handler
    const onMouseMove = (e: MouseEvent) => {
      const rect = container.getBoundingClientRect();
      mouseRef.current = {
        x: e.clientX - rect.left,
        y: e.clientY - rect.top,
      };
    };

    const onMouseLeave = () => {
      mouseRef.current = { x: -1000, y: -1000 };
    };

    container.addEventListener("mousemove", onMouseMove);
    container.addEventListener("mouseleave", onMouseLeave);

    const resize = () => {
      canvas.width = container.clientWidth;
      canvas.height = container.clientHeight;
    };
    resize();
    window.addEventListener("resize", resize);

    const animate = () => {
      if (!ctx || !canvas) return;
      const width = canvas.width;
      const height = canvas.height;

      ctx.clearRect(0, 0, width, height);

      // Draw grid dots
      for (let x = 0; x < width; x += gridGap) {
        for (let y = 0; y < height; y += gridGap) {
          // Distance to mouse
          const dx = x - mouseRef.current.x;
          const dy = y - mouseRef.current.y;
          const dist = Math.sqrt(dx * dx + dy * dy);

          // Base state
          let currentSize = dotSize;
          let currentAlpha = 0.5;
          let currentColor = color;

          // Interaction
          if (dist < radius) {
            const ratio = 1 - dist / radius;
            currentSize = dotSize + ratio * 2; // Scale up
            currentAlpha = 0.5 + ratio * 0.5;

            // We can't easily interpolate hex in canvas without parsing,
            // so we'll just switch color or rely on opacity.
            // For a premium feel, let's use the highlight color when very close
            if (ratio > 0.5) {
              currentColor = highlightColor;
            }
          }

          ctx.beginPath();
          ctx.arc(x, y, currentSize, 0, Math.PI * 2);

          // Simple hex to rgba (approximate or just use globalAlpha)
          // Let's assume input colors are hex.
          // To support alpha, we set globalAlpha.
          ctx.fillStyle = currentColor;
          ctx.globalAlpha = currentAlpha;
          ctx.fill();
          ctx.globalAlpha = 1.0;
        }
      }

      requestRef.current = requestAnimationFrame(animate);
    };

    animate();

    return () => {
      window.removeEventListener("resize", resize);
      container.removeEventListener("mousemove", onMouseMove);
      container.removeEventListener("mouseleave", onMouseLeave);
      if (requestRef.current) cancelAnimationFrame(requestRef.current);
    };
  }, [gridGap, dotSize, color, highlightColor, radius]);

  return (
    <div
      ref={containerRef}
      className={cn(
        "relative w-full h-screen bg-black overflow-hidden",
        className
      )}
      {...props}
    >
      <canvas ref={canvasRef} className="absolute inset-0 z-0" />
      <div className="relative z-10 w-full h-full pointer-events-none">
        {children}
      </div>
    </div>
  );
}
```

---

### Neo Brutalist Button <a name="neo-brutalist-button"></a>

A bold, retro-styled button with skewed design, offset shadow, and shimmer effect.

- **Registry Name**: `neo-brutalist-button`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/neo-brutalist-button.json
```

#### Dependencies

- **NPM Packages**:
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/neo-brutalist-button/neo-brutalist-button.tsx`

```tsx
"use client";

import { cn } from "@/lib/utils";

interface NeoBrutalistButtonProps {
  text?: string;
  className?: string;
  onClick?: () => void;
}

const NeoBrutalistButton = ({
  text = "Click Me",
  className,
  onClick,
}: NeoBrutalistButtonProps) => {
  return (
    <>
      <style jsx>{`
        @keyframes shimmer {
          0% {
            transform: translateX(-100%);
          }
          100% {
            transform: translateX(200%);
          }
        }
        .shimmer-effect {
          transform: translateX(-100%);
        }
        .group:hover .shimmer-effect {
          animation: shimmer 0.2s linear forwards;
        }
      `}</style>
      <button
        onClick={onClick}
        className={cn(
          "group relative overflow-hidden px-6 py-3 cursor-pointer",
          "bg-[#ff90e8] text-black",
          "skew-x-[-10deg] transition-all duration-100 border-[1.5px] border-black",
          "shadow-[4px_4px_0_0_black] hover:shadow-[8px_8px_0_0_black]",
          className
        )}
      >
        <div className="shimmer-effect absolute top-0 bottom-0 left-0 w-1/2 bg-gradient-to-r from-transparent via-white/80 to-transparent z-0" />
        <span className="relative z-10 inline-block skew-x-[10deg] text-lg font-medium">
          {text}
        </span>
      </button>
    </>
  );
};

export default NeoBrutalistButton;
```

---

### Premium Button <a name="premium-button"></a>

An animated button with a dynamic arrow grid animation.

- **Registry Name**: `premium-button`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/premium-button.json
```

#### Dependencies

- **NPM Packages**:
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/premium-button/premium-button.tsx`

```tsx
"use client";

import { cn } from "@/lib/utils";
import { useState, useEffect } from "react";

interface PremiumButtonProps {
  text?: string;
  className?: string;
  onClick?: () => void;
}

const PremiumButton = ({
  text = "Premium Button",
  className,
  onClick,
}: PremiumButtonProps) => {
  return (
    <button
      onClick={onClick}
      className={cn(
        "relative rounded-[8px] flex items-center gap-2 pl-[48px] pr-4 tracking-tight cursor-pointer h-[44px] bg-black  hover:scale-[1.02] active:scale-[0.98] transition-all dark:border dark:border-neutral-800",
        className
      )}
    >
      <Box />
      <span className="font-medium text-white">{text}</span>
    </button>
  );
};

const Box = () => {
  const [step, setStep] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => {
      setStep((prev) => (prev + 1) % 14);
    }, 100);
    return () => clearInterval(timer);
  }, []);

  const isArrow = (row: number, col: number) => {
    const headX = step - 4; // Start from outside left

    // Arrow shape definition based on head position
    if (row === 2) return col <= headX && col >= headX - 4;
    if (row === 1 || row === 3) return col === headX - 1;
    if (row === 0 || row === 4) return col === headX - 2;
    return false;
  };

  return (
    <div className="absolute inset-y-0 left-1 my-auto size-9 rounded-[4px] bg-rose-500 flex flex-col justify-center items-center gap-px transition-all duration-400 ease-out shadow-sm">
      {[0, 1, 2, 3, 4].map((row) => (
        <div key={row} className="flex gap-[2px]">
          {[0, 1, 2, 3, 4].map((col) => (
            <Bubble key={col} highlight={isArrow(row, col)} />
          ))}
        </div>
      ))}
    </div>
  );
};

const Bubble = ({ highlight }: { highlight?: boolean }) => {
  return (
    <span
      className={cn(
        "inline-block size-[3px] bg-white/25",
        highlight && "bg-white animate-nudge"
      )}
    />
  );
};

export default PremiumButton;
```

---

### Shimmer Button <a name="shimmer-button"></a>

A button with a shimmer animation effect.

- **Registry Name**: `shimmer-button`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/shimmer-button.json
```

#### Dependencies

- **NPM Packages**:
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/shimmer-button/shimmer-button.tsx`

```tsx
"use client";

import { LazyMotion, domAnimation, m } from "motion/react";
import { cn } from "@/lib/utils";

interface ShimmerButtonProps {
  text?: string;
  className?: string;
  duration?: number;
  onClick?: () => void;
}

const ShimmerButton = ({
  text = "Book a Free Call",
  className,
  duration = 1.2,
  onClick,
}: ShimmerButtonProps) => {
  return (
    <LazyMotion features={domAnimation}>
      <button
        onClick={onClick}
        className={cn(
          "group relative bg-white dark:bg-neutral-950 border border-neutral-200 dark:border-neutral-800 px-8 py-3 rounded-full cursor-pointer transition-all hover:scale-[1.02] active:scale-[0.98]",
          className
        )}
      >
        <m.span
          className="relative block bg-clip-text text-transparent bg-[linear-gradient(110deg,#737373_0%,#737373_40%,#171717_50%,#737373_60%,#737373_100%)] dark:bg-[linear-gradient(110deg,#a1a1aa_0%,#a1a1aa_40%,#ffffff_50%,#a1a1aa_60%,#a1a1aa_100%)] bg-[length:200%_100%] font-medium text-base tracking-tight"
          animate={{
            backgroundPosition: ["0% 0%", "-200% 0%"],
          }}
          transition={{
            repeat: Infinity,
            duration: duration,
            ease: "linear",
          }}
        >
          {text}
        </m.span>
      </button>
    </LazyMotion>
  );
};

export default ShimmerButton;
```

---

### Slide Up Button <a name="slideup-button"></a>

A button with a smooth slide-up text animation on hover.

- **Registry Name**: `slideup-button`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/slideup-button.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/slideup-button/slideup-button.tsx`

```tsx
"use client";

import React from "react";
import { LazyMotion, domAnimation, m } from "motion/react";
import { cn } from "@/lib/utils";

interface SlideUpButtonProps {
  children: React.ReactNode;
  className?: string;
  textDuration?: number;
  cloneDuration?: number;
  cloneDelay?: number;
  buttonScale?: number;
  buttonOpacity?: number;
  onClick?: () => void;
}

const SlideUpButton = ({
  children,
  className = "bg-[#f73b20] text-white",
  textDuration = 0.25,
  cloneDuration = 0.5,
  cloneDelay = 0.12,
  buttonScale = 0.98,
  buttonOpacity = 0.8,
  onClick,
}: SlideUpButtonProps) => {
  const buttonVariants = {
    initial: { scale: 1 },
    hover: { scale: buttonScale, opacity: buttonOpacity },
  };

  const textVariants = {
    initial: { y: 0 },
    hover: { y: "-200%" },
  };

  const cloneVariants = {
    initial: { y: "200%", rotate: 20 },
    hover: { y: 0, rotate: 0 },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.button
        onClick={onClick}
        variants={buttonVariants}
        initial="initial"
        whileHover="hover"
        className={cn(
          "relative overflow-hidden px-6 py-3 rounded-[12px] text-[16px] leading-[1.5] cursor-pointer",
          className
        )}
      >
        {/* container for stacked text */}
        <m.div className="relative overflow-hidden">
          {/* ORIGINAL TEXT */}
          <m.span
            variants={textVariants}
            transition={{
              duration: textDuration,
              ease: [0.55, 0.085, 0.68, 0.53],
            }}
            className="block"
          >
            {children}
          </m.span>

          {/* CLONE TEXT */}
          <m.span
            variants={cloneVariants}
            transition={{
              duration: cloneDuration,
              ease: [0.165, 0.84, 0.44, 1],
              delay: cloneDelay,
            }}
            className="block absolute top-0 left-0"
          >
            {children}
          </m.span>
        </m.div>
      </m.button>
    </LazyMotion>
  );
};

export default SlideUpButton;
```

---

### Stats Cards <a name="stats-cards"></a>

A set of animated statistic cards with hover effects.

- **Registry Name**: `stats-cards`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/stats-cards.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/stats-cards/stats-cards.tsx`

```tsx
"use client";

import Image from "next/image";
import { cn } from "@/lib/utils";
import { LazyMotion, domAnimation, m } from "motion/react";
import { Inter } from "next/font/google";

const inter = Inter({ subsets: ["latin"] });

interface StatsCardsProps {
  className?: string;
  width?: string;
  height?: string;
  images?: string[];
}

export function StatsCards({
  className,
  width = "w-70",
  height = "h-84",
  images = ["/images/models/1.png", "/images/models/2.png"],
}: StatsCardsProps) {
  return (
    <LazyMotion features={domAnimation}>
      <div
        className={cn(
          "flex flex-wrap items-center justify-center gap-6 sm:gap-4 md:gap-0 px-4 py-4 bg-orange-50",
          inter.className,
          className
        )}
      >
        {/* Card 1: Revenue */}
        <m.div
          className={cn(
            `relative z-10 ${width} ${height} bg-white rounded-[16px] p-5 flex flex-col justify-between hover:z-50`
          )}
          initial={{
            rotate: -3,
          }}
          whileHover={{
            rotate: 0,
            scale: 1.05,
            transition: { duration: 0.3, ease: "easeInOut" },
          }}
        >
          <div>
            <h2 className="text-[#FF4400] text-5xl font-semibold tracking-tighter">
              $100k+
            </h2>
          </div>
          <div>
            <h4 className="text-[#FF4400] font-medium text-lg leading-tight tracking-tighter">
              Revenue driven
            </h4>
            <div className="w-full h-px bg-[#FF4400] my-2"></div>
            <p className="text-[#FF4400] text-sm leading-tight tracking-tight max-w-[90%]">
              From scroll-stopping campaigns that actually sell.
            </p>
          </div>
        </m.div>

        {/* Card 2: Image Stats */}
        <m.div
          className={cn(
            `relative z-20 ${width} ${height} rounded-[16px] overflow-hidden group hover:z-50`
          )}
          initial={{
            rotate: 2,
            y: 1,
          }}
          whileHover={{
            rotate: 0,
            scale: 1.05,
            transition: { duration: 0.3, ease: "easeInOut" },
          }}
        >
          <Image
            src={images[0]}
            alt="Model"
            fill
            sizes="(max-width: 768px) 90vw, 400px"
            className="object-cover"
          />
          <div className="absolute top-5 left-5 bg-white/90 py-[4px] px-[8px] rounded-full text-xs font-semibold tracking-tighter text-black ">
            900k Liked
          </div>
        </m.div>

        {/* Card 3: Impressions */}
        <m.div
          className={cn(
            `relative z-30 ${width} ${height} bg-[#FF4400] rounded-[16px] p-5 flex flex-col justify-between  flex-shrink-0 hover:z-50`
          )}
          initial={{
            rotate: 8,
          }}
          whileHover={{
            rotate: 0,
            scale: 1.02,
            transition: { duration: 0.3, ease: "easeInOut" },
          }}
        >
          <div>
            <h2 className="text-white text-5xl font-semibold tracking-tighter">
              37M+
            </h2>
          </div>
          <div>
            <h4 className="text-white font-medium text-lg leading-tight tracking-tighter">
              Organic impressions
            </h4>
            <div className="w-full h-px bg-white my-2"></div>
            <p className="text-white text-sm leading-tight tracking-tight max-w-[90%]">
              Growth through content that sells, not just trends.
            </p>
          </div>
        </m.div>

        {/* Card 4: Image Stats */}
        <m.div
          className={cn(
            `relative z-40 ${width} ${height} rounded-[16px] overflow-hidden -ml-1 hover:z-50`
          )}
          initial={{
            rotate: -4,
          }}
          whileHover={{
            rotate: 0,
            scale: 1.05,
            transition: { duration: 0.3, ease: "easeInOut" },
          }}
        >
          <Image
            src={images[1]}
            alt="Campaign"
            fill
            sizes="(max-width: 768px) 90vw, 400px"
            className="object-cover"
          />
          <div className="absolute bottom-5 right-5 bg-white/90 py-[4px] px-[8px] rounded-full text-xs font-semibold tracking-tighter text-black ">
            1.5M Viewed
          </div>
        </m.div>
      </div>
    </LazyMotion>
  );
}
```

---

### Text Loop <a name="text-loop"></a>

An animated text loop with typewriter and gradient effect.

- **Registry Name**: `text-loop`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/text-loop.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `clsx`
  - `tailwind-merge`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/text-loop/text-loop.tsx`

```tsx
"use client";

import React, { useEffect, useState } from "react";
import {
  LazyMotion,
  domAnimation,
  m,
  AnimatePresence,
  Transition,
} from "motion/react";
import { cn } from "@/lib/utils";

interface TextLoopProps {
  staticText?: string;
  rotatingTexts?: string[];
  className?: string;
  interval?: number;
  transition?: Transition;
  staticTextClassName?: string;
  rotatingTextClassName?: string;
  backgroundClassName?: string;
  cursorClassName?: string;
}

export default function TextLoop({
  staticText = "Design",
  rotatingTexts = ["Limitless", "Timeless", "Flawless"],
  className,
  interval = 3000,
  transition = { duration: 0.8, ease: "easeInOut" },
  staticTextClassName,
  rotatingTextClassName,
  backgroundClassName,
  cursorClassName,
}: TextLoopProps) {
  const [index, setIndex] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => {
      setIndex((prev) => (prev + 1) % rotatingTexts.length);
    }, interval);
    return () => clearInterval(timer);
  }, [rotatingTexts.length, interval]);

  return (
    <LazyMotion features={domAnimation}>
      <div
        className={cn(
          "flex flex-row items-center justify-start w-fit text-4xl md:text-7xl font-medium tracking-tight",
          className
        )}
      >
        <span className={cn("mr-3 whitespace-nowrap", staticTextClassName)}>
          {staticText}
        </span>
        <div className="relative flex items-center">
          <AnimatePresence mode="wait">
            <m.div
              key={rotatingTexts[index]}
              initial={{ width: 0, opacity: 0 }}
              animate={{ width: "auto", opacity: 1 }}
              exit={{ width: 0, opacity: 0 }}
              transition={transition}
              className="overflow-hidden whitespace-nowrap relative"
            >
              {/* Background gradient box */}
              <div
                className={cn(
                  "absolute inset-0",
                  "bg-gradient-to-r from-transparent via-purple-200/30 to-purple-200",
                  "dark:from-transparent dark:via-violet-950/30 dark:to-violet-950/60",
                  backgroundClassName
                )}
              />

              <span
                className={cn(
                  "relative bg-clip-text text-transparent",
                  "bg-gradient-to-r from-violet-400 to-violet-800",
                  "dark:bg-gradient-to-r from-violet-400 to-violet-600 pr-1",
                  rotatingTextClassName
                )}
              >
                {rotatingTexts[index]}
              </span>
            </m.div>
          </AnimatePresence>

          {/* Cursor Line */}
          <m.div
            className={cn(
              "w-[3px] md:w-[4px] bg-violet-500 h-[1.10em] sm:h-[1em]",
              cursorClassName
            )}
            animate={{ opacity: [1, 0.5] }}
            transition={{
              duration: 0.8,
              repeat: Infinity,
              repeatType: "reverse",
            }}
          />
        </div>
      </div>
    </LazyMotion>
  );
}
```

---

## Animated Icons <a name="animated-icons"></a>

### Activity Icon <a name="activity-icon"></a>

An animated activity icon with a heartbeat line.

- **Registry Name**: `activity-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/activity-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/activity-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface ActivityIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  ease?: Easing;
}

const ActivityIcon = (props: ActivityIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 2, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    ease = "easeInOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const pathAnimationProps = {
    animate: shouldAnimate
      ? {
          pathLength: [0, 1, 1, 0],
          opacity: 1,
        }
      : { pathLength: 1, opacity: 1 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatType: "reverse" as const,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path
          d="M3 12h4.5l1.5 -6l4 12l2 -9l1.5 3h4.5"
          {...pathAnimationProps}
        />
      </m.svg>
    </LazyMotion>
  );
};

export default ActivityIcon;
```

---

### Adjustments Icon <a name="adjustments-icon"></a>

An animated adjustments/settings icon with smooth motion effects.

- **Registry Name**: `adjustments-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/adjustments-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/adjustments-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface AdjustmentsHorizontalIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const AdjustmentsHorizontalIcon = (props: AdjustmentsHorizontalIconProps) => {
  const {
    size = 28,
    duration = 1.2,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const path1Props = {
    animate: shouldAnimate ? { x: [0, -10, 0] } : { x: 0 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
      repeatType: "loop" as const,
    },
  };

  const path2Props = {
    animate: shouldAnimate ? { x: [0, 10, 0] } : { x: 0 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
      repeatType: "loop" as const,
    },
  };

  const path3Props = {
    animate: shouldAnimate ? { x: [0, -10, 0] } : { x: 0 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
      repeatType: "loop" as const,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M4 6l15 0" />
        <m.path
          className="fill-white dark:fill-black"
          stroke="currentColor"
          strokeWidth={strokeWidth}
          d="M14 6m-2 0a2 2 0 1 0 4 0a2 2 0 1 0 -4 0"
          {...path1Props}
        />

        <path d="M4 12l15 0" />
        <m.path
          className="fill-white dark:fill-black"
          stroke="currentColor"
          strokeWidth={strokeWidth}
          d="M8 12m-2 0a2 2 0 1 0 4 0a2 2 0 1 0 -4 0"
          {...path2Props}
        />

        <path d="M4 18l15 0" />
        <m.path
          className="fill-white dark:fill-black"
          stroke="currentColor"
          strokeWidth={strokeWidth}
          d="M17 18m-2 0a2 2 0 1 0 4 0a2 2 0 1 0 -4 0"
          {...path3Props}
        />
      </m.svg>
    </LazyMotion>
  );
};

export default AdjustmentsHorizontalIcon;
```

---

### Alert Icon <a name="alert-circle-icon"></a>

An animated alert icon.

- **Registry Name**: `alert-circle-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/alert-circle-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/alert-circle-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const AlertCircleIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const transition = {
    duration: duration,
    ease: ease,
    repeat: isHovered ? 0 : Infinity,
    repeatDelay: repeatDelay,
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <circle cx="12" cy="12" r="9" />
        <m.g
          animate={
            shouldAnimate ? { rotate: [0, -10, 10, -10, 10, 0] } : { rotate: 0 }
          }
          transition={transition}
          style={{ originX: "50%", originY: "50%" }}
        >
          <line x1="12" y1="8" x2="12" y2="12" />
          <line x1="12" y1="16" x2="12.01" y2="16" />
        </m.g>
      </m.svg>
    </LazyMotion>
  );
};

export default AlertCircleIcon;
```

---

### Arrow Down Icon <a name="arrow-down-icon"></a>

An animated arrow down icon.

- **Registry Name**: `arrow-down-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/arrow-down-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/arrow-down-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ArrowDownIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animation = {
    animate: shouldAnimate ? { y: [0, 4, 0] } : { x: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M12 5l0 14" {...animation} />
        <m.path d="M18 13l-6 6" {...animation} />
        <m.path d="M6 13l6 6" {...animation} />
      </m.svg>
    </LazyMotion>
  );
};

export default ArrowDownIcon;
```

---

### Arrow Down Left Icon <a name="arrow-down-left-icon"></a>

An animated arrow down left icon.

- **Registry Name**: `arrow-down-left-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/arrow-down-left-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/arrow-down-left-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ArrowDownLeftIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animation = {
    animate: shouldAnimate ? { x: [0, -2, 0], y: [0, 2, 0] } : { x: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M7 17l10 -10" {...animation} />
        <m.path d="M16 17l-9 0l0 -9" {...animation} />
      </m.svg>
    </LazyMotion>
  );
};

export default ArrowDownLeftIcon;
```

---

### Arrow Down Right Icon <a name="arrow-down-right-icon"></a>

An animated arrow down right icon.

- **Registry Name**: `arrow-down-right-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/arrow-down-right-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/arrow-down-right-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ArrowDownRightIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animation = {
    animate: shouldAnimate ? { x: [0, 2, 0], y: [0, 2, 0] } : { x: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M17 17l-10 -10" {...animation} />
        <m.path d="M8 17l9 0l0 -9" {...animation} />
      </m.svg>
    </LazyMotion>
  );
};

export default ArrowDownRightIcon;
```

---

### Arrow Left Icon <a name="arrow-left-icon"></a>

An animated arrow left icon.

- **Registry Name**: `arrow-left-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/arrow-left-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/arrow-left-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ArrowLeftIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animation = {
    animate: shouldAnimate ? { x: [0, -4, 0] } : { x: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M5 12l14 0" {...animation} />
        <m.path d="M5 12l6 6" {...animation} />
        <m.path d="M5 12l6 -6" {...animation} />
      </m.svg>
    </LazyMotion>
  );
};

export default ArrowLeftIcon;
```

---

### Arrow Right Icon <a name="arrow-right-icon"></a>

An animated arrow right icon that moves horizontally.

- **Registry Name**: `arrow-right-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/arrow-right-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/arrow-right-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface ArrowRightIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const ArrowRightIcon = (props: ArrowRightIconProps) => {
  const {
    size = 28,
    duration = 1,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const pathAnimationProps = {
    animate: shouldAnimate ? { x: [0, 4, 0] } : { x: 0 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M5 12l14 0" {...pathAnimationProps} />
        <m.path d="M13 18l6 -6" {...pathAnimationProps} />
        <m.path d="M13 6l6 6" {...pathAnimationProps} />
      </m.svg>
    </LazyMotion>
  );
};

export default ArrowRightIcon;
```

---

### Arrow Up Icon <a name="arrow-up-icon"></a>

An animated arrow up icon.

- **Registry Name**: `arrow-up-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/arrow-up-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/arrow-up-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ArrowUpIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animation = {
    animate: shouldAnimate ? { y: [0, -4, 0] } : { x: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M12 5l0 14" {...animation} />
        <m.path d="M18 11l-6 -6" {...animation} />
        <m.path d="M6 11l6 -6" {...animation} />
      </m.svg>
    </LazyMotion>
  );
};

export default ArrowUpIcon;
```

---

### Arrow Up Left Icon <a name="arrow-up-left-icon"></a>

An animated arrow up left icon.

- **Registry Name**: `arrow-up-left-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/arrow-up-left-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/arrow-up-left-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ArrowUpLeftIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animation = {
    animate: shouldAnimate ? { x: [0, -2, 0], y: [0, -2, 0] } : { x: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M7 7l10 10" {...animation} />
        <m.path d="M16 7l-9 0l0 9" {...animation} />
      </m.svg>
    </LazyMotion>
  );
};

export default ArrowUpLeftIcon;
```

---

### Arrow Up Right Icon <a name="arrow-up-right-icon"></a>

An animated arrow up right icon.

- **Registry Name**: `arrow-up-right-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/arrow-up-right-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/arrow-up-right-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface ArrowUpRightIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ArrowUpRightIcon = (props: ArrowUpRightIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 1.5, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    repeatDelay = 1, // Delay between animation loops (seconds)
    ease = "easeInOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const pathAnimationProps = {
    animate: shouldAnimate
      ? {
          x: [0, 2, 0],
          y: [0, -2, 0],
        }
      : { x: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M17 7l-10 10" {...pathAnimationProps} />
        <m.path d="M8 7l9 0l0 9" {...pathAnimationProps} />
      </m.svg>
    </LazyMotion>
  );
};

export default ArrowUpRightIcon;
```

---

### Battery Icon <a name="battery-icon"></a>

An animated battery icon.

- **Registry Name**: `battery-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/battery-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/battery-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface BatteryIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const BatteryIcon = (props: BatteryIconProps) => {
  const {
    size = 28,
    duration = 1,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const getBarProps = (barIndex: number) => {
    const appearTime = barIndex * 0.2;
    const resetTime = 0.9;

    return {
      animate: shouldAnimate
        ? {
            opacity: [0, 0, 1, 1, 0],
          }
        : { opacity: 0 },
      transition: {
        duration: isHovered ? duration * 0.5 : duration,
        ease: isHovered ? ("easeOut" as const) : ("linear" as const),
        repeat: isHovered ? 0 : Infinity,
        times: [
          0,
          appearTime,
          appearTime + (isHovered ? 0.1 : 0.05),
          resetTime,
          1,
        ],
      },
    };
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M6 7h11a2 2 0 0 1 2 2v.5a.5 .5 0 0 0 .5 .5a.5 .5 0 0 1 .5 .5v3a.5 .5 0 0 1 -.5 .5a.5 .5 0 0 0 -.5 .5v.5a2 2 0 0 1 -2 2h-11a2 2 0 0 1 -2 -2v-6a2 2 0 0 1 2 -2" />
        <m.path d="M7 10l0 4" {...getBarProps(0)} />
        <m.path d="M10 10l0 4" {...getBarProps(1)} />
        <m.path d="M13 10l0 4" {...getBarProps(2)} />
        <m.path d="M16 10l0 4" {...getBarProps(3)} />
      </m.svg>
    </LazyMotion>
  );
};

export default BatteryIcon;
```

---

### Bell Icon <a name="bell-icon"></a>

An animated bell icon that rings.

- **Registry Name**: `bell-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/bell-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/bell-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface BellIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const BellIcon = (props: BellIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 1.3, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    repeatDelay = 1, // Delay between animation loops (seconds)
    ease = "easeInOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animationProps = {
    animate: shouldAnimate
      ? {
          rotate: [0, -10, 10, -10, 10, 0, 0, 0],
        }
      : { rotate: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        style={{ originX: "12px", originY: "2px" }}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
        {...animationProps}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M10 5a2 2 0 0 1 4 0a7 7 0 0 1 4 6v3a4 4 0 0 0 2 3h-16a4 4 0 0 0 2 -3v-3a7 7 0 0 1 4 -6" />
        <path d="M9 17v1a3 3 0 0 0 6 0v-1" />
      </m.svg>
    </LazyMotion>
  );
};

export default BellIcon;
```

---

### Calendar Icon <a name="calendar-icon"></a>

An animated calendar icon.

- **Registry Name**: `calendar-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/calendar-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/calendar-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface CalendarIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const CalendarIcon = (props: CalendarIconProps) => {
  const {
    size = 28,
    duration = 2,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const dateAnimationProps = {
    animate: shouldAnimate
      ? {
          scale: [1, 1.2, 1],
          opacity: [0.7, 1, 0.7],
        }
      : { scale: 1, opacity: 1 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <rect x="4" y="5" width="16" height="16" rx="2" />
        <path d="M16 3v4" />
        <path d="M8 3v4" />
        <path d="M4 11h16" />
        <m.g style={{ transformOrigin: "12px 16px" }} {...dateAnimationProps}>
          <circle cx="12" cy="16" r="1.5" fill="currentColor" stroke="none" />
        </m.g>
      </m.svg>
    </LazyMotion>
  );
};

export default CalendarIcon;
```

---

### Cart Icon <a name="cart-icon"></a>

An animated cart icon.

- **Registry Name**: `cart-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/cart-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/cart-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const CartIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const transition = {
    duration: duration,
    ease: ease,
    repeat: isHovered ? 0 : Infinity,
    repeatDelay: repeatDelay,
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
        overflow="visible"
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.g
          animate={shouldAnimate ? { x: [0, 5, 0] } : { x: 0 }}
          transition={transition}
        >
          <circle cx="6" cy="19" r="2" />
          <circle cx="17" cy="19" r="2" />
          <path d="M17 17h-11v-14h-2" />
          <path d="M6 5l14 1l-1 7h-13" />
        </m.g>
      </m.svg>
    </LazyMotion>
  );
};

export default CartIcon;
```

---

### Check Icon <a name="check-icon"></a>

An animated check icon with drawing circle and checkmark.

- **Registry Name**: `check-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/check-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/check-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface CheckIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const CheckIcon = (props: CheckIconProps) => {
  const {
    size = 28,
    duration = 2.5,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const pathAnimationProps = {
    animate: shouldAnimate
      ? { pathLength: [0, 1, 1, 0], opacity: 1 }
      : { pathLength: 1, opacity: 1 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
      times: [0, 0.4, 0.8, 1],
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <circle cx="12" cy="12" r="9" fill="none" />
        <m.path d="M9 12l2 2l4 -4" {...pathAnimationProps} />
      </m.svg>
    </LazyMotion>
  );
};

export default CheckIcon;
```

---

### Chevron Down Icon <a name="chevron-down-icon"></a>

An animated chevron down icon.

- **Registry Name**: `chevron-down-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/chevron-down-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/chevron-down-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ChevronDownIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animation = {
    animate: shouldAnimate ? { y: [0, 4, 0] } : { x: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M6 9l6 6l6 -6" {...animation} />
      </m.svg>
    </LazyMotion>
  );
};

export default ChevronDownIcon;
```

---

### Chevron Left Icon <a name="chevron-left-icon"></a>

An animated chevron left icon.

- **Registry Name**: `chevron-left-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/chevron-left-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/chevron-left-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ChevronLeftIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animation = {
    animate: shouldAnimate ? { x: [0, -4, 0] } : { x: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M15 6l-6 6l6 6" {...animation} />
      </m.svg>
    </LazyMotion>
  );
};

export default ChevronLeftIcon;
```

---

### Chevron Right Icon <a name="chevron-right-icon"></a>

An animated chevron right icon.

- **Registry Name**: `chevron-right-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/chevron-right-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/chevron-right-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface ChevronRightIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ChevronRightIcon = (props: ChevronRightIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 1.5, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    repeatDelay = 1, // Delay between animation loops (seconds)
    ease = "easeInOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const pathAnimationProps = {
    animate: shouldAnimate ? { x: [0, 4, 0] } : { x: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M9 6l6 6l-6 6" {...pathAnimationProps} />
      </m.svg>
    </LazyMotion>
  );
};

export default ChevronRightIcon;
```

---

### Chevron Up Icon <a name="chevron-up-icon"></a>

An animated chevron up icon.

- **Registry Name**: `chevron-up-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/chevron-up-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/chevron-up-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ChevronUpIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animation = {
    animate: shouldAnimate ? { y: [0, -4, 0] } : { x: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M6 15l6 -6l6 6" {...animation} />
      </m.svg>
    </LazyMotion>
  );
};

export default ChevronUpIcon;
```

---

### Chevrons Down Icon <a name="chevrons-down-icon"></a>

An animated chevrons down icon.

- **Registry Name**: `chevrons-down-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/chevrons-down-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/chevrons-down-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ChevronsDownIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animation = {
    animate: shouldAnimate ? { y: [0, 4, 0] } : { x: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M7 7l5 5l5 -5" {...animation} />
        <m.path d="M7 13l5 5l5 -5" {...animation} />
      </m.svg>
    </LazyMotion>
  );
};

export default ChevronsDownIcon;
```

---

### Chevrons Left Icon <a name="chevrons-left-icon"></a>

An animated chevrons left icon.

- **Registry Name**: `chevrons-left-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/chevrons-left-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/chevrons-left-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ChevronsLeftIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animation = {
    animate: shouldAnimate ? { x: [0, -4, 0] } : { x: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M11 19l-7 -7l7 -7" {...animation} />
        <m.path d="M17 19l-7 -7l7 -7" {...animation} />
      </m.svg>
    </LazyMotion>
  );
};

export default ChevronsLeftIcon;
```

---

### Chevrons Right Icon <a name="chevrons-right-icon"></a>

An animated chevrons right icon.

- **Registry Name**: `chevrons-right-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/chevrons-right-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/chevrons-right-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ChevronsRightIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animation = {
    animate: shouldAnimate ? { x: [0, 4, 0] } : { x: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M7 7l5 5l-5 5" {...animation} />
        <m.path d="M13 7l5 5l-5 5" {...animation} />
      </m.svg>
    </LazyMotion>
  );
};

export default ChevronsRightIcon;
```

---

### Chevrons Up Icon <a name="chevrons-up-icon"></a>

An animated chevrons up icon.

- **Registry Name**: `chevrons-up-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/chevrons-up-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/chevrons-up-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ChevronsUpIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animation = {
    animate: shouldAnimate ? { y: [0, -4, 0] } : { x: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M7 11l5 -5l5 5" {...animation} />
        <m.path d="M7 17l5 -5l5 5" {...animation} />
      </m.svg>
    </LazyMotion>
  );
};

export default ChevronsUpIcon;
```

---

### Clock Icon <a name="clock-icon"></a>

An animated clock icon.

- **Registry Name**: `clock-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/clock-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/clock-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface ClockIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ClockIcon = (props: ClockIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 0.5, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    repeatDelay = 1, // Delay between animation loops (seconds)
    ease = "easeInOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const shakeProps = {
    animate: shouldAnimate
      ? {
          rotate: [0, -10, 10, -10, 10, 0],
          scale: [1, 1.1, 1.1, 1],
        }
      : { rotate: 0, scale: 1 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...shakeProps}
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M5 13a7 7 0 1 0 14 0a7 7 0 1 0 -14 0" />
        <path d="M7 4l-2.75 2" />
        <path d="M17 4l2.75 2" />
        <path d="M12 10l0 3l2 0" />
      </m.svg>
    </LazyMotion>
  );
};

export default ClockIcon;
```

---

### Close Icon <a name="close-icon"></a>

An animated close icon.

- **Registry Name**: `close-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/close-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/close-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface CloseIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const CloseIcon = (props: CloseIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 0.8, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    repeatDelay = 1, // Delay between animation loops (seconds)
    ease = "easeInOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const rotateProps = {
    animate: shouldAnimate
      ? {
          rotate: [0, -20, 20, -20, 20, 0],
        }
      : { rotate: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
        {...rotateProps}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M18 6l-12 12" />
        <path d="M6 6l12 12" />
      </m.svg>
    </LazyMotion>
  );
};

export default CloseIcon;
```

---

### Cloud Icon <a name="cloud-icon"></a>

An animated cloud icon.

- **Registry Name**: `cloud-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/cloud-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/cloud-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const CloudIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const transition = {
    duration: duration,
    ease: ease,
    repeat: isHovered ? 0 : Infinity,
    repeatDelay: repeatDelay,
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path
          d="M6.657 18c-2.572 0 -4.657 -2.007 -4.657 -4.483c0 -2.475 2.085 -4.482 4.657 -4.482c.393 -1.762 1.794 -3.2 3.675 -3.773c1.88 -.572 3.956 -.193 5.444 1c1.488 1.19 2.162 3.007 1.77 4.769h.99c1.913 0 3.464 1.56 3.464 3.486c0 1.927 -1.551 3.487 -3.465 3.487h-11.878"
          animate={shouldAnimate ? { x: [0, 2, 0] } : { x: 0 }}
          transition={transition}
        />
      </m.svg>
    </LazyMotion>
  );
};

export default CloudIcon;
```

---

### Copy Icon <a name="copy-icon"></a>

An animated copy icon with smooth motion effects.

- **Registry Name**: `copy-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/copy-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/copy-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface CopyIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const CopyIcon = (props: CopyIconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const rectAnimationProps = {
    animate: shouldAnimate
      ? {
          x: [8, 4, 8],
          y: [8, 4, 8],
        }
      : { x: 4, y: 4 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />

        <m.rect
          width="12"
          height="12"
          rx="2"
          stroke="currentColor"
          strokeWidth={strokeWidth}
          fill="none"
          {...rectAnimationProps}
        />

        <rect
          x="8"
          y="8"
          width="12"
          height="12"
          rx="2"
          className="fill-gray-100 dark:fill-[#111111]"
          stroke="currentColor"
          strokeWidth={strokeWidth}
        />
      </m.svg>
    </LazyMotion>
  );
};

export default CopyIcon;
```

---

### Credit Card Icon <a name="credit-card-icon"></a>

An animated credit card icon.

- **Registry Name**: `credit-card-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/credit-card-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/credit-card-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const CreditCardIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const transition = {
    duration: duration,
    ease: ease,
    repeat: isHovered ? 0 : Infinity,
    repeatDelay: repeatDelay,
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.g
          animate={shouldAnimate ? { rotateY: [0, 180, 360] } : { rotateY: 0 }}
          transition={{ ...transition, duration: duration * 2 }}
          style={{ originX: "50%", originY: "50%" }}
        >
          <rect x="3" y="5" width="18" height="14" rx="3" />
          <line x1="3" y1="10" x2="21" y2="10" />
          <line x1="7" y1="15" x2="7.01" y2="15" />
          <line x1="11" y1="15" x2="13" y2="15" />
        </m.g>
      </m.svg>
    </LazyMotion>
  );
};

export default CreditCardIcon;
```

---

### Crown Icon <a name="crown-icon"></a>

An animated crown icon.

- **Registry Name**: `crown-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/crown-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/crown-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface CrownIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const CrownIcon = (props: CrownIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 1, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    repeatDelay = 1.5, // Delay between animation loops (seconds)
    ease = "easeInOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const sparkle1Props = {
    animate: shouldAnimate
      ? {
          rotate: [0, -15, 15, -10, 10, -5, 5, 0],
          y: [0, -2, 0, -1, 0],
        }
      : { rotate: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
        {...sparkle1Props}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.g style={{ transformOrigin: "center" }}>
          <path d="M12 6l4 6l5 -4l-2 10h-14l-2 -10l5 4z" />
        </m.g>
      </m.svg>
    </LazyMotion>
  );
};

export default CrownIcon;
```

---

### Download Icon <a name="download-icon"></a>

An animated download icon with a bouncing arrow.

- **Registry Name**: `download-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/download-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/download-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface DownloadIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const DownloadIcon = (props: DownloadIconProps) => {
  const {
    size = 28,
    duration = 1,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const groupAnimationProps = {
    animate: shouldAnimate
      ? {
          y: [0, 3, 0],
        }
      : { y: 0 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M4 17v2a2 2 0 0 0 2 2h12a2 2 0 0 0 2 -2v-2" />
        <m.g {...groupAnimationProps}>
          <path d="M7 10l5 5l5 -5" />
          <path d="M12 3l0 12" />
        </m.g>
      </m.svg>
    </LazyMotion>
  );
};

export default DownloadIcon;
```

---

### External Link Icon <a name="external-link-icon"></a>

An animated external link icon.

- **Registry Name**: `external-link-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/external-link-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/external-link-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ExternalLinkIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const transition = {
    duration: duration,
    ease: ease,
    repeat: isHovered ? 0 : Infinity,
    repeatDelay: repeatDelay,
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M12 6h-6a2 2 0 0 0 -2 2v10a2 2 0 0 0 2 2h10a2 2 0 0 0 2 -2v-6" />
        <m.path
          d="M11 13l9 -9"
          animate={
            shouldAnimate ? { x: [0, 2, 0], y: [0, -2, 0] } : { x: 0, y: 0 }
          }
          transition={transition}
        />
        <m.path
          d="M15 4h5v5"
          animate={
            shouldAnimate ? { x: [0, 2, 0], y: [0, -2, 0] } : { x: 0, y: 0 }
          }
          transition={transition}
        />
      </m.svg>
    </LazyMotion>
  );
};

export default ExternalLinkIcon;
```

---

### Eye Icon <a name="eye-icon"></a>

An animated eye icon with blinking and looking animation.

- **Registry Name**: `eye-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/eye-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/eye-icon.tsx`

```tsx
"use client";

import { useState, useId } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface EyeIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const EyeIcon = (props: EyeIconProps) => {
  const {
    size = 28,
    duration = 3,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const clipId = useId();

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const eyeAnimation = {
    d: [
      "M3 12c2.4 -4 5.4 -6 9 -6c3.6 0 6.6 2 9 6c-2.4 4 -5.4 6 -9 6c-3.6 0 -6.6 -2 -9 -6z",
      "M3 12c2.4 4 5.4 6 9 6c3.6 0 6.6 -2 9 -6c-2.4 4 -5.4 6 -9 6c-3.6 0 -6.6 -2 -9 -6z",
      "M3 12c2.4 4 5.4 6 9 6c3.6 0 6.6 -2 9 -6c-2.4 4 -5.4 6 -9 6c-3.6 0 -6.6 -2 -9 -6z",
      "M3 12c2.4 -4 5.4 -6 9 -6c3.6 0 6.6 2 9 6c-2.4 4 -5.4 6 -9 6c-3.6 0 -6.6 -2 -9 -6z",
    ],
  };

  const lidAnimation = {
    d: [
      "M3 12c2.4 -4 5.4 -6 9 -6c3.6 0 6.6 2 9 6",
      "M3 12c2.4 4 5.4 6 9 6c3.6 0 6.6 -2 9 -6",
      "M3 12c2.4 4 5.4 6 9 6c3.6 0 6.6 -2 9 -6",
      "M3 12c2.4 -4 5.4 -6 9 -6c3.6 0 6.6 2 9 6",
    ],
  };

  const clipAnimationProps = {
    animate: shouldAnimate ? eyeAnimation : undefined,
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
      times: [0, 0.4, 0.6, 1],
    },
  };

  const lidAnimationProps = {
    animate: shouldAnimate ? lidAnimation : undefined,
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
      times: [0, 0.4, 0.6, 1],
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <defs>
          <clipPath id={clipId}>
            <m.path
              d="M3 12c2.4 -4 5.4 -6 9 -6c3.6 0 6.6 2 9 6c-2.4 4 -5.4 6 -9 6c-3.6 0 -6.6 -2 -9 -6z"
              {...clipAnimationProps}
            />
          </clipPath>
        </defs>
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <g clipPath={`url(#${clipId})`}>
          <path d="M10 12a2 2 0 1 0 4 0a2 2 0 0 0 -4 0" />
        </g>
        <path d="M21 12c-2.4 4 -5.4 6 -9 6c-3.6 0 -6.6 -2 -9 -6" />
        <m.path
          d="M3 12c2.4 -4 5.4 -6 9 -6c3.6 0 6.6 2 9 6"
          {...lidAnimationProps}
        />
      </m.svg>
    </LazyMotion>
  );
};

export default EyeIcon;
```

---

### Gift Icon <a name="gift-icon"></a>

An animated gift icon.

- **Registry Name**: `gift-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/gift-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/gift-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface GiftIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const GiftIcon = (props: GiftIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 0.8, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    repeatDelay = 1.5, // Delay between animation loops (seconds)
    ease = "easeInOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const shakeProps = {
    animate: shouldAnimate
      ? {
          rotate: [0, -8, 8, -6, 6, -3, 3, 0],
          y: [0, -2, 0, -1, 0],
        }
      : { rotate: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        style={{ transformOrigin: "center bottom" }}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
        {...shakeProps}
      >
        <m.g>
          <path stroke="none" d="M0 0h24v24H0z" fill="none" />
          <path d="M3 9a1 1 0 0 1 1 -1h16a1 1 0 0 1 1 1v2a1 1 0 0 1 -1 1h-16a1 1 0 0 1 -1 -1l0 -2" />
          <path d="M12 8l0 13" />
          <path d="M7.5 8a2.5 2.5 0 0 1 0 -5a4.8 8 0 0 1 4.5 5a4.8 8 0 0 1 4.5 -5a2.5 2.5 0 0 1 0 5" />
        </m.g>
        <path d="M19 12v7a2 2 0 0 1 -2 2h-10a2 2 0 0 1 -2 -2v-7" />
      </m.svg>
    </LazyMotion>
  );
};

export default GiftIcon;
```

---

### Globe Icon <a name="globe-icon"></a>

An animated globe icon that rotates continuously.

- **Registry Name**: `globe-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/globe-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/globe-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface GlobeIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const GlobeIcon = (props: GlobeIconProps) => {
  const {
    size = 28,
    duration = 8,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animationProps = {
    animate: shouldAnimate
      ? {
          rotate: 360,
        }
      : { rotate: 0 },
    transition: {
      duration: duration,
      ease: "linear" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
        {...animationProps}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M3 12a9 9 0 1 0 18 0a9 9 0 0 0 -18 0" />
        <path d="M3.6 9h16.8" />
        <path d="M3.6 15h16.8" />
        <path d="M11.5 3a17 17 0 0 0 0 18" />
        <path d="M12.5 3a17 17 0 0 1 0 18" />
      </m.svg>
    </LazyMotion>
  );
};

export default GlobeIcon;
```

---

### Heart Icon <a name="heart-icon"></a>

An animated heart icon that beats.

- **Registry Name**: `heart-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/heart-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/heart-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface HeartIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const HeartIcon = (props: HeartIconProps) => {
  const {
    size = 28,
    duration = 0.8,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const pathAnimationProps = {
    animate: shouldAnimate
      ? {
          scale: [1, 1.2, 1],
        }
      : { scale: 1 },
    transition: {
      duration: duration,
      repeat: isHovered ? 0 : Infinity,
      ease: "easeInOut" as const,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path
          d="M19.5 12.572l-7.5 7.428l-7.5 -7.428a5 5 0 1 1 7.5 -6.566a5 5 0 1 1 7.5 6.572"
          style={{ originX: "12px", originY: "12px" }}
          {...pathAnimationProps}
        />
      </m.svg>
    </LazyMotion>
  );
};

export default HeartIcon;
```

---

### Hourglass Icon <a name="hourglass-icon"></a>

An animated hourglass icon that rotates.

- **Registry Name**: `hourglass-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/hourglass-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/hourglass-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface HourglassIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const HourglassIcon = (props: HourglassIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 2, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    repeatDelay = 1, // Delay between animation loops (seconds)
    ease = "easeInOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animationProps = {
    animate: shouldAnimate
      ? {
          rotate: 180,
        }
      : { rotate: 0 },
    transition: {
      duration: duration,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
      ease: ease,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
        {...animationProps}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M6 20v-2a6 6 0 1 1 12 0v2a1 1 0 0 1 -1 1h-10a1 1 0 0 1 -1 -1" />
        <path d="M6 4v2a6 6 0 1 0 12 0v-2a1 1 0 0 0 -1 -1h-10a1 1 0 0 0 -1 1" />
      </m.svg>
    </LazyMotion>
  );
};

export default HourglassIcon;
```

---

### Layers Icon <a name="layers-icon"></a>

An animated layers icon.

- **Registry Name**: `layers-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/layers-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/layers-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface LayersIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const LayersIcon = (props: LayersIconProps) => {
  const {
    size = 28,
    duration = 1,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const layer1Props = {
    animate: shouldAnimate ? { y: [0, -3, 0] } : { y: 0 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  const layer2Props = {
    animate: shouldAnimate ? { y: [0, -1.5, 0] } : { y: 0 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
      delay: 0.1,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M12 4l-8 4l8 4l8 -4l-8 -4" {...layer1Props} />
        <m.path d="M4 12l8 4l8 -4" {...layer2Props} />
        <path d="M4 16l8 4l8 -4" />
      </m.svg>
    </LazyMotion>
  );
};

export default LayersIcon;
```

---

### Lightbulb Icon <a name="lightbulb-icon"></a>

An animated lightbulb icon with pulsing rays.

- **Registry Name**: `lightbulb-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/lightbulb-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/lightbulb-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface LightbulbIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const LightbulbIcon = (props: LightbulbIconProps) => {
  const {
    size = 28,
    duration = 2,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const pathAnimationProps = {
    animate: shouldAnimate ? { opacity: [0.2, 1, 0.2] } : { opacity: 1 },
    transition: {
      duration: duration,
      repeat: isHovered ? 0 : Infinity,
      ease: "easeInOut" as const,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path
          d="M3 12h1m8 -9v1m8 8h1m-15.4 -6.4l.7 .7m12.1 -.7l-.7 .7"
          {...pathAnimationProps}
        />
        <path d="M9 16a5 5 0 1 1 6 0a3.5 3.5 0 0 0 -1 3a2 2 0 0 1 -4 0a3.5 3.5 0 0 0 -1 -3" />
        <path d="M9.7 17l4.6 0" />
      </m.svg>
    </LazyMotion>
  );
};

export default LightbulbIcon;
```

---

### Location Icon <a name="location-icon"></a>

An animated location icon that rotates like a compass.

- **Registry Name**: `location-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/location-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/location-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface LocationIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  ease?: Easing;
}

const LocationIcon = (props: LocationIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 1, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    ease = "easeOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  // Shared transition properties
  const transition = {
    duration: duration,
    ease: ease,
    repeat: isHovered ? 0 : Infinity,
    repeatDelay: 1, // Reduced delay for smoother feel
  };

  // Pin bounce animation
  const pinVariants = {
    normal: { y: 0 },
    animate: {
      y: [0, -6, 0, -3, 0],
      transition: transition,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        {/* Location pin - animating group */}
        <m.g
          variants={pinVariants}
          initial="normal"
          animate={shouldAnimate ? "animate" : "normal"}
        >
          <path stroke="none" d="M0 0h24v24H0z" fill="none" />
          <path d="M9 11a3 3 0 1 0 6 0a3 3 0 0 0 -6 0" />
          <path d="M17.657 16.657l-4.243 4.243a2 2 0 0 1 -2.827 0l-4.244 -4.243a8 8 0 1 1 11.314 0z" />
        </m.g>
      </m.svg>
    </LazyMotion>
  );
};

export default LocationIcon;
```

---

### Lock Icon <a name="lock-icon"></a>

An animated lock icon that locks and unlocks.

- **Registry Name**: `lock-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/lock-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/lock-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface LockIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const LockIcon = (props: LockIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 1.2, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    repeatDelay = 1, // Delay between animation loops (seconds)
    ease = "easeInOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const pathAnimationProps = {
    animate: shouldAnimate
      ? { pathLength: [0, 1, 1, 0], opacity: 1 }
      : { pathLength: 1, opacity: 1 },
    transition: {
      duration: duration,
      repeat: isHovered ? 0 : Infinity,
      ease: ease,
      repeatDelay: repeatDelay,
      repeatType: "reverse" as const,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M5 13a2 2 0 0 1 2 -2h10a2 2 0 0 1 2 2v6a2 2 0 0 1 -2 2h-10a2 2 0 0 1 -2 -2v-6z" />
        <path d="M11 16a1 1 0 1 0 2 0a1 1 0 0 0 -2 0" />
        <m.path d="M8 11v-4a4 4 0 1 1 8 0v4" {...pathAnimationProps} />
      </m.svg>
    </LazyMotion>
  );
};

export default LockIcon;
```

---

### Mail Icon <a name="mail-icon"></a>

An animated mail icon with a moving envelope flap.

- **Registry Name**: `mail-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/mail-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/mail-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface MailIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const MailIcon = (props: MailIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 4, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    repeatDelay = 1, // Delay between animation loops (seconds)
    ease = "easeInOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const envelopeProps = {
    animate: shouldAnimate ? { y: [0, -2, 0] } : { y: 0 },
    transition: {
      duration: duration,
      repeat: isHovered ? 0 : Infinity,
      ease: ease,
    },
  };

  const flapProps = {
    animate: shouldAnimate
      ? { pathLength: [0, 1, 1, 0], opacity: 1 }
      : { pathLength: 1, opacity: 1 },
    transition: {
      duration: duration / 2,
      repeat: isHovered ? 0 : Infinity,
      ease: ease,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path
          d="M3 7a2 2 0 0 1 2 -2h14a2 2 0 0 1 2 2v10a2 2 0 0 1 -2 2h-14a2 2 0 0 1 -2 -2v-10z"
          {...envelopeProps}
        />
        <m.path d="M3 7l9 6l9 -6" {...flapProps} />
      </m.svg>
    </LazyMotion>
  );
};

export default MailIcon;
```

---

### Menu Icon <a name="menu-icon"></a>

An animated menu icon.

- **Registry Name**: `menu-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/menu-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/menu-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface IconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const MenuIcon = (props: IconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    repeatDelay = 1,
    ease = "easeInOut",
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const lineProps = {
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <m.line
          x1="4"
          x2="20"
          y1="12"
          y2="12"
          animate={shouldAnimate ? { opacity: [1, 0, 0, 1] } : { opacity: 1 }}
          {...lineProps}
        />
        <m.line
          x1="4"
          x2="20"
          y1="6"
          y2="6"
          animate={
            shouldAnimate
              ? { y: [0, 6, 6, 0], rotate: [0, 45, 45, 0] }
              : { y: 0, rotate: 0 }
          }
          style={{ originX: "50%", originY: "50%" }}
          {...lineProps}
        />
        <m.line
          x1="4"
          x2="20"
          y1="18"
          y2="18"
          animate={
            shouldAnimate
              ? { y: [0, -6, -6, 0], rotate: [0, -45, -45, 0] }
              : { y: 0, rotate: 0 }
          }
          style={{ originX: "50%", originY: "50%" }}
          {...lineProps}
        />
      </m.svg>
    </LazyMotion>
  );
};

export default MenuIcon;
```

---

### Message Icon <a name="message-icon"></a>

An animated message icon.

- **Registry Name**: `message-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/message-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/message-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface MessageIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const MessageIcon = (props: MessageIconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const dot1Props = {
    animate: shouldAnimate ? { opacity: [0.3, 1, 0.3] } : { opacity: 1 },
    transition: {
      duration: duration,
      repeat: isHovered ? 0 : Infinity,
      delay: 0,
    },
  };

  const dot2Props = {
    animate: shouldAnimate ? { opacity: [0.3, 1, 0.3] } : { opacity: 1 },
    transition: {
      duration: duration,
      repeat: isHovered ? 0 : Infinity,
      delay: duration * 0.2,
    },
  };

  const dot3Props = {
    animate: shouldAnimate ? { opacity: [0.3, 1, 0.3] } : { opacity: 1 },
    transition: { duration: duration, repeat: isHovered ? 0 : Infinity },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M4 21v-13a3 3 0 0 1 3 -3h10a3 3 0 0 1 3 3v6a3 3 0 0 1 -3 3h-9l-4 4" />
        <m.circle
          cx="8"
          cy="11"
          r="1"
          fill="currentColor"
          stroke="none"
          {...dot1Props}
        />
        <m.circle
          cx="12"
          cy="11"
          r="1"
          fill="currentColor"
          stroke="none"
          {...dot2Props}
        />
        <m.circle
          cx="16"
          cy="11"
          r="1"
          fill="currentColor"
          stroke="none"
          {...dot3Props}
        />
      </m.svg>
    </LazyMotion>
  );
};

export default MessageIcon;
```

---

### Moon Icon <a name="moon-icon"></a>

An animated moon icon.

- **Registry Name**: `moon-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/moon-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/moon-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface MoonIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const MoonIcon = (props: MoonIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 1, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    repeatDelay = 0.3, // Delay between animation loops (seconds)
    ease = "easeInOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const moonProps = {
    animate: shouldAnimate
      ? { rotate: [0, -20, 20, -10, 10, 0] }
      : { rotate: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
        {...moonProps}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M12 3c.132 0 .263 0 .393 0a7.5 7.5 0 0 0 7.92 12.446a9 9 0 1 1 -8.313 -12.454z" />
      </m.svg>
    </LazyMotion>
  );
};

export default MoonIcon;
```

---

### Music Icon <a name="music-icon"></a>

An animated music icon with dancing notes.

- **Registry Name**: `music-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/music-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/music-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface MusicIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const MusicIcon = (props: MusicIconProps) => {
  const {
    size = 28,
    duration = 2,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const groupAnimationProps = {
    animate: shouldAnimate
      ? { y: [0, -4, 0], rotate: [0, -5, 5, 0] }
      : { y: 0, rotate: 0 },
    transition: {
      duration: duration,
      repeat: isHovered ? 0 : Infinity,
      ease: "easeInOut" as const,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.g {...groupAnimationProps}>
          <path d="M3 17a3 3 0 1 0 6 0a3 3 0 0 0 -6 0" />
          <path d="M9 17v-13h10v9.5" />
          <path d="M9 8h10" />
        </m.g>
      </m.svg>
    </LazyMotion>
  );
};

export default MusicIcon;
```

---

### Refresh Icon <a name="refresh-icon"></a>

An animated refresh icon that rotates.

- **Registry Name**: `refresh-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/refresh-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/refresh-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface RefreshIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const RefreshIcon = (props: RefreshIconProps) => {
  const {
    size = 28,
    duration = 2,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animationProps = {
    animate: shouldAnimate ? { rotate: 360 } : { rotate: 0 },
    transition: {
      duration: duration,
      ease: "linear" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
        {...animationProps}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M20 11a8.1 8.1 0 0 0 -15.5 -2m-.5 -4v4h4" />
        <path d="M4 13a8.1 8.1 0 0 0 15.5 2m.5 4v-4h-4" />
      </m.svg>
    </LazyMotion>
  );
};

export default RefreshIcon;
```

---

### Rocket Icon <a name="rocket-icon"></a>

An animated rocket icon with launch thrust animation.

- **Registry Name**: `rocket-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/rocket-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/rocket-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface RocketIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const RocketIcon = (props: RocketIconProps) => {
  const {
    size = 28,
    duration = 2,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const bodyAnimationProps = {
    animate: shouldAnimate
      ? {
          y: [0, -2, 0],
          x: [0, 1, 0],
        }
      : { y: 0, x: 0 },
    transition: {
      duration: duration * 0.5,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  const flameAnimationProps = {
    animate: shouldAnimate
      ? {
          opacity: [0.4, 1, 0.4],
          scale: [0.8, 1.1, 0.8],
        }
      : { opacity: 0.4, scale: 0.8 },
    transition: {
      duration: duration * 0.15,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.g {...bodyAnimationProps}>
          <path d="M4 13a8 8 0 0 1 7 7a6 6 0 0 0 3 -5a9 9 0 0 0 6 -8a3 3 0 0 0 -3 -3a9 9 0 0 0 -8 6a6 6 0 0 0 -5 3" />
          <path d="M7 14a6 6 0 0 0 -3 6a6 6 0 0 0 6 -3" />
          <circle cx="15" cy="9" r="1" />
        </m.g>
        <m.g style={{ transformOrigin: "7px 17px" }} {...flameAnimationProps}>
          <path d="M4.5 17l-1.5 3" stroke="currentColor" strokeWidth={1.5} />
          <path d="M7 19l-1 2" stroke="currentColor" strokeWidth={1.5} />
        </m.g>
      </m.svg>
    </LazyMotion>
  );
};

export default RocketIcon;
```

---

### Scan Icon <a name="scan-icon"></a>

An animated scan icon with a moving scan line.

- **Registry Name**: `scan-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/scan-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/scan-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface ScanIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const ScanIcon = (props: ScanIconProps) => {
  const {
    size = 28,
    duration = 2,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const lineAnimationProps = {
    animate: shouldAnimate ? { y: [-8, 8, -8] } : { y: 0 },
    transition: {
      duration: duration,
      ease: "linear" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M4 7v-1a2 2 0 0 1 2 -2h2" />
        <path d="M4 17v1a2 2 0 0 0 2 2h2" />
        <path d="M16 4h2a2 2 0 0 1 2 2v1" />
        <path d="M16 20h2a2 2 0 0 0 2 -2v-1" />
        <m.line x1="5" y1="12" x2="19" y2="12" {...lineAnimationProps} />
      </m.svg>
    </LazyMotion>
  );
};

export default ScanIcon;
```

---

### Search Icon <a name="search-icon"></a>

An animated search icon.

- **Registry Name**: `search-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/search-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/search-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface SearchIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const SearchIcon = (props: SearchIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 1.2, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    repeatDelay = 1, // Delay between animation loops (seconds)
    ease = "easeInOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const groupAnimationProps = {
    animate: shouldAnimate ? { rotate: [0, -25, 15, 0] } : { rotate: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.g
          {...groupAnimationProps}
          style={{ originX: "21px", originY: "21px" }}
        >
          <m.circle cx="10" cy="10" r="7" />
          <m.path d="M21 21l-6 -6" />
        </m.g>
      </m.svg>
    </LazyMotion>
  );
};

export default SearchIcon;
```

---

### Send Icon <a name="send-icon"></a>

An animated send icon with paper airplane flying motion.

- **Registry Name**: `send-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/send-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/send-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface SendIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const SendIcon = (props: SendIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 1, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    repeatDelay = 1, // Delay between animation loops (seconds)
    ease = "easeInOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const groupAnimationProps = {
    animate: shouldAnimate
      ? {
          rotate: [0, 45, 0],
          scale: [1, 1.2, 1],
          x: [0, -2, 0], // slight translation as per request
          y: [0, 1, 0],
        }
      : { rotate: 0, scale: 1, x: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.g
          style={{ originX: "12px", originY: "12px" }}
          {...groupAnimationProps}
        >
          <path d="M10 14l11 -11" />
          <path d="M21 3l-6.5 18a.55 .55 0 0 1 -1 0l-3.5 -7l-7 -3.5a.55 .55 0 0 1 0 -1l18 -6.5" />
        </m.g>
      </m.svg>
    </LazyMotion>
  );
};

export default SendIcon;
```

---

### Settings Icon <a name="settings-icon"></a>

An animated settings icon that rotates.

- **Registry Name**: `settings-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/settings-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/settings-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface SettingsIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const SettingsIcon = (props: SettingsIconProps) => {
  const {
    size = 28,
    duration = 4,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const animationProps = {
    animate: shouldAnimate ? { rotate: 360 } : { rotate: 0 },
    transition: {
      duration: duration,
      ease: "linear" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
        {...animationProps}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M10.325 4.317c.426 -1.756 2.924 -1.756 3.35 0a1.724 1.724 0 0 0 2.573 1.066c1.543 -.94 3.31 .826 2.37 2.37a1.724 1.724 0 0 0 1.065 2.572c1.756 .426 1.756 2.924 0 3.35a1.724 1.724 0 0 0 -1.066 2.573c.94 1.543 -.826 3.31 -2.37 2.37a1.724 1.724 0 0 0 -2.572 1.065c-.426 1.756 -2.924 1.756 -3.35 0a1.724 1.724 0 0 0 -2.573 -1.066c-1.543 .94 -3.31 -.826 -2.37 -2.37a1.724 1.724 0 0 0 -1.065 -2.572c-1.756 -.426 -1.756 -2.924 0 -3.35a1.724 1.724 0 0 0 1.066 -2.573c-.94 -1.543 .826 -3.31 2.37 -2.37c1 .608 2.296 .07 2.572 -1.065z" />
        <path d="M9 12a3 3 0 1 0 6 0a3 3 0 0 0 -6 0" />
      </m.svg>
    </LazyMotion>
  );
};

export default SettingsIcon;
```

---

### Shield Icon <a name="shield-icon"></a>

An animated shield icon.

- **Registry Name**: `shield-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/shield-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/shield-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface ShieldIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const ShieldIcon = (props: ShieldIconProps) => {
  const {
    size = 28,
    duration = 2.5,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const shieldProps = {
    animate: shouldAnimate ? { scale: [1, 1.03, 1] } : { scale: 1 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  const checkProps = {
    animate: shouldAnimate
      ? { pathLength: [0, 1, 1, 0], opacity: 1 }
      : { pathLength: 1, opacity: 1 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
      times: [0, 0.4, 0.8, 1],
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path
          d="M12 3a12 12 0 0 0 8.5 3a12 12 0 0 1 -8.5 15a12 12 0 0 1 -8.5 -15a12 12 0 0 0 8.5 -3"
          style={{ transformOrigin: "12px 12px" }}
          {...shieldProps}
        />
        <m.path d="M9 12l2 2l4 -4" {...checkProps} />
      </m.svg>
    </LazyMotion>
  );
};

export default ShieldIcon;
```

---

### Sparkle Icon <a name="sparkle-icon"></a>

An animated sparkle icon with twinkling stars.

- **Registry Name**: `sparkle-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/sparkle-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/sparkle-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface SparkleIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const SparkleIcon = (props: SparkleIconProps) => {
  const {
    size = 28,
    duration = 2,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const mainSparkleProps = {
    animate: shouldAnimate
      ? { scale: [1, 1.1, 1], rotate: [0, 10, 0, -10, 0] }
      : { scale: 1, rotate: 0 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  const smallSparkle1Props = {
    animate: shouldAnimate
      ? { opacity: [0.3, 1, 0.3], scale: [0.8, 1.2, 0.8] }
      : { opacity: 1, scale: 1 },
    transition: {
      duration: duration * 0.8,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
      delay: duration * 0.2,
    },
  };

  const smallSparkle2Props = {
    animate: shouldAnimate
      ? { opacity: [0.3, 1, 0.3], scale: [0.8, 1.2, 0.8] }
      : { opacity: 1, scale: 1 },
    transition: {
      duration: duration * 0.8,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
      delay: duration * 0.5,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path
          d="M12 3l1.5 4.5l4.5 1.5l-4.5 1.5l-1.5 4.5l-1.5 -4.5l-4.5 -1.5l4.5 -1.5z"
          style={{ transformOrigin: "12px 9px" }}
          {...mainSparkleProps}
        />
        <m.path
          d="M18 16l.5 1.5l1.5 .5l-1.5 .5l-.5 1.5l-.5 -1.5l-1.5 -.5l1.5 -.5z"
          style={{ transformOrigin: "18px 18px" }}
          {...smallSparkle1Props}
        />
        <m.path
          d="M5 18l.5 1l1 .5l-1 .5l-.5 1l-.5 -1l-1 -.5l1 -.5z"
          style={{ transformOrigin: "5px 19px" }}
          {...smallSparkle2Props}
        />
      </m.svg>
    </LazyMotion>
  );
};

export default SparkleIcon;
```

---

### Star Icon <a name="star-icon"></a>

An animated star icon that twinkles and rotates.

- **Registry Name**: `star-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/star-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/star-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface StarIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const StarIcon = (props: StarIconProps) => {
  const {
    size = 28,
    duration = 2,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const pathAnimationProps = {
    animate: shouldAnimate
      ? {
          scale: [1, 1.2, 1],
          rotate: [0, 5, -5, 0],
        }
      : { scale: 1, rotate: 0 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path
          d="M12 17.75l-6.172 3.245l1.179 -6.873l-5 -4.867l6.9 -1l3.086 -6.253l3.086 6.253l6.9 1l-5 4.867l1.179 6.873z"
          style={{ originX: "12px", originY: "12px" }}
          {...pathAnimationProps}
        />
      </m.svg>
    </LazyMotion>
  );
};

export default StarIcon;
```

---

### Sun Icon <a name="sun-icon"></a>

An animated sun icon.

- **Registry Name**: `sun-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/sun-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/sun-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface SunIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const SunIcon = (props: SunIconProps) => {
  const {
    size = 28,
    duration = 4,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const rotateProps = {
    animate: shouldAnimate ? { rotate: 360 } : { rotate: 0 },
    transition: {
      duration: duration,
      ease: "linear" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
        {...rotateProps}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <circle cx="12" cy="12" r="4" />
        <path d="M12 2v2" />
        <path d="M12 20v2" />
        <path d="M4.93 4.93l1.41 1.41" />
        <path d="M17.66 17.66l1.41 1.41" />
        <path d="M2 12h2" />
        <path d="M20 12h2" />
        <path d="M6.34 17.66l-1.41 1.41" />
        <path d="M19.07 4.93l-1.41 1.41" />
      </m.svg>
    </LazyMotion>
  );
};

export default SunIcon;
```

---

### Thumbs Up Icon <a name="thumbs-up-icon"></a>

An animated thumbs up icon.

- **Registry Name**: `thumbs-up-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/thumbs-up-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/thumbs-up-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import {
  m,
  LazyMotion,
  domAnimation,
  SVGMotionProps,
  Easing,
} from "motion/react";

interface ThumbsUpIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
  repeatDelay?: number;
  ease?: Easing;
}

const ThumbsUpIcon = (props: ThumbsUpIconProps) => {
  const {
    size = 28, // Icon size in pixels
    duration = 0.8, // Animation duration in seconds
    strokeWidth = 2, // SVG stroke width
    isHovered = false, // When true, animate only on hover
    repeatDelay = 1.2, // Delay between animation loops (seconds)
    ease = "easeInOut", // Animation easing function
    className,
    ...restProps
  } = props;

  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const thumbProps = {
    animate: shouldAnimate
      ? {
          rotate: [0, -15, 5, -10, 0],
          y: [0, -2, 0],
        }
      : { rotate: 0, y: 0 },
    transition: {
      duration: duration,
      ease: ease,
      repeat: isHovered ? 0 : Infinity,
      repeatDelay: repeatDelay,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        style={{ transformOrigin: "center bottom" }}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
        {...thumbProps}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M7 11v8a1 1 0 0 1 -1 1h-2a1 1 0 0 1 -1 -1v-7a1 1 0 0 1 1 -1h3a4 4 0 0 0 4 -4v-1a2 2 0 0 1 4 0v5h3a2 2 0 0 1 2 2l-1 5a2 3 0 0 1 -2 2h-7a3 3 0 0 1 -3 -3" />
      </m.svg>
    </LazyMotion>
  );
};

export default ThumbsUpIcon;
```

---

### Trash Icon <a name="trash-icon"></a>

An animated trash icon.

- **Registry Name**: `trash-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/trash-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/trash-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface TrashIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const TrashIcon = (props: TrashIconProps) => {
  const {
    size = 28,
    duration = 1,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const lidProps = {
    animate: shouldAnimate
      ? {
          y: [-1],
          rotate: [0, -10, 10, -5, 5, 0],
        }
      : { y: 0, rotate: 0 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.g {...lidProps}>
          <path d="M4 7l16 0" />
          <path d="M9 7v-3a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v3" />
        </m.g>
        <path d="M10 11l0 6" />
        <path d="M14 11l0 6" />
        <path d="M5 7l1 12a2 2 0 0 0 2 2h8a2 2 0 0 0 2 -2l1 -12" />
      </m.svg>
    </LazyMotion>
  );
};

export default TrashIcon;
```

---

### Upload Icon <a name="upload-icon"></a>

An animated upload icon.

- **Registry Name**: `upload-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/upload-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/upload-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface UploadIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const UploadIcon = (props: UploadIconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const groupAnimationProps = {
    animate: shouldAnimate ? { y: [0, -3, 0] } : { y: 0 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M4 17v2a2 2 0 0 0 2 2h12a2 2 0 0 0 2 -2v-2" />
        <m.g {...groupAnimationProps}>
          <path d="M7 9l5 -5l5 5" />
          <path d="M12 4l0 12" />
        </m.g>
      </m.svg>
    </LazyMotion>
  );
};

export default UploadIcon;
```

---

### Volume Icon <a name="volume-icon"></a>

An animated volume icon with pulsing sound waves.

- **Registry Name**: `volume-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/volume-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/volume-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface VolumeIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const VolumeIcon = (props: VolumeIconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const innerWaveProps = {
    animate: shouldAnimate
      ? {
          opacity: [0.5, 1, 0.5, 1, 0.5],
          scaleX: [1, 1.05, 0.95, 1.05, 1],
          x: [0, 0.5, -0.25, 0.5, 0],
        }
      : { opacity: 1, scaleX: 1, x: 0 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  const outerWaveProps = {
    animate: shouldAnimate
      ? {
          opacity: [0.3, 1, 0.3, 1, 0.3],
          scaleX: [1, 1.1, 0.95, 1.1, 1],
          x: [0, 1, -0.25, 1, 0],
        }
      : { opacity: 1, scaleX: 1, x: 0 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
      delay: 0.1,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        overflow="visible"
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path d="M6 15h-2a1 1 0 0 1 -1 -1v-4a1 1 0 0 1 1 -1h2l3.5 -4.5a.8 .8 0 0 1 1.5 .5v14a.8 .8 0 0 1 -1.5 .5l-3.5 -4.5" />
        <m.path
          d="M15 8a5 5 0 0 1 0 8"
          style={{ transformOrigin: "15px 12px" }}
          {...innerWaveProps}
        />
        <m.path
          d="M18 5a9 9 0 0 1 0 14"
          style={{ transformOrigin: "18px 12px" }}
          {...outerWaveProps}
        />
      </m.svg>
    </LazyMotion>
  );
};

export default VolumeIcon;
```

---

### Wavy Icon <a name="wavy-icon"></a>

An animated wavy icon with smooth motion effects.

- **Registry Name**: `wavy-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/wavy-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/wavy-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface WavyIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const WavyIcon = (props: WavyIconProps) => {
  const {
    size = 28,
    duration = 0.8,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  // Use internal hover state when isHovered prop is true, otherwise animate infinitely
  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const wave1Props = {
    animate: shouldAnimate
      ? {
          d: [
            "M3 7c3 -3 6 -3 9 0s6 3 9 0",
            "M3 9c3 0 6 -4 9 -4s6 4 9 4",
            "M3 7c3 3 6 3 9 0s6 -3 9 0",
            "M3 5c3 0 6 4 9 4s6 -4 9 -4",
            "M3 7c3 -3 6 -3 9 0s6 3 9 0",
          ],
        }
      : undefined,
    transition: {
      duration: duration,
      ease: "linear" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  const wave2Props = {
    animate: shouldAnimate
      ? {
          d: [
            "M3 12c3 -3 6 -3 9 0s6 3 9 0",
            "M3 14c3 0 6 -4 9 -4s6 4 9 4",
            "M3 12c3 3 6 3 9 0s6 -3 9 0",
            "M3 10c3 0 6 4 9 4s6 -4 9 -4",
            "M3 12c3 -3 6 -3 9 0s6 3 9 0",
          ],
        }
      : undefined,
    transition: {
      duration: duration,
      ease: "linear" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  const wave3Props = {
    animate: shouldAnimate
      ? {
          d: [
            "M3 17c3 -3 6 -3 9 0s6 3 9 0",
            "M3 19c3 0 6 -4 9 -4s6 4 9 4",
            "M3 17c3 3 6 3 9 0s6 -3 9 0",
            "M3 15c3 0 6 4 9 4s6 -4 9 -4",
            "M3 17c3 -3 6 -3 9 0s6 3 9 0",
          ],
        }
      : undefined,
    transition: {
      duration: duration,
      ease: "linear" as const,
      repeat: isHovered ? 0 : Infinity,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path
          d="M3 7c3 -2 6 -2 9 0s6 2 9 0"
          stroke="currentColor"
          fill="none"
          {...wave1Props}
        />
        <m.path
          d="M3 12c3 -2 6 -2 9 0s6 2 9 0"
          stroke="currentColor"
          fill="none"
          {...wave2Props}
        />
        <m.path
          d="M3 17c3 -2 6 -2 9 0s6 2 9 0"
          stroke="currentColor"
          fill="none"
          {...wave3Props}
        />
      </m.svg>
    </LazyMotion>
  );
};

export default WavyIcon;
```

---

### Wifi Icon <a name="wifi-icon"></a>

An animated wifi icon with pulsing signal bars.

- **Registry Name**: `wifi-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/wifi-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/wifi-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface WifiIconProps extends SVGMotionProps<SVGSVGElement> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const WifiIcon = (props: WifiIconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const path1Props = {
    animate: shouldAnimate ? { opacity: [0.3, 1, 0.3] } : { opacity: 1 },
    transition: {
      duration: duration,
      repeat: isHovered ? 0 : Infinity,
      delay: 0,
    },
  };

  const path2Props = {
    animate: shouldAnimate ? { opacity: [0.3, 1, 0.3] } : { opacity: 1 },
    transition: {
      duration: duration,
      repeat: isHovered ? 0 : Infinity,
      delay: 0.2,
    },
  };

  const path3Props = {
    animate: shouldAnimate ? { opacity: [0.3, 1, 0.3] } : { opacity: 1 },
    transition: {
      duration: duration,
      repeat: isHovered ? 0 : Infinity,
      delay: 0.4,
    },
  };

  const path4Props = {
    animate: shouldAnimate ? { opacity: [0.3, 1, 0.3] } : { opacity: 1 },
    transition: {
      duration: duration,
      repeat: isHovered ? 0 : Infinity,
      delay: 0.6,
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path d="M12 18l.01 0" {...path1Props} />
        <m.path d="M9.172 15.172a4 4 0 0 1 5.656 0" {...path2Props} />
        <m.path d="M6.343 12.343a8 8 0 0 1 11.314 0" {...path3Props} />
        <m.path
          d="M3.515 9.515c4.686 -4.687 12.284 -4.687 17 0"
          {...path4Props}
        />
      </m.svg>
    </LazyMotion>
  );
};

export default WifiIcon;
```

---

### Zap Icon <a name="zap-icon"></a>

An animated zap icon with electric flash effect.

- **Registry Name**: `zap-icon`
- **Type**: `registry:component`

#### Installation

```bash
npx shadcn@latest add https://www.chamaac.com/r/zap-icon.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### How to Use

```tsx
// Usage example not available
```

#### Component Source Code

##### File: `registry/chamaac/animated-icons/zap-icon.tsx`

```tsx
"use client";

import { useState } from "react";
import { m, LazyMotion, domAnimation, SVGMotionProps } from "motion/react";

interface ZapIconProps
  extends Omit<SVGMotionProps<SVGSVGElement>, "strokeWidth"> {
  size?: number;
  duration?: number;
  strokeWidth?: number;
  isHovered?: boolean;
}

const ZapIcon = (props: ZapIconProps) => {
  const {
    size = 28,
    duration = 1.5,
    strokeWidth = 2,
    isHovered = false,
    className,
    ...restProps
  } = props;
  const [isHoveredInternal, setIsHoveredInternal] = useState(false);

  const shouldAnimate = isHovered ? isHoveredInternal : true;

  const pathAnimationProps = {
    animate: shouldAnimate
      ? {
          opacity: [1, 0.4, 1, 0.6, 1],
          scale: [1, 1.02, 0.98, 1.01, 1],
        }
      : { opacity: 1, scale: 1 },
    transition: {
      duration: duration,
      ease: "easeInOut" as const,
      repeat: isHovered ? 0 : Infinity,
      times: [0, 0.2, 0.4, 0.7, 1],
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.svg
        {...restProps}
        xmlns="http://www.w3.org/2000/svg"
        width={size}
        height={size}
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        strokeLinecap="round"
        strokeLinejoin="round"
        className={className}
        onMouseEnter={() => isHovered && setIsHoveredInternal(true)}
        onMouseLeave={() => isHovered && setIsHoveredInternal(false)}
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <m.path
          d="M13 3l0 7l6 0l-8 11l0 -7l-6 0l8 -11"
          style={{ transformOrigin: "center" }}
          {...pathAnimationProps}
        />
      </m.svg>
    </LazyMotion>
  );
};

export default ZapIcon;
```

---
