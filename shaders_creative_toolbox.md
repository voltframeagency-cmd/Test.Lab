# 🌌 Shaders.com Creative Toolbox & Guide

This document contains the scraped and compiled core guide and reference documentation for the **Shaders.com** WebGL/WebGPU component library. It includes details on all **116 creative components** across their respective categories.

---

## 📖 Core Concepts & Guide

What is Shaders?Shaders is a component library for building GPU-accelerated visual effects directly in the browser. You describe what you want using the same declarative, component-based syntax you already know from building your frontend UI — and Shaders handles all the GPU-side work for you.How does it work?You define components and props within a <Shader> tag, and we compile that into a WebGPU shader program, and handle any blending/masking/etc. to output the final look.It all renders to a single <canvas> element. You size and position it with CSS, just like any other HTML element. No matter how many nested layers you define, the result is always a single visual element.<Shader class="absolute inset-0 -z-10">
  <LinearGradient colorA="#0f172a" colorB="#7c3aed" />
  <CursorTrail />
</Shader>
There's no crazy math to learn, no GLSL to write, and no render loop to manage. Changes to prop values update instantly without recompiling the shader, keeping things lightweight for the browser.What can you build?You can build a TON of different effects with Shaders, ranging from hero section backgrounds to interactive and stylized media. Every component comes with configurable props that, when combined, can create a wide range of different interesting visuals. Power features like masking, blending, and dynamic props provide even more ways to build something that's truly unique.Supported frameworksShaders ships a single package with dedicated entry points for each framework. Component-based frameworks (Vue, React, Svelte, Solid) share the same declarative API. The JavaScript API uses a preset config approach better suited to imperative workflows.RequirementsShaders uses WebGPU, which is supported in most modern browsers now. When WebGPU is unavailable, we fallback gracefully to WebGLv2.

---

## 🗂️ Component Directory by Category

### Category Overview
| Category | Components Count | Description |
| --- | --- | --- |
| **Adjustments** | 12 | Color correction and post-processing adjustments. |
| **Blurs** | 8 | Various blur algorithms used to create depth of field, motion, or backdrop effects. |
| **Distortions** | 16 | Mathematical coordinate warps that twist, wave, or bend underlying textures. |
| **Interactive** | 8 | Shaders that respond directly to user mouse, touch, or drag inputs. |
| **Shape Effects** | 5 | GPU-driven styling applied specifically to shape elements. |
| **Shapes** | 11 | Procedural vector shapes constructed directly on the GPU. |
| **Stylize** | 16 | Filters that manipulate rendering styles to create distinct artistic or retro aesthetics. |
| **Textures** | 40 | Procedural or media-driven textures that can be mapped onto shapes or used to drive other effects. |

---

## 🏁 Adjustments

Color correction and post-processing adjustments.

### 📦 `<BrightnessContrast>`
*Adjust brightness and contrast of the image*

[Official Documentation](https://shaders.com/docs/components/brightnesscontrast)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `brightness` | `number` | `0` | Brightness adjustment (-1 to 1) |
| `contrast` | `number` | `0` | Contrast adjustment (-1 to 1) |

#### React JSX Usage
```tsx
<Shader>
  <BrightnessContrast>
    <Circle />
  </BrightnessContrast>
</Shader>
```

---

### 📦 `<Duotone>`
*Map colors to two tones based on luminance*

[Official Documentation](https://shaders.com/docs/components/duotone)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#ff0000` | First color (used for darker areas) |
| `colorB` | `string` | `#023af4` | Second color (used for brighter areas) |
| `blend` | `number` | `0.5` | Blend point between the two colors |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <Duotone>
    <Circle />
  </Duotone>
</Shader>
```

---

### 📦 `<Grayscale>`
*Convert colors to black and white*

[Official Documentation](https://shaders.com/docs/components/grayscale)

#### React JSX Usage
```tsx
<Shader>
  <Grayscale>
    <Circle />
  </Grayscale>
</Shader>
```

---

### 📦 `<HueShift>`
*Rotate hue around the color wheel*

[Official Documentation](https://shaders.com/docs/components/hueshift)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `shift` | `number` | `0` | The amount to shift the hue by |

#### React JSX Usage
```tsx
<Shader>
  <HueShift>
    <Circle />
  </HueShift>
</Shader>
```

---

### 📦 `<Invert>`
*Invert RGB colors while preserving alpha*

[Official Documentation](https://shaders.com/docs/components/invert)

#### React JSX Usage
```tsx
<Shader>
  <Invert>
    <Circle />
  </Invert>
</Shader>
```

---

### 📦 `<Posterize>`
*Reduce color depth to create a poster effect*

[Official Documentation](https://shaders.com/docs/components/posterize)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `5` | The intensity of the posterization effect (lower is more posterized) |

#### React JSX Usage
```tsx
<Shader>
  <Posterize
    intensity={5}
  >
    <Circle />
  </Posterize>
</Shader>
```

---

### 📦 `<Saturation>`
*Adjust color saturation intensity*

[Official Documentation](https://shaders.com/docs/components/saturation)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `1` | The intensity of the saturation effect (1 being no change) |

#### React JSX Usage
```tsx
<Shader>
  <Saturation
    intensity={1}
  >
    <Circle />
  </Saturation>
</Shader>
```

---

### 📦 `<Sharpness>`
*Adjust image sharpness using a convolution kernel*

[Official Documentation](https://shaders.com/docs/components/sharpness)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `sharpness` | `number` | `0` | How sharp to make the underlying image |

#### React JSX Usage
```tsx
<Shader>
  <Sharpness>
    <Circle />
  </Sharpness>
</Shader>
```

---

### 📦 `<Solarize>`
*Inverts tones above a luminance threshold — a classic darkroom and photo effect*

[Official Documentation](https://shaders.com/docs/components/solarize)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `threshold` | `number` | `0.5` | Luminance level above which colors are inverted. Pixels brighter than this threshold get flipped. |
| `strength` | `number` | `1` | Blend between original (0) and fully solarized (1) |

#### React JSX Usage
```tsx
<Shader>
  <Solarize>
    <Circle />
  </Solarize>
</Shader>
```

---

### 📦 `<Tint>`
*Apply a color tint to the image*

[Official Documentation](https://shaders.com/docs/components/tint)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ff8800` | Tint color |
| `amount` | `number` | `0.5` | Tint amount (0 = no tint, 1 = full tint) |
| `preserveLuminosity` | `boolean` | `true` | Preserve original brightness |

#### React JSX Usage
```tsx
<Shader>
  <Tint
    color="#ff8800"
  >
    <Circle />
  </Tint>
</Shader>
```

---

### 📦 `<Tritone>`
*Map colors to three tones: shadows, midtones, highlights*

[Official Documentation](https://shaders.com/docs/components/tritone)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#ce1bea` | First color (used for shadows/darkest areas) |
| `colorB` | `string` | `#2fff00` | Second color (used for midtones) |
| `colorC` | `string` | `#ffff00` | Third color (used for highlights/brightest areas) |
| `blendMid` | `number` | `0.5` | Midpoint position between the three colors |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <Tritone>
    <Circle />
  </Tritone>
</Shader>
```

---

### 📦 `<Vibrance>`
*Selective saturation adjustment protecting skin tones*

[Official Documentation](https://shaders.com/docs/components/vibrance)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `0` | The intensity of the vibrance effect |

#### React JSX Usage
```tsx
<Shader>
  <Vibrance
    intensity={0}
  >
    <Circle />
  </Vibrance>
</Shader>
```

---

## 🏁 Blurs

Various blur algorithms used to create depth of field, motion, or backdrop effects.

### 📦 `<AngularBlur>`
*Radial motion blur rotating around a center point*

[Official Documentation](https://shaders.com/docs/components/angularblur)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `20` | Intensity of the angular blur effect |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | The center point of the rotation |

#### React JSX Usage
```tsx
<Shader>
  <AngularBlur
    intensity={20}
  >
    <Circle />
  </AngularBlur>
</Shader>
```

---

### 📦 `<Blur>`
*A simple Gaussian blur effect*

[Official Documentation](https://shaders.com/docs/components/blur)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `50` | Intensity of the blur effect |

#### React JSX Usage
```tsx
<Shader>
  <Blur
    intensity={50}
  >
    <Circle />
  </Blur>
</Shader>
```

---

### 📦 `<ChannelBlur>`
*Independent blur for red, green, and blue channels*

[Official Documentation](https://shaders.com/docs/components/channelblur)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `redIntensity` | `number` | `0` | Blur intensity for red channel |
| `greenIntensity` | `number` | `20` | Blur intensity for green channel |
| `blueIntensity` | `number` | `40` | Blur intensity for blue channel |

#### React JSX Usage
```tsx
<Shader>
  <ChannelBlur>
    <Circle />
  </ChannelBlur>
</Shader>
```

---

### 📦 `<DiffuseBlur>`
*Grain-like pixel displacement at random*

[Official Documentation](https://shaders.com/docs/components/diffuseblur)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `30` | Intensity of the diffuse blur effect |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `stretch` | How to handle edges when distortion pushes content out of bounds |

#### React JSX Usage
```tsx
<Shader>
  <DiffuseBlur
    intensity={30}
  >
    <Circle />
  </DiffuseBlur>
</Shader>
```

---

### 📦 `<LinearBlur>`
*Directional motion blur in a specific angle*

[Official Documentation](https://shaders.com/docs/components/linearblur)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `30` | Intensity of the linear blur effect |
| `angle` | `number` | `0` | Direction of the linear blur (in degrees) |

#### React JSX Usage
```tsx
<Shader>
  <LinearBlur
    intensity={30}
  >
    <Circle />
  </LinearBlur>
</Shader>
```

---

### 📦 `<ProgressiveBlur>`
*Blur that increases progressively in one direction*

[Official Documentation](https://shaders.com/docs/components/progressiveblur)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `50` | Maximum intensity of the blur effect |
| `angle` | `number` | `0` | Direction of the blur gradient (in degrees) |
| `center` | `{x: number, y: number}` | `{"x":0,"y":0.5}` | Center point where blur begins |
| `falloff` | `number` | `1` | Distance over which blur transitions to full strength |

#### React JSX Usage
```tsx
<Shader>
  <ProgressiveBlur
    intensity={50}
  >
    <Circle />
  </ProgressiveBlur>
</Shader>
```

---

### 📦 `<TiltShift>`
*Selective focus blur mimicking tilt-shift photography*

[Official Documentation](https://shaders.com/docs/components/tiltshift)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `50` | Maximum blur intensity at edges |
| `width` | `number` | `0.3` | Width of the sharp focus area |
| `falloff` | `number` | `0.3` | Distance over which blur transitions to full strength |
| `angle` | `number` | `0` | Rotation angle of the focus line (in degrees) |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center point of the focus line |

#### React JSX Usage
```tsx
<Shader>
  <TiltShift
    intensity={50}
  >
    <Circle />
  </TiltShift>
</Shader>
```

---

### 📦 `<ZoomBlur>`
*Radial zoom blur expanding from a center point*

[Official Documentation](https://shaders.com/docs/components/zoomblur)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `30` | Intensity of the zoom blur effect |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center point of the zoom blur |

#### React JSX Usage
```tsx
<Shader>
  <ZoomBlur
    intensity={30}
  >
    <Circle />
  </ZoomBlur>
</Shader>
```

---

## 🏁 Distortions

Mathematical coordinate warps that twist, wave, or bend underlying textures.

### 📦 `<BarShift>`
*Slices content into parallel bars, each offset independently for a fractured or glitch-like effect*

[Official Documentation](https://shaders.com/docs/components/barshift)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `count` | `number` | `6` | Number of bars across the longest viewport dimension |
| `angle` | `number` | `0` | Angle of bar orientation in degrees (0 = vertical bars, 90 = horizontal bars) |
| `intensity` | `number` | `0.15` | Maximum displacement per bar |
| `seed` | `number` | `0` | Randomization seed for per-bar offset variation |
| `speed` | `number` | `0` | Animation speed — each bar drifts at its own rate and direction |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `mirror` | How to handle edges when distortion pushes content out of bounds |

#### React JSX Usage
```tsx
<Shader>
  <BarShift
    intensity={0.15}
  >
    <Circle />
  </BarShift>
</Shader>
```

---

### 📦 `<Bulge>`
*Magnify or pinch content around a center point*

[Official Documentation](https://shaders.com/docs/components/bulge)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | The center point of the bulge effect |
| `strength` | `number` | `1` | The intensity of the bulge effect (positive = bulge out, negative = pinch in) |
| `radius` | `number` | `1` | The radius of the bulge effect area |
| `falloff` | `number` | `0.5` | Controls the smoothness of the transition (0 = hard edge, 1 = very smooth) |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `stretch` | How to handle edges when distortion pushes content out of bounds |

#### React JSX Usage
```tsx
<Shader>
  <Bulge
    radius={1}
  >
    <Circle />
  </Bulge>
</Shader>
```

---

### 📦 `<ConcentricSpin>`
*Concentric rings that each rotate the underlying image by different amounts*

[Official Documentation](https://shaders.com/docs/components/concentricspin)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `20` | Maximum rotation angle per ring |
| `rings` | `number` | `8` | Number of concentric rings |
| `smoothness` | `number` | `0.03` | Softness of transitions between rings |
| `seed` | `number` | `0` | Randomization seed for per-ring rotation variation |
| `speed` | `number` | `0.1` | Speed of continuous ring rotation |
| `speedRandomness` | `number` | `0.5` | How much each ring varies in rotation speed and direction |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `mirror` | How to handle edges when distortion pushes content out of bounds |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center point of the concentric rings |

#### React JSX Usage
```tsx
<Shader>
  <ConcentricSpin
    intensity={20}
  >
    <Circle />
  </ConcentricSpin>
</Shader>
```

---

### 📦 `<FlowField>`
*Fluid-like distortion with constant smooth motion*

[Official Documentation](https://shaders.com/docs/components/flowfield)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `strength` | `number` | `0.15` | Intensity of the flow distortion |
| `detail` | `number` | `2` | Scale of the flow patterns |
| `speed` | `number` | `0` | Speed of the flow |
| `evolutionSpeed` | `number` | `0` | How fast the flow field pattern reshapes over time |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `mirror` | How to handle edges when distortion pushes content out of bounds |

#### React JSX Usage
```tsx
<Shader>
  <FlowField>
    <Circle />
  </FlowField>
</Shader>
```

---

### 📦 `<FlutedGlass>`
*Full-screen fluted glass effect — refracts content through repeating cylindrical bars*

[Official Documentation](https://shaders.com/docs/components/flutedglass)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `shape` | `"bars" \| "rounded" \| "waves"` | `bars` | Cross-section shape of each flute |
| `angle` | `number` | `0` | Direction of the flutes in degrees (0 = vertical bars) |
| `frequency` | `number` | `10` | Number of flutes across the longest viewport axis |
| `softness` | `number` | `0.5` | How smoothly distortion fades from each flute centre to its edge (0 = flat middle / sharp seams, 1 = gentle curve) |
| `waveAmplitude` | `number` | `0.06` | How far each flute sways horizontally as it travels (Waves shape only) |
| `waveFrequency` | `number` | `1.5` | How many sways fit along each flute (Waves shape only) |
| `speed` | `number` | `0` | Animation speed — drifts the flute pattern over time and flows wave perturbations |
| `refraction` | `number` | `1.5` | How aggressively each flute bends content beneath it |
| `aberration` | `number` | `0.2` | Chromatic aberration — splits RGB along the refraction direction at flute seams |
| `lightAngle` | `number` | `30` | Direction the light source is coming from (0 = head-on, 90 = grazing) |
| `highlight` | `number` | `0.2` | Strength of the specular reflection on each flute |
| `highlightSoftness` | `number` | `0.3` | Spread of the specular peak (0 = pin-tight, 1 = broad sheen) |
| `highlightColor` | `string` | `#ffffff` | Color of the specular highlight |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `mirror` | How to handle edges when distortion samples beyond the canvas |

#### React JSX Usage
```tsx
<Shader>
  <FlutedGlass>
    <Circle />
  </FlutedGlass>
</Shader>
```

---

### 📦 `<Form3D>`
*Wraps child content onto a 3D raymarched shape with lighting.*

[Official Documentation](https://shaders.com/docs/components/form3d)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `shape3d` | `string` | `{"type":"ribbon","angle":0,"twist":50,"width":40,"thickness":20,"seed":0}` | 3D shape and its parameters |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the shape on screen |
| `zoom` | `number` | `50` | Camera zoom level |
| `glossiness` | `number` | `50` | Specular highlight intensity and sharpness |
| `lighting` | `number` | `50` | Overall intensity of lighting effects |
| `uvMode` | `"stretch" \| "mirror" \| "wrap"` | `stretch` | How to handle UV coordinates at shape boundaries |
| `speed` | `number` | `1` | Animation speed — scales all spin rates |

#### React JSX Usage
```tsx
<Shader>
  <Form3D>
    <Circle />
  </Form3D>
</Shader>
```

---

### 📦 `<GlassTiles>`
*Refraction-like distortion in a tile grid pattern*

[Official Documentation](https://shaders.com/docs/components/glasstiles)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `2` | The intensity of the glass tiles effect |
| `tileCount` | `number` | `20` | Number of tiles across the longest dimension |
| `rotation` | `number` | `0` | Rotation angle of the tile grid in degrees |
| `roundness` | `number` | `0` | Makes tiles more circular instead of square |

#### React JSX Usage
```tsx
<Shader>
  <GlassTiles
    intensity={2}
  >
    <Circle />
  </GlassTiles>
</Shader>
```

---

### 📦 `<Kaleidoscope>`
*Create a kaleidoscope effect with radial mirrored segments*

[Official Documentation](https://shaders.com/docs/components/kaleidoscope)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | The center point of the kaleidoscope effect |
| `segments` | `number` | `6` | Number of radial segments in the kaleidoscope |
| `angle` | `number` | `0` | Rotation offset for the entire kaleidoscope pattern |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `mirror` | How to handle edges when distortion pushes content out of bounds |

#### React JSX Usage
```tsx
<Shader>
  <Kaleidoscope>
    <Circle />
  </Kaleidoscope>
</Shader>
```

---

### 📦 `<Mirror>`
*Mirror content across a line defined by center point and angle*

[Official Documentation](https://shaders.com/docs/components/mirror)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | The point the mirror line passes through |
| `angle` | `number` | `0` | The angle of the mirror line in degrees |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `mirror` | How to handle edges when distortion pushes content out of bounds |

#### React JSX Usage
```tsx
<Shader>
  <Mirror>
    <Circle />
  </Mirror>
</Shader>
```

---

### 📦 `<Perspective>`
*Rotate the plane in 3D space with pan and tilt*

[Official Documentation](https://shaders.com/docs/components/perspective)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center point of rotation |
| `pan` | `number` | `0` | Horizontal rotation (left/right) |
| `tilt` | `number` | `0` | Vertical rotation (up/down) |
| `fov` | `number` | `60` | Field of view - controls perspective intensity |
| `zoom` | `number` | `1` | Zoom in to fill the frame after rotation |
| `offset` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Shift the result in X/Y |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `transparent` | How to handle edges |

#### React JSX Usage
```tsx
<Shader>
  <Perspective>
    <Circle />
  </Perspective>
</Shader>
```

---

### 📦 `<PolarCoordinates>`
*Convert rectangular coordinates to polar space*

[Official Documentation](https://shaders.com/docs/components/polarcoordinates)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | The center point for polar coordinate conversion |
| `wrap` | `number` | `1` | Controls how much of the angular range to use (1 = full 360°, 0.5 = 180°) |
| `radius` | `number` | `1` | Controls how much of the radius range to use (affects the radial mapping) |
| `intensity` | `number` | `1` | Blends between original UVs (0) and polar coordinates (1) |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `transparent` | How to handle edges when distortion pushes content out of bounds |

#### React JSX Usage
```tsx
<Shader>
  <PolarCoordinates
    radius={1}
    intensity={1}
  >
    <Circle />
  </PolarCoordinates>
</Shader>
```

---

### 📦 `<RectangularCoordinates>`
*Convert polar coordinates back to rectangular space*

[Official Documentation](https://shaders.com/docs/components/rectangularcoordinates)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | The center point for rectangular coordinate conversion |
| `scale` | `number` | `1` | Scale factor for the rectangular output |
| `intensity` | `number` | `1` | Blends between original UVs (0) and rectangular coordinates (1) |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `transparent` | How to handle edges when distortion pushes content out of bounds |

#### React JSX Usage
```tsx
<Shader>
  <RectangularCoordinates
    intensity={1}
  >
    <Circle />
  </RectangularCoordinates>
</Shader>
```

---

### 📦 `<Spherize>`
*Map content onto a 3D sphere surface with depth distortion*

[Official Documentation](https://shaders.com/docs/components/spherize)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `radius` | `number` | `1` | Radius of the sphere (1 = half viewport height) |
| `depth` | `number` | `1` | How much the sphere bulges toward viewer (0 = flat, higher = more bulge) |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | The center point of the sphere |
| `lightPosition` | `{x: number, y: number}` | `{"x":0.3,"y":0.3}` | Position of the specular light source |
| `lightIntensity` | `number` | `0.5` | Intensity of the rim light (0 = off) |
| `lightSoftness` | `number` | `0.5` | Softness of the rim light falloff (0 = hard edge, 1 = soft glow) |
| `lightColor` | `string` | `#ffffff` | Color of the specular highlight |

#### React JSX Usage
```tsx
<Shader>
  <Spherize
    radius={1}
  >
    <Circle />
  </Spherize>
</Shader>
```

---

### 📦 `<Stretch>`
*Stretch content towards a direction from a center point*

[Official Documentation](https://shaders.com/docs/components/stretch)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | The center point of the stretch effect |
| `strength` | `number` | `1` | The intensity of the stretch effect |
| `angle` | `number` | `0` | The direction of the stretch in degrees |
| `falloff` | `number` | `0` | Controls the sharpness of the transition (0 = sharp edge, 1 = gradual transition) |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `stretch` | How to handle edges when distortion pushes content out of bounds |

#### React JSX Usage
```tsx
<Shader>
  <Stretch>
    <Circle />
  </Stretch>
</Shader>
```

---

### 📦 `<Twirl>`
*Rotate and twist content around a center point*

[Official Documentation](https://shaders.com/docs/components/twirl)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | The center point of the twirl effect |
| `intensity` | `number` | `1` | The strength of the twirl effect |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `stretch` | How to handle edges when distortion pushes content out of bounds |

#### React JSX Usage
```tsx
<Shader>
  <Twirl
    intensity={1}
  >
    <Circle />
  </Twirl>
</Shader>
```

---

### 📦 `<WaveDistortion>`
*Wave-based distortion with multiple waveform types*

[Official Documentation](https://shaders.com/docs/components/wavedistortion)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `strength` | `number` | `0.3` | Distortion intensity |
| `frequency` | `number` | `1` | Number of bends/waves |
| `speed` | `number` | `1` | Animation speed |
| `angle` | `number` | `0` | Direction of wave distortion in degrees |
| `waveType` | `"sine" \| "triangle" \| "square" \| "sawtooth" \| "bounce"` | `sine` | Shape of the distortion wave |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `stretch` | How to handle edges when distortion pushes content out of bounds |

#### React JSX Usage
```tsx
<Shader>
  <WaveDistortion>
    <Circle />
  </WaveDistortion>
</Shader>
```

---

## 🏁 Interactive

Shaders that respond directly to user mouse, touch, or drag inputs.

### 📦 `<ChromaFlow>`
*Interactive liquid flow effect that follows your cursor*

[Official Documentation](https://shaders.com/docs/components/chromaflow)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `baseColor` | `string` | `#0066ff` | Base liquid color |
| `upColor` | `string` | `#00ff00` | Color for upward movement |
| `downColor` | `string` | `#ff0000` | Color for downward movement |
| `leftColor` | `string` | `#0000ff` | Color for leftward movement |
| `rightColor` | `string` | `#ffff00` | Color for rightward movement |
| `intensity` | `number` | `1` | Strength of the liquid effect |
| `radius` | `number` | `3` | Radius of the liquid effect |
| `momentum` | `number` | `30` | How much momentum colors retain in their flow direction |

#### React JSX Usage
```tsx
<Shader>
  <ChromaFlow
    intensity={1}
    radius={3}
  />
</Shader>
```

---

### 📦 `<CursorRipples>`
*Fluid-like ripple distortion*

[Official Documentation](https://shaders.com/docs/components/cursorripples)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `10` | Strength of the ripple distortion |
| `decay` | `number` | `10` | How quickly ripples fade (higher = faster) |
| `radius` | `number` | `0.5` | Radius of cursor influence |
| `chromaticSplit` | `number` | `1` | RGB channel separation along ripple edges |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `stretch` | How to handle edges when distortion pushes content out of bounds |

#### React JSX Usage
```tsx
<Shader>
  <CursorRipples
    intensity={10}
    radius={0.5}
  >
    <Circle />
  </CursorRipples>
</Shader>
```

---

### 📦 `<CursorTrail>`
*Animated trail effect that tracks cursor movement*

[Official Documentation](https://shaders.com/docs/components/cursortrail)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#00aaff` | Color of fresh trails |
| `colorB` | `string` | `#ff00aa` | Color trails transition to as they fade |
| `radius` | `number` | `0.5` | Base radius of trail circles |
| `length` | `number` | `0.5` | How long trail circles persist (in seconds) |
| `shrink` | `number` | `1` | How much circles shrink as they fade out (0 = no shrink, 1 = full shrink) |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <CursorTrail
    radius={0.5}
  />
</Shader>
```

---

### 📦 `<Fog>`
*Fog that fills the screen and interacts with the mouse*

[Official Documentation](https://shaders.com/docs/components/fog)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#e0e0e0` | Primary fog color |
| `colorB` | `string` | `#888888` | Secondary fog color — creates variation across the field |
| `seed` | `number` | `0` | Deterministic starting pattern — different seeds produce different fog configurations |
| `speed` | `number` | `1` | Simulation speed multiplier |
| `turbulence` | `number` | `1` | Ambient motion strength |
| `detail` | `number` | `15` | Fine-scale swirling structure — higher values produce more intricate wisps and vortices |
| `blending` | `number` | `0.3` | How much the two colors blend together — 0 behaves like oil & water (colors stay distinct with sharp boundaries), 1 behaves like food coloring (colors fully mix) |
| `mouseInfluence` | `number` | `0.1` | Strength of cursor influence — move the cursor to push fog |
| `mouseRadius` | `number` | `0.1` | Radius of cursor influence area |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <Fog />
</Shader>
```

---

### 📦 `<GridDistortion>`
*Interactive grid distortion controlled by mouse position*

[Official Documentation](https://shaders.com/docs/components/griddistortion)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `1` | Strength of the distortion effect |
| `decay` | `number` | `3` | Rate of distortion decay (higher = faster) |
| `radius` | `number` | `1` | Radius of the distortion effect |
| `gridSize` | `number` | `20` | Resolution of the distortion grid (higher = more detailed) |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `stretch` | How to handle edges when distortion pushes content out of bounds |

#### React JSX Usage
```tsx
<Shader>
  <GridDistortion
    intensity={1}
    radius={1}
  >
    <Circle />
  </GridDistortion>
</Shader>
```

---

### 📦 `<Liquify>`
*Liquid-like interactive deformation effect*

[Official Documentation](https://shaders.com/docs/components/liquify)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `10` | Scale of the fabric displacement effect |
| `stiffness` | `number` | `3` | Fabric rigidity (higher = stiffer canvas, lower = stretchy silk) |
| `damping` | `number` | `3` | How quickly fabric motion settles |
| `radius` | `number` | `1` | Cursor influence area |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `stretch` | How to handle edges when distortion pushes content out of bounds |

#### React JSX Usage
```tsx
<Shader>
  <Liquify
    intensity={10}
    radius={1}
  >
    <Circle />
  </Liquify>
</Shader>
```

---

### 📦 `<Shatter>`
*Broken glass effect with tectonic plate displacement*

[Official Documentation](https://shaders.com/docs/components/shatter)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `crackWidth` | `number` | `1` | Thickness of crack lines |
| `intensity` | `number` | `4` | How much shards shift |
| `radius` | `number` | `0.4` | Cursor influence radius |
| `decay` | `number` | `1` | How fast shards return to rest |
| `seed` | `number` | `2` | Random seed for pattern |
| `chromaticSplit` | `number` | `1` | RGB separation for prismatic glass effect |
| `refractionStrength` | `number` | `5` | How much cracks bend/distort the underlying image |
| `shardLighting` | `number` | `0.1` | Subtle lighting on tilted shards for 3D depth |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `mirror` | How to handle edges when displacement pushes content out of bounds |

#### React JSX Usage
```tsx
<Shader>
  <Shatter
    intensity={4}
    radius={0.4}
  >
    <Circle />
  </Shatter>
</Shader>
```

---

### 📦 `<Smoke>`
*Realistic fluid smoke simulation with vorticity dynamics*

[Official Documentation](https://shaders.com/docs/components/smoke)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#fc83f9` | Color of fresh smoke |
| `colorB` | `string` | `#c21c79` | Color smoke transitions to as it ages |
| `emitFrom` | `{x: number, y: number}` | `{"x":0.5,"y":1}` | The emission source point |
| `direction` | `number` | `0` | Emission direction (0 = up, 90 = right, 180 = down, 270 = left) |
| `speed` | `number` | `20` | Emission velocity strength |
| `spread` | `number` | `60` | Emission cone angle in degrees |
| `emitRadius` | `number` | `0.08` | Size of the emission area |
| `intensity` | `number` | `1` | Smoke emission density |
| `dissipation` | `number` | `0.2` | How fast smoke fades over time |
| `detail` | `number` | `25` | Fine-scale swirling detail |
| `gravity` | `number` | `0.5` | Downward gravitational pull on smoke |
| `colorDecay` | `number` | `0.4` | How quickly smoke shifts from Color A to Color B |
| `mouseInfluence` | `number` | `0.1` | Strength of cursor influence |
| `mouseRadius` | `number` | `0.1` | Radius of cursor influence area |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <Smoke
    intensity={1}
  />
</Shader>
```

---

## 🏁 Shape Effects

GPU-driven styling applied specifically to shape elements.

### 📦 `<Crystal>`
*Diamond-like crystal lens with faceted refraction.*

[Official Documentation](https://shaders.com/docs/components/crystal)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the crystal shape |
| `scale` | `number` | `1` | Scale of the crystal shape (1 = default size) |
| `cutout` | `boolean` | `false` | Cut out alpha outside the crystal shape |
| `refraction` | `number` | `0.5` | How strongly the crystal refracts content beneath |
| `dispersion` | `number` | `0.5` | Prismatic rainbow dispersion — splits light into spectral colors |
| `facets` | `number` | `5` | Symmetry order — how many times the facet pattern repeats around the center |
| `fresnel` | `number` | `0.05` | Fresnel rim glow intensity around the crystal boundary |
| `fresnelSoftness` | `number` | `1` | Fresnel rim width — higher values spread the glow further inward |
| `fresnelColor` | `string` | `#ffffff` | Color of the fresnel rim glow |
| `edgeSoftness` | `number` | `0` | Softness of the crystal boundary edge |
| `innerZoom` | `number` | `1.5` | Magnification of content seen through the crystal |
| `lightAngle` | `number` | `270` | Light direction angle in degrees |
| `highlights` | `number` | `0.5` | Additive brightness on light-facing facets — never darkens |
| `shadows` | `number` | `0.3` | Darkening on shadow-facing facets — never brightens |
| `brightness` | `number` | `1.2` | Overall crystal brightness — higher values push facets toward brilliant white |
| `tintColor` | `string` | `#e8e0ff` | Crystal body tint color |
| `tintIntensity` | `number` | `0` | How much tint color is applied to the crystal interior |
| `tintPreserveLuminosity` | `boolean` | `true` | Preserve original brightness when tinting |
| `shape` | `ShapeConfig` | `polygonSDF` | Shape to render — choose from 11 built-in analytical shapes or supply a custom SDF. See the Shape Effects guide for all available shapes and their options. |
| `shapeSdfUrl` | `string` | `""` | URL to a pre-generated SDF `.bin` file — when non-empty, activates SVG mode and triggers a shader recompile. See the Shape Effects guide for how to generate an SDF from an SVG. |

#### React JSX Usage
```tsx
<Shader>
  <Crystal>
    <Circle />
  </Crystal>
</Shader>
```

---

### 📦 `<Emboss>`
*Embossed / debossed relief shading on top of child content, driven by a custom shape*

[Official Documentation](https://shaders.com/docs/components/emboss)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the embossed shape |
| `scale` | `number` | `1` | Scale of the embossed shape (1 = default size) |
| `depth` | `number` | `-0.5` | Relief depth — negative = inset (debossed), positive = raised (embossed) |
| `lightAngle` | `number` | `260` | Directional light angle in degrees — controls highlight and shadow direction |
| `lightIntensity` | `number` | `0.6` | Strength of the directional edge highlights and shadows |
| `shadowIntensity` | `number` | `0.3` | Darkness of the relief shadow |
| `shape` | `ShapeConfig` | `circleSDF` | Shape to render — choose from 11 built-in analytical shapes or supply a custom SDF. See the Shape Effects guide for all available shapes and their options. |
| `shapeSdfUrl` | `string` | `""` | URL to a pre-generated SDF `.bin` file — when non-empty, activates SVG mode and triggers a shader recompile. See the Shape Effects guide for how to generate an SDF from an SVG. |

#### React JSX Usage
```tsx
<Shader>
  <Emboss>
    <Circle />
  </Emboss>
</Shader>
```

---

### 📦 `<Glass>`
*Optically realistic glass lens driven in a custom shape*

[Official Documentation](https://shaders.com/docs/components/glass)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the glass shape |
| `scale` | `number` | `1` | Scale of the glass shape (1 = default size) |
| `cutout` | `boolean` | `false` | Cut out the alpha outside the glass shape |
| `refraction` | `number` | `1` | Lens refraction — how aggressively the edges warp content beneath (0 = none, 1 = max) |
| `edgeSoftness` | `number` | `0.1` | Edge softness — higher values give a wider, softer fade at the glass boundary |
| `blur` | `number` | `0` | Frosted blur amount — 0 = clear glass, higher = frosted/diffuse |
| `thickness` | `number` | `0.2` | Glass depth — how far inward from the edge the refraction extends |
| `aberration` | `number` | `0.5` | Chromatic aberration — splits RGB channels along the refraction vector |
| `innerZoom` | `number` | `1` | Inner zoom level — magnifies content seen through the glass |
| `lightAngle` | `number` | `300` | Light angle in degrees |
| `highlight` | `number` | `0.05` | Directional edge highlight — bright rim on the light-facing boundary |
| `highlightColor` | `string` | `#ffffff` | Color of the directional edge highlight and specular glint |
| `highlightSoftness` | `number` | `0.5` | Specular highlight softness |
| `fresnel` | `number` | `0.1` | Fresnel rim glow — a soft luminous halo around the glass boundary |
| `fresnelSoftness` | `number` | `0.1` | Fresnel rim width — higher values spread the glow further inward |
| `fresnelColor` | `string` | `#ffffff` | Color of the fresnel rim glow |
| `tintColor` | `string` | `#ffffff` | Color tint applied to the internal directional gradient |
| `tintIntensity` | `number` | `0` | Intensity of the color tint applied to the glass interior |
| `tintPreserveLuminosity` | `boolean` | `true` | Preserve original brightness when tinting |
| `shape` | `ShapeConfig` | `circleSDF` | Shape to render — choose from 11 built-in analytical shapes or supply a custom SDF. See the Shape Effects guide for all available shapes and their options. |
| `shapeSdfUrl` | `string` | `""` | URL to a pre-generated SDF `.bin` file — when non-empty, activates SVG mode and triggers a shader recompile. See the Shape Effects guide for how to generate an SDF from an SVG. |

#### React JSX Usage
```tsx
<Shader>
  <Glass>
    <Circle />
  </Glass>
</Shader>
```

---

### 📦 `<Neon>`
*Photorealistic neon tube / 3D pipe effect driven by a custom shape*

[Official Documentation](https://shaders.com/docs/components/neon)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the neon shape |
| `scale` | `number` | `1` | Scale of the neon shape (1 = default size) |
| `color` | `string` | `#00ddff` | Primary neon tube color |
| `secondaryColor` | `string` | `#ff00aa` | Shadow-side color for a two-tone / dual-lit pipe look |
| `secondaryBlend` | `number` | `0.5` | Blend between mono (0) and two-tone (1) tube coloring |
| `glowColor` | `string` | `#00ddff` | Color of the outer glow / bloom |
| `tubeThickness` | `number` | `0.2` | How far inward from the boundary the tube extends. Low = thin neon outline, high = thick 3D pipe |
| `intensity` | `number` | `1.5` | Overall brightness multiplier |
| `hotCoreIntensity` | `number` | `0.6` | Bright white-hot center line — the gas discharge glow inside the tube |
| `glowIntensity` | `number` | `0.6` | Outer glow / bloom strength |
| `glowRadius` | `number` | `0.25` | How far the glow extends beyond the tube |
| `lightAngle` | `number` | `300` | Directional light angle in degrees — controls 3D shading on the tube |
| `specularIntensity` | `number` | `0.5` | Specular highlight brightness on the tube surface |
| `specularSize` | `number` | `0.5` | Specular highlight size — 0 = tight pinpoint, 1 = broad sheen |
| `cornerSmoothing` | `number` | `0.15` | Rounds sharp corners to mimic how real glass tubes curve at bends |
| `flickerSpeed` | `number` | `0` | Flicker animation speed — 0 = off, higher = faster sporadic on/off |
| `flickerAmount` | `number` | `0.2` | How often the neon flickers off — 0 = always on, 1 = frequent outages |
| `flowSpeed` | `number` | `0` | Flow animation speed — 0 = off, light rotates through the tube |
| `flowAmount` | `number` | `0.3` | Strength of the flowing brightness variation — 0 = uniform, 1 = dramatic |
| `shape` | `ShapeConfig` | `circleSDF` | Shape to render — choose from 11 built-in analytical shapes or supply a custom SDF. See the Shape Effects guide for all available shapes and their options. |
| `shapeSdfUrl` | `string` | `""` | URL to a pre-generated SDF `.bin` file — when non-empty, activates SVG mode and triggers a shader recompile. See the Shape Effects guide for how to generate an SDF from an SVG. |

#### React JSX Usage
```tsx
<Shader>
  <Neon
    color="#00ddff"
    intensity={1.5}
  />
</Shader>
```

---

### 📦 `<SmokeFill>`
*Fill a shape with swirling fluid smoke that interacts with the shape boundary*

[Official Documentation](https://shaders.com/docs/components/smokefill)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#8cf3ff` | Color of fresh smoke |
| `colorB` | `string` | `#04a0d6` | Color smoke transitions to as it ages |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the shape |
| `scale` | `number` | `1` | Scale of the shape (1 = default size) |
| `emitFrom` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Emission source point within the shape |
| `direction` | `number` | `0` | Emission direction (0 = up, 90 = right, 180 = down, 270 = left) |
| `speed` | `number` | `10` | Emission velocity strength |
| `spread` | `number` | `60` | Emission cone angle in degrees |
| `emitRadius` | `number` | `0.03` | Size of the emission area |
| `intensity` | `number` | `1` | Smoke emission density |
| `dissipation` | `number` | `0.3` | How fast smoke fades over time |
| `detail` | `number` | `25` | Fine-scale swirling detail |
| `gravity` | `number` | `0.5` | Downward gravitational pull on smoke — 0 = weightless, negative values = smoke rises |
| `colorDecay` | `number` | `0.4` | How quickly smoke shifts from Color A to Color B |
| `mouseInfluence` | `number` | `0.1` | Strength of cursor influence |
| `mouseRadius` | `number` | `0.1` | Radius of cursor influence area |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |
| `shape` | `ShapeConfig` | `circleSDF` | Shape to render — choose from 11 built-in analytical shapes or supply a custom SDF. See the Shape Effects guide for all available shapes and their options. |
| `shapeSdfUrl` | `string` | `""` | URL to a pre-generated SDF `.bin` file — when non-empty, activates SVG mode and triggers a shader recompile. See the Shape Effects guide for how to generate an SDF from an SVG. |

#### React JSX Usage
```tsx
<Shader>
  <SmokeFill
    intensity={1}
  />
</Shader>
```

---

## 🏁 Shapes

Procedural vector shapes constructed directly on the GPU.

### 📦 `<Circle>`
*Generate a circle with adjustable size and softness*

[Official Documentation](https://shaders.com/docs/components/circle)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ffffff` | The color of the circle |
| `radius` | `number` | `1` | The radius of the circle. A value of one (1) is sets the circle to fit the canvas. |
| `softness` | `number` | `0` | Edge softness. Lower values like zero (0) are sharp, higher values like one (1) are softer. |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | The center point of the circle |
| `strokeThickness` | `number` | `0` | The thickness of the stroke outline. Zero (0) means no stroke. |
| `strokeColor` | `string` | `#000000` | The color of the stroke outline |
| `strokePosition` | `"outside" \| "center" \| "inside"` | `center` | Position of the stroke relative to the circle edge |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for blending fill and stroke colors in soft edges |

#### React JSX Usage
```tsx
<Shader>
  <Circle
    color="#ffffff"
    radius={1}
  />
</Shader>
```

---

### 📦 `<Crescent>`
*Crescent moon shape — an outer circle with an inner circle subtracted*

[Official Documentation](https://shaders.com/docs/components/crescent)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ffffff` | Fill color of the crescent |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the crescent |
| `radius` | `number` | `0.3` | Outer circle radius |
| `innerRatio` | `number` | `0.8` | Inner (bite) circle radius as a fraction of outer radius |
| `offset` | `number` | `0.2` | Horizontal distance the bite circle is shifted from center |
| `rotation` | `number` | `0` | Rotation in degrees |
| `softness` | `number` | `0` | Edge softness for antialiasing |
| `strokeThickness` | `number` | `0` | Stroke thickness. Zero means no stroke. |
| `strokeColor` | `string` | `#000000` | Color of the stroke outline |
| `strokePosition` | `"outside" \| "center" \| "inside"` | `center` | Position of the stroke relative to the shape edge |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for blending fill and stroke colors |

#### React JSX Usage
```tsx
<Shader>
  <Crescent
    color="#ffffff"
    radius={0.3}
  />
</Shader>
```

---

### 📦 `<Cross>`
*Plus / cross shape with adjustable arm length, width, and rounding*

[Official Documentation](https://shaders.com/docs/components/cross)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ffffff` | Fill color of the cross |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the cross |
| `radius` | `number` | `0.35` | Arm half-length — distance from center to the end of each arm |
| `thickness` | `number` | `0.08` | Arm half-width — controls how wide each arm is |
| `rounding` | `number` | `0` | Corner rounding — rounds the arm ends and concave corners |
| `rotation` | `number` | `0` | Rotation in degrees (45° turns a plus into an ×) |
| `softness` | `number` | `0` | Edge softness for antialiasing |
| `strokeThickness` | `number` | `0` | Stroke thickness. Zero means no stroke. |
| `strokeColor` | `string` | `#000000` | Color of the stroke outline |
| `strokePosition` | `"outside" \| "center" \| "inside"` | `center` | Position of the stroke relative to the shape edge |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for blending fill and stroke colors |

#### React JSX Usage
```tsx
<Shader>
  <Cross
    color="#ffffff"
    radius={0.35}
  />
</Shader>
```

---

### 📦 `<Ellipse>`
*Ellipse with independently adjustable horizontal and vertical radii*

[Official Documentation](https://shaders.com/docs/components/ellipse)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ffffff` | Fill color of the ellipse |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the ellipse |
| `radiusX` | `number` | `0.35` | Horizontal semi-axis radius |
| `radiusY` | `number` | `0.2` | Vertical semi-axis radius |
| `rotation` | `number` | `0` | Rotation in degrees |
| `softness` | `number` | `0` | Edge softness for antialiasing |
| `strokeThickness` | `number` | `0` | Stroke thickness. Zero means no stroke. |
| `strokeColor` | `string` | `#000000` | Color of the stroke outline |
| `strokePosition` | `"outside" \| "center" \| "inside"` | `center` | Position of the stroke relative to the shape edge |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for blending fill and stroke colors |

#### React JSX Usage
```tsx
<Shader>
  <Ellipse
    color="#ffffff"
  />
</Shader>
```

---

### 📦 `<Flower>`
*Petal shape with N lobes and adjustable inner-to-outer radius ratio*

[Official Documentation](https://shaders.com/docs/components/flower)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ffffff` | Fill color of the flower |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the flower |
| `radius` | `number` | `0.4` | Outer petal tip radius in UV space |
| `sides` | `number` | `5` | Number of petals |
| `innerRatio` | `number` | `0.4` | Inner valley radius as a ratio of outer radius — lower values make deeper notches |
| `rotation` | `number` | `0` | Rotation in degrees |
| `softness` | `number` | `0` | Edge softness for antialiasing |
| `strokeThickness` | `number` | `0` | Stroke thickness. Zero means no stroke. |
| `strokeColor` | `string` | `#000000` | Color of the stroke outline |
| `strokePosition` | `"outside" \| "center" \| "inside"` | `center` | Position of the stroke relative to the shape edge |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for blending fill and stroke colors |

#### React JSX Usage
```tsx
<Shader>
  <Flower
    color="#ffffff"
    radius={0.4}
  />
</Shader>
```

---

### 📦 `<Polygon>`
*Regular polygon with adjustable sides and corner rounding*

[Official Documentation](https://shaders.com/docs/components/polygon)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ffffff` | Fill color of the polygon |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the polygon |
| `radius` | `number` | `0.4` | Circumradius — distance from center to vertices in UV space |
| `sides` | `number` | `6` | Number of sides (3 = triangle, 4 = square, 6 = hexagon, etc.) |
| `rounding` | `number` | `0` | Corner rounding — 0 is sharp vertices, 1 morphs into a circle |
| `rotation` | `number` | `0` | Rotation in degrees |
| `softness` | `number` | `0` | Edge softness for antialiasing |
| `strokeThickness` | `number` | `0` | Stroke thickness. Zero means no stroke. |
| `strokeColor` | `string` | `#000000` | Color of the stroke outline |
| `strokePosition` | `"outside" \| "center" \| "inside"` | `center` | Position of the stroke relative to the shape edge |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for blending fill and stroke colors |

#### React JSX Usage
```tsx
<Shader>
  <Polygon
    color="#ffffff"
    radius={0.4}
  />
</Shader>
```

---

### 📦 `<Ring>`
*Annular ring (donut) with adjustable radius and band thickness*

[Official Documentation](https://shaders.com/docs/components/ring)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ffffff` | Fill color of the ring |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the ring |
| `radius` | `number` | `0.3` | Distance from center to the ring's midline in UV space |
| `thickness` | `number` | `0.07` | Half-width of the ring band — total ring width is twice this value |
| `softness` | `number` | `0` | Edge softness for antialiasing (applied to both inner and outer ring edges) |
| `strokeThickness` | `number` | `0` | Stroke thickness. Zero means no stroke. |
| `strokeColor` | `string` | `#000000` | Color of the stroke outline |
| `strokePosition` | `"outside" \| "center" \| "inside"` | `center` | Position of the stroke relative to the ring edge |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for blending fill and stroke colors |

#### React JSX Usage
```tsx
<Shader>
  <Ring
    color="#ffffff"
    radius={0.3}
  />
</Shader>
```

---

### 📦 `<RoundedRect>`
*Rounded rectangle with adjustable width, height, and corner rounding*

[Official Documentation](https://shaders.com/docs/components/roundedrect)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ffffff` | Fill color of the rectangle |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the rectangle |
| `width` | `number` | `0.35` | Half-width of the rectangle |
| `height` | `number` | `0.25` | Half-height of the rectangle |
| `rounding` | `number` | `0.05` | Corner rounding radius — set to min(width, height) for a pill shape |
| `rotation` | `number` | `0` | Rotation in degrees |
| `softness` | `number` | `0` | Edge softness for antialiasing |
| `strokeThickness` | `number` | `0` | Stroke thickness. Zero means no stroke. |
| `strokeColor` | `string` | `#000000` | Color of the stroke outline |
| `strokePosition` | `"outside" \| "center" \| "inside"` | `center` | Position of the stroke relative to the shape edge |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for blending fill and stroke colors |

#### React JSX Usage
```tsx
<Shader>
  <RoundedRect
    color="#ffffff"
  />
</Shader>
```

---

### 📦 `<Star>`
*Classic star polygon with straight sides and sharp pointed tips*

[Official Documentation](https://shaders.com/docs/components/star)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ffffff` | Fill color of the star |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the star |
| `radius` | `number` | `0.4` | Outer tip radius — distance from center to the pointed tips |
| `sides` | `number` | `5` | Number of points on the star |
| `innerRatio` | `number` | `0.4` | Inner vertex radius as a ratio of outer radius (0.382 = golden-ratio 5-star) |
| `rotation` | `number` | `0` | Rotation in degrees |
| `softness` | `number` | `0` | Edge softness for antialiasing |
| `strokeThickness` | `number` | `0` | Stroke thickness. Zero means no stroke. |
| `strokeColor` | `string` | `#000000` | Color of the stroke outline |
| `strokePosition` | `"outside" \| "center" \| "inside"` | `center` | Position of the stroke relative to the shape edge |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for blending fill and stroke colors |

#### React JSX Usage
```tsx
<Shader>
  <Star
    color="#ffffff"
    radius={0.4}
  />
</Shader>
```

---

### 📦 `<Trapezoid>`
*Trapezoid with adjustable top and bottom widths and height*

[Official Documentation](https://shaders.com/docs/components/trapezoid)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ffffff` | Fill color of the trapezoid |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the trapezoid |
| `bottomWidth` | `number` | `0.35` | Half-width of the bottom edge |
| `topWidth` | `number` | `0.2` | Half-width of the top edge |
| `height` | `number` | `0.25` | Half-height of the trapezoid |
| `rotation` | `number` | `0` | Rotation in degrees |
| `softness` | `number` | `0` | Edge softness for antialiasing |
| `strokeThickness` | `number` | `0` | Stroke thickness. Zero means no stroke. |
| `strokeColor` | `string` | `#000000` | Color of the stroke outline |
| `strokePosition` | `"outside" \| "center" \| "inside"` | `center` | Position of the stroke relative to the shape edge |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for blending fill and stroke colors |

#### React JSX Usage
```tsx
<Shader>
  <Trapezoid
    color="#ffffff"
  />
</Shader>
```

---

### 📦 `<Vesica>`
*Vesica piscis (lens shape) formed by the intersection of two overlapping circles*

[Official Documentation](https://shaders.com/docs/components/vesica)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ffffff` | Fill color of the vesica |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center position of the vesica |
| `radius` | `number` | `0.35` | Radius of the two overlapping circles |
| `spread` | `number` | `0.5` | Circle separation — 0 = full circle overlap, 1 = infinitely thin lens |
| `rotation` | `number` | `0` | Rotation in degrees |
| `softness` | `number` | `0` | Edge softness for antialiasing |
| `strokeThickness` | `number` | `0` | Stroke thickness. Zero means no stroke. |
| `strokeColor` | `string` | `#000000` | Color of the stroke outline |
| `strokePosition` | `"outside" \| "center" \| "inside"` | `center` | Position of the stroke relative to the shape edge |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for blending fill and stroke colors |

#### React JSX Usage
```tsx
<Shader>
  <Vesica
    color="#ffffff"
    radius={0.35}
  />
</Shader>
```

---

## 🏁 Stylize

Filters that manipulate rendering styles to create distinct artistic or retro aesthetics.

### 📦 `<Ascii>`
*Convert imagery to ASCII character art*

[Official Documentation](https://shaders.com/docs/components/ascii)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `characters` | `string` | `@%#*+=-:.` | Characters ordered from dense to sparse. First character is used for bright areas, last for dark areas. |
| `cellSize` | `number` | `30` | Size of each ASCII character cell (normalized to 1080p reference, scales proportionally at other resolutions) |
| `fontFamily` | `"Azeret Mono" \| "Courier Prime" \| "Cutive Mono" \| "Fira Code" \| "Geist Mono" \| "IBM Plex Mono" \| "JetBrains Mono" \| "Major Mono Display" \| "Martian Mono" \| "Nova Mono" \| "Press Start 2P" \| "Roboto Mono" \| "Share Tech Mono" \| "Silkscreen" \| "Source Code Pro" \| "Space Mono" \| "Syne Mono" \| "VT323" \| "Xanh Mono"` | `JetBrains Mono` | Font family for characters |
| `spacing` | `number` | `1` | Character size within each cell (1.0 = optimal size, 0.0 = smallest) |
| `gamma` | `number` | `1` | Brightness curve adjustment. <1 brightens darks (more light characters), >1 darkens midtones (more dark characters). Use to better fit characters to image brightness range. |
| `alphaThreshold` | `number` | `0` | Pixels with alpha below this threshold become fully transparent. |
| `preserveAlpha` | `boolean` | `true` | When enabled, output alpha matches input alpha. When disabled, pixels above the alpha threshold become fully opaque. |

#### React JSX Usage
```tsx
<Shader>
  <Ascii>
    <Circle />
  </Ascii>
</Shader>
```

---

### 📦 `<CRTScreen>`
*Retro CRT monitor simulation with scanlines*

[Official Documentation](https://shaders.com/docs/components/crtscreen)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `pixelSize` | `number` | `128` | Size of individual TV pixels (lower = more pixels) |
| `colorShift` | `number` | `1` | Chromatic aberration amount |
| `scanlineIntensity` | `number` | `0.3` | Strength of horizontal scanlines |
| `scanlineFrequency` | `number` | `200` | Number of scanlines across screen |
| `brightness` | `number` | `1` | Screen brightness boost |
| `contrast` | `number` | `1` | Screen contrast enhancement |
| `vignetteIntensity` | `number` | `1` | Strength of corner darkening effect (0 = off) |
| `vignetteRadius` | `number` | `0.5` | How far the vignette extends inward (0 = edges only, 1 = reaches center) |

#### React JSX Usage
```tsx
<Shader>
  <CRTScreen>
    <Circle />
  </CRTScreen>
</Shader>
```

---

### 📦 `<ChromaticAberration>`
*Separate RGB channels for a prismatic distortion effect*

[Official Documentation](https://shaders.com/docs/components/chromaticaberration)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `strength` | `number` | `0.2` | Overall strength of the chromatic aberration effect |
| `angle` | `number` | `0` | Direction of the chromatic aberration in degrees |
| `redOffset` | `number` | `-1` | Red channel offset multiplier |
| `greenOffset` | `number` | `0` | Green channel offset multiplier |
| `blueOffset` | `number` | `1` | Blue channel offset multiplier |

#### React JSX Usage
```tsx
<Shader>
  <ChromaticAberration>
    <Circle />
  </ChromaticAberration>
</Shader>
```

---

### 📦 `<ContourLines>`
*Draw topographical contour lines based on luminance or alpha*

[Official Documentation](https://shaders.com/docs/components/contourlines)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `levels` | `number` | `5` | Number of contour levels |
| `lineWidth` | `number` | `2` | Width of the contour lines in pixels |
| `softness` | `number` | `0` | Edge softness of the lines (0 = sharp, 1 = soft) |
| `gamma` | `number` | `0.5` | Contour distribution. <1 clusters in bright, >1 clusters in dark |
| `invert` | `boolean` | `false` | Invert the source values |
| `source` | `"luminance" \| "alpha"` | `luminance` | Use luminance or alpha channel for contours |
| `colorMode` | `"source" \| "custom"` | `source` | Use source image colors or custom colors |
| `lineColor` | `string` | `#000000` | Color of the contour lines (custom mode) |
| `backgroundColor` | `string` | `transparent` | Background color (custom mode) |

#### React JSX Usage
```tsx
<Shader>
  <ContourLines>
    <Circle />
  </ContourLines>
</Shader>
```

---

### 📦 `<Dither>`
*Dithering effect with multiple pattern options*

[Official Documentation](https://shaders.com/docs/components/dither)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `pattern` | `"bayer2" \| "bayer4" \| "bayer8" \| "clusteredDot" \| "blueNoise" \| "whiteNoise"` | `bayer4` | Dithering pattern algorithm |
| `pixelSize` | `number` | `4` | Size of dithering pixels |
| `threshold` | `number` | `0.5` | Luminance threshold for dithering |
| `spread` | `number` | `1` | How much of the luminance range participates in dithering (lower = more solid areas) |
| `colorMode` | `"custom" \| "source"` | `custom` | How colors are determined |
| `colorA` | `string` | `transparent` | Dark color for dithering |
| `colorB` | `string` | `#ffffff` | Light color for dithering |

#### React JSX Usage
```tsx
<Shader>
  <Dither>
    <Circle />
  </Dither>
</Shader>
```

---

### 📦 `<DropShadow>`
*Adds a soft shadow behind the child content based on its alpha silhouette*

[Official Documentation](https://shaders.com/docs/components/dropshadow)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#000000` | Shadow color |
| `distance` | `number` | `0.1` | How far the shadow is offset from the content |
| `angle` | `number` | `135` | Direction the shadow is cast (compass degrees: 0=up, 90=right, 135=lower-right, 180=down) |
| `blur` | `number` | `5` | Shadow softness (blur radius in pixels) |
| `intensity` | `number` | `0.5` | Shadow intensity — how strong/visible the shadow is |
| `cutout` | `boolean` | `false` | Hide the original layer and show only the shadow |

#### React JSX Usage
```tsx
<Shader>
  <DropShadow
    color="#000000"
    intensity={0.5}
  >
    <Circle />
  </DropShadow>
</Shader>
```

---

### 📦 `<FilmGrain>`
*Analog film grain texture overlay, weighted toward darker areas*

[Official Documentation](https://shaders.com/docs/components/filmgrain)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `strength` | `number` | `0.5` | Intensity of the film grain noise |
| `bias` | `number` | `2` | Concentrates grain in darker areas. Higher values focus grain more heavily on shadows; 0 applies grain uniformly. |
| `animated` | `boolean` | `false` | When enabled, the grain pattern changes each frame for a dynamic film effect |

#### React JSX Usage
```tsx
<Shader>
  <FilmGrain>
    <Circle />
  </FilmGrain>
</Shader>
```

---

### 📦 `<Glitch>`
*Digital glitch that melts pixels and distorts colors*

[Official Documentation](https://shaders.com/docs/components/glitch)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `0.5` | Overall glitch strength and frequency of glitch bursts |
| `speed` | `number` | `1` | How fast the glitch pattern evolves |
| `rgbShift` | `number` | `5` | Amount of chromatic aberration (RGB channel splitting) |
| `blockDensity` | `number` | `10` | Base number of horizontal glitch bands |
| `colorBarIntensity` | `number` | `0.2` | Intensity of vivid neon color bar overlay in glitch regions |
| `mirrorAmount` | `number` | `0.3` | Chance of glitch blocks showing mirrored/flipped content |
| `scanlineIntensity` | `number` | `0.2` | Visibility of CRT-style horizontal scanlines in distorted areas |

#### React JSX Usage
```tsx
<Shader>
  <Glitch
    intensity={0.5}
  >
    <Circle />
  </Glitch>
</Shader>
```

---

### 📦 `<Glow>`
*Soft glow effect with adjustable intensity*

[Official Documentation](https://shaders.com/docs/components/glow)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `intensity` | `number` | `1` | Glow intensity (brightness of the glow effect) |
| `threshold` | `number` | `0.5` | Brightness threshold for glow extraction (lower = more glow) |
| `size` | `number` | `25` | Glow spread in pixels (clean up to ~72px, mild banding above) |

#### React JSX Usage
```tsx
<Shader>
  <Glow
    intensity={1}
  >
    <Circle />
  </Glow>
</Shader>
```

---

### 📦 `<Halftone>`
*Halftone dot pattern effect for printing aesthetics*

[Official Documentation](https://shaders.com/docs/components/halftone)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `style` | `"classic" \| "cmyk"` | `classic` | Halftone rendering style |
| `frequency` | `number` | `100` | Frequency of the halftone dots |
| `angle` | `number` | `45` | Rotation angle of the pattern (in degrees) |
| `cyanAngle` | `number` | `15` | Screen angle for the cyan plate (in degrees) |
| `magentaAngle` | `number` | `75` | Screen angle for the magenta plate (in degrees) |
| `yellowAngle` | `number` | `0` | Screen angle for the yellow plate (in degrees) |
| `blackAngle` | `number` | `45` | Screen angle for the black plate (in degrees) |
| `misprint` | `number` | `0` | Simulated mis-registration between plates. Plates are offset around the misprint angle, producing colour fringing at the edges of inked regions. |
| `misprintAngle` | `number` | `0` | Direction the plates drift apart. Rotating this rotates the colour-fringing pattern. |
| `paperColor` | `string` | `#ffffff` | Paper/substrate color shown where no ink lands |
| `cyanColor` | `string` | `#00ffff` | Cyan ink color |
| `magentaColor` | `string` | `#ff00ff` | Magenta ink color |
| `yellowColor` | `string` | `#ffff00` | Yellow ink color |
| `blackColor` | `string` | `#000000` | Black (key) ink color |

#### React JSX Usage
```tsx
<Shader>
  <Halftone>
    <Circle />
  </Halftone>
</Shader>
```

---

### 📦 `<LensFlare>`
*Realistic camera lens flare with artifacts.*

[Official Documentation](https://shaders.com/docs/components/lensflare)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `lightPosition` | `{x: number, y: number}` | `{"x":0.3,"y":0.3}` | Position of the light source |
| `intensity` | `number` | `0.5` | Master brightness of the entire lens flare effect |
| `ghostIntensity` | `number` | `0.4` | Brightness of internal reflection ghost discs along the flare axis |
| `ghostSpread` | `number` | `0.7` | Spacing between ghost reflections along the flare axis |
| `ghostChroma` | `number` | `0.3` | Rainbow chromatic fringing around ghost element edges |
| `haloIntensity` | `number` | `0.4` | Brightness of the circular halo ring from internal reflection |
| `haloRadius` | `number` | `0.6` | Radius of the halo ring |
| `haloChroma` | `number` | `0.6` | Spectral dispersion on the halo creating rainbow color separation |
| `haloSoftness` | `number` | `0.8` | Thickness and softness of the halo ring |
| `starburstIntensity` | `number` | `0.3` | Brightness of diffraction spikes radiating from the light source |
| `starburstPoints` | `number` | `6` | Number of starburst spikes (simulates aperture blade count) |
| `streakIntensity` | `number` | `0.15` | Brightness of horizontal anamorphic light streak |
| `streakLength` | `number` | `0.5` | Horizontal extent of the anamorphic streak |
| `glareIntensity` | `number` | `0.2` | Soft veiling glare that washes out contrast around the light |
| `glareSize` | `number` | `0.5` | Size of the soft glare glow |
| `edgeFade` | `number` | `0.2` | How much the flare fades when the light source is near the screen edge (0 = no fade, 1 = heavy fade) |
| `speed` | `number` | `0.5` | Speed of subtle flare shimmer and starburst rotation |

#### React JSX Usage
```tsx
<Shader>
  <LensFlare
    intensity={0.5}
  />
</Shader>
```

---

### 📦 `<Paper>`
*Applies realistic paper grain and surface roughness to child content*

[Official Documentation](https://shaders.com/docs/components/paper)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `roughness` | `number` | `0.3` | Surface roughness — higher values create more pronounced brightness variation |
| `grainScale` | `number` | `1` | Scale of the paper grain — lower = coarser, higher = finer |
| `displacement` | `number` | `0.15` | Surface micro-roughness — shifts pixels at grain scale like real paper fiber bumps |
| `seed` | `number` | `0` | Random seed for pattern variation |

#### React JSX Usage
```tsx
<Shader>
  <Paper>
    <Circle />
  </Paper>
</Shader>
```

---

### 📦 `<Pixelate>`
*Pixelation effect with adjustable cell size*

[Official Documentation](https://shaders.com/docs/components/pixelate)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `scale` | `number` | `50` | Number of pixels along the longest edge (higher = smaller pixels) |
| `gap` | `number` | `0` | Space between pixels as a fraction of cell size (0 = no gap, 1 = fully invisible) |
| `roundness` | `number` | `0` | Roundness of each pixel's corners (0 = square, 1 = circle) |

#### React JSX Usage
```tsx
<Shader>
  <Pixelate>
    <Circle />
  </Pixelate>
</Shader>
```

---

### 📦 `<ReflectivePlane>`
*Reflective floor that mirrors the content above it*

[Official Documentation](https://shaders.com/docs/components/reflectiveplane)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `height` | `number` | `0.7` | Vertical position of the reflective surface |
| `distance` | `number` | `0.5` | How far below the floor the reflection remains visible before fully fading to transparent. |
| `falloff` | `number` | `0.5` | Width of the fade zone, as a fraction of reflection distance. |
| `blur` | `number` | `3` | Maximum blur applied to the reflection far from the surface. |
| `blurDistance` | `number` | `0.3` | How far below the surface the blur takes to ramp from sharp to maximum. |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `stretch` | How to handle reflected samples that fall outside the source content. |

#### React JSX Usage
```tsx
<Shader>
  <ReflectivePlane>
    <Circle />
  </ReflectivePlane>
</Shader>
```

---

### 📦 `<VHS>`
*Analog VHS tape with intermittent tape damage, chroma bleed, and per-scanline noise*

[Official Documentation](https://shaders.com/docs/components/vhs)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `wobble` | `number` | `1` | Overall amount of tape damage — waves, creases, and head-switching noise. Bursts on and off organically over time. |
| `scanlineNoise` | `number` | `0.6` | Per-scanline fine chroma/luma jitter. Adds the classic horizontal-streak detail. |
| `smear` | `number` | `0.2` | Horizontal chroma smear (color bleed) amount. Positive trails colour to the right (classic VHS), negative trails it to the left. |
| `speed` | `number` | `1` | Animation speed of the tape effects. |

#### React JSX Usage
```tsx
<Shader>
  <VHS>
    <Circle />
  </VHS>
</Shader>
```

---

### 📦 `<Vignette>`
*Darkens or tints the edges of the frame, drawing attention toward the center*

[Official Documentation](https://shaders.com/docs/components/vignette)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#000000` | Color of the vignette at the edges |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center of the clear area where the vignette begins |
| `radius` | `number` | `0.5` | Distance from center where the vignette begins to fade in |
| `falloff` | `number` | `0.5` | Width of the transition zone from clear to full vignette |
| `intensity` | `number` | `1` | Strength of the vignette effect |

#### React JSX Usage
```tsx
<Shader>
  <Vignette
    color="#000000"
    radius={0.5}
    intensity={1}
  >
    <Circle />
  </Vignette>
</Shader>
```

---

## 🏁 Textures

Procedural or media-driven textures that can be mapped onto shapes or used to drive other effects.

### 📦 `<Aurora>`
*Mesmerizing aurora borealis with layered curtains, vertical rays, and flowing light.*

[Official Documentation](https://shaders.com/docs/components/aurora)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#a533f8` | Edge color at the curtain base |
| `colorB` | `string` | `#22ee88` | Core color in the bright center |
| `colorC` | `string` | `#1694e8` | Tip color at the ray ends |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |
| `balance` | `number` | `50` | Shifts color distribution across the curtain height |
| `intensity` | `number` | `80` | Overall aurora brightness |
| `curtainCount` | `number` | `4` | Number of aurora curtain layers |
| `speed` | `number` | `5` | Animation speed |
| `waviness` | `number` | `50` | How much the curtains undulate |
| `rayDensity` | `number` | `20` | Density of vertical ray structures |
| `height` | `number` | `120` | How tall the aurora extends |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0}` | Center position of the aurora |
| `seed` | `number` | `0` | Random seed for variation |

#### React JSX Usage
```tsx
<Shader>
  <Aurora
    intensity={80}
  />
</Shader>
```

---

### 📦 `<Beam>`
*A beam of light from one point to another.*

[Official Documentation](https://shaders.com/docs/components/beam)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `startPosition` | `{x: number, y: number}` | `{"x":0.2,"y":0.5}` | Starting point of the beam |
| `endPosition` | `{x: number, y: number}` | `{"x":0.8,"y":0.5}` | Ending point of the beam |
| `startThickness` | `number` | `0.2` | Thickness at the start of the beam |
| `endThickness` | `number` | `0.2` | Thickness at the end of the beam |
| `startSoftness` | `number` | `0.5` | Edge softness at the start of the beam |
| `endSoftness` | `number` | `0.5` | Edge softness at the end of the beam |
| `insideColor` | `string` | `#FF0000` | Color at the center of the beam |
| `outsideColor` | `string` | `#0000FF` | Color at the edges of the beam |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <Beam />
</Shader>
```

---

### 📦 `<Blob>`
*Organic animated blob with 3D lighting and gradients*

[Official Documentation](https://shaders.com/docs/components/blob)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#ff6b35` | Primary color of the blob |
| `colorB` | `string` | `#e91e63` | Secondary color of the blob |
| `size` | `number` | `0.5` | Size of the blob |
| `deformation` | `number` | `0.5` | How organic and blobby the shape is (0 = circle, 1 = very blobby) |
| `softness` | `number` | `0.5` | Softness of the blob edges (combines edge width and transition curve) |
| `highlightIntensity` | `number` | `0.5` | Intensity of specular highlight effect |
| `highlightX` | `number` | `0.3` | Light direction X component |
| `highlightY` | `number` | `-0.3` | Light direction Y component |
| `highlightZ` | `number` | `0.4` | Light direction Z component |
| `highlightColor` | `string` | `#ffe11a` | Color of the specular highlight |
| `speed` | `number` | `0.5` | Animation speed |
| `seed` | `number` | `1` | Adjusts the starting state, useful for variation |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | The center point of the blob |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <Blob />
</Shader>
```

---

### 📦 `<BrickPattern>`
*Classic brick wall pattern with alternating rows and mortar gaps*

[Official Documentation](https://shaders.com/docs/components/brickpattern)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorBrick` | `string` | `#000000` | Brick color |
| `colorMortar` | `string` | `#ffffff` | Mortar / gap color |
| `cellsX` | `number` | `8` | Number of bricks per row |
| `cellsY` | `number` | `10` | Number of brick rows |
| `mortar` | `number` | `0.05` | Width of mortar gaps — equal pixel thickness in both directions |
| `angle` | `number` | `0` | Rotation angle in degrees |
| `speed` | `number` | `0` | Animation speed |
| `offset` | `number` | `0` | Static horizontal offset — shifts the brick pattern without animating |
| `speedVariance` | `number` | `0` | How much each row's speed varies — at high values rows move at different speeds and directions |
| `seed` | `number` | `0` | Random seed for per-row speed variation |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for interpolation |

#### React JSX Usage
```tsx
<Shader>
  <BrickPattern />
</Shader>
```

---

### 📦 `<Checkerboard>`
*Classic checkerboard pattern with two alternating colors*

[Official Documentation](https://shaders.com/docs/components/checkerboard)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#cccccc` | First color of the checkerboard pattern |
| `colorB` | `string` | `#999999` | Second color of the checkerboard pattern |
| `cells` | `number` | `8` | Number of cells along the shortest canvas edge (creates square cells) |
| `softness` | `number` | `0` | Smoothness of the transition between colors (0 = hard edges, 1 = very soft) |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <Checkerboard />
</Shader>
```

---

### 📦 `<Chevron>`
*Animated chevron / zigzag stripe pattern*

[Official Documentation](https://shaders.com/docs/components/chevron)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#000000` | First color |
| `colorB` | `string` | `#ffffff` | Second color |
| `count` | `number` | `5` | Number of chevron pairs visible |
| `angle` | `number` | `0` | Rotation angle of the chevrons |
| `balance` | `number` | `0.5` | Ratio of the two colors |
| `softness` | `number` | `0` | Edge softness |
| `speed` | `number` | `0` | Animation speed |
| `offset` | `number` | `0` | Phase offset for pattern positioning |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for interpolation |

#### React JSX Usage
```tsx
<Shader>
  <Chevron />
</Shader>
```

---

### 📦 `<ColorWheel>`
*A directional gradient that smoothly cycles through rainbow colors or a custom set of three colors*

[Official Documentation](https://shaders.com/docs/components/colorwheel)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `mode` | `"rainbow" \| "custom"` | `rainbow` | Rainbow cycles through the full spectrum; Custom loops through your three chosen colors |
| `colorA` | `string` | `#ff0000` | First color in the cycle |
| `colorB` | `string` | `#00ff88` | Second color in the cycle |
| `colorC` | `string` | `#0066ff` | Third color in the cycle |
| `scale` | `number` | `1` | Number of color cycles across the viewport |
| `angle` | `number` | `0` | Direction the gradient flows |
| `speed` | `number` | `0.05` | Speed at which the gradient cycles |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `oklch` | Color space for blending between custom colors |

#### React JSX Usage
```tsx
<Shader>
  <ColorWheel />
</Shader>
```

---

### 📦 `<ConicGradient>`
*Colors sweep in a full circle around a center point, like a color wheel*

[Official Documentation](https://shaders.com/docs/components/conicgradient)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#FF0080` | Starting color of the sweep |
| `colorB` | `string` | `#00BFFF` | Ending color of the sweep (wraps back to Color A) |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center point of the sweep |
| `rotation` | `number` | `0` | Rotation offset in degrees — shifts where Color A begins |
| `repeat` | `number` | `1` | Number of times the gradient repeats around the circle. Values above 1 create a starburst pattern. |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `oklch` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <ConicGradient />
</Shader>
```

---

### 📦 `<DOMTexture>`
*Render live HTML/DOM content as a WebGPU texture layer via the html-in-canvas API. Requires Chrome Canary with chrome://flags/#canvas-draw-element enabled.*

[Official Documentation](https://shaders.com/docs/components/domtexture)

#### React JSX Usage
```tsx
<Shader>
  <DOMTexture />
</Shader>
```

---

### 📦 `<DiamondGradient>`
*Diamond-shaped gradient radiating from a center point using Manhattan distance*

[Official Documentation](https://shaders.com/docs/components/diamondgradient)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#4ffb4a` | Color at the center of the diamond |
| `colorB` | `string` | `#4f1238` | Color at the outer edges of the diamond |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center point of the diamond |
| `size` | `number` | `0.7` | Extent of the gradient — controls how far Color A reaches before transitioning to Color B |
| `rotation` | `number` | `0` | Rotation in degrees — tilts the diamond into a rhombus |
| `repeat` | `number` | `1` | Number of times the gradient repeats outward. Values above 1 create concentric diamond or square bands. |
| `roundness` | `number` | `0` | Morphs from a sharp diamond (0) to a square (1) |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `oklch` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <DiamondGradient />
</Shader>
```

---

### 📦 `<DotGrid>`
*Grid of dots with optional twinkling animation*

[Official Documentation](https://shaders.com/docs/components/dotgrid)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ffffff` | The color of the dot |
| `density` | `number` | `30` | The number of dots on the longest canvas edge |
| `dotSize` | `number` | `0.3` | The size of each dot, zero (0) being invisible, one (1) filled the grid with no gaps |
| `twinkle` | `number` | `0` | Intensity of the twinkle effect (0 = off, 1 = full twinkle) |

#### React JSX Usage
```tsx
<Shader>
  <DotGrid
    color="#ffffff"
  />
</Shader>
```

---

### 📦 `<FallingLines>`
*Directional falling lines with a leading-to-trailing color fade*

[Official Documentation](https://shaders.com/docs/components/fallinglines)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#ffffff` | Color at the leading edge of each line |
| `colorB` | `string` | `#ffffff00` | Color at the trailing edge (transparent by default) |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for interpolation between lead and trail colors |
| `angle` | `number` | `90` | Direction of movement in degrees (90=down, 270=up, 0=right, 180=left) |
| `speed` | `number` | `0.5` | Movement speed |
| `speedVariance` | `number` | `0.3` | Per-line speed variance (0=uniform, 1=high variance) |
| `density` | `number` | `15` | Number of line columns across the canvas |
| `trailLength` | `number` | `0.35` | Streak length relative to spacing (0=point, 1=continuous) |
| `balance` | `number` | `0.5` | Color mix midpoint (0.5=linear, 0=all trailing/colorB, 1=all leading/colorA) |
| `strokeWidth` | `number` | `0.15` | Line thickness as fraction of column width (0=hairline, 1=full width) |
| `rounding` | `number` | `1` | Rounds the leading edge (0=flat/square, 1=fully rounded cap) |

#### React JSX Usage
```tsx
<Shader>
  <FallingLines />
</Shader>
```

---

### 📦 `<FloatingParticles>`
*Animated floating particles with twinkle effects*

[Official Documentation](https://shaders.com/docs/components/floatingparticles)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `randomness` | `number` | `0.25` | Randomness of particle animation |
| `speed` | `number` | `0.25` | Speed of particle movement |
| `angle` | `number` | `90` | Movement angle in degrees (0=right, 90=down, 180=left, 270=up) |
| `particleSize` | `number` | `2` | Size of particles |
| `particleSoftness` | `number` | `0` | Edge softness of particles (0 = sharp, 1 = very soft) |
| `twinkle` | `number` | `0.5` | Intensity of the twinkle effect (0 = off, 1 = full twinkle) |
| `count` | `number` | `5` | Number of particle layers |
| `particleColor` | `string` | `#ffffff` | Color of the particles |
| `speedVariance` | `number` | `0.3` | Per-layer speed variance (0 = all layers same speed, 1 = high variance) |
| `angleVariance` | `number` | `30` | Per-layer angle variance in degrees (0 = all layers same angle, 180 = full variance) |
| `particleDensity` | `number` | `3` | Particle density (lower = more spread out, higher = more dense) |

#### React JSX Usage
```tsx
<Shader>
  <FloatingParticles />
</Shader>
```

---

### 📦 `<FlowingGradient>`
*Liquid silk gradient with organic flowing color bands*

[Official Documentation](https://shaders.com/docs/components/flowinggradient)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#0a0015` | Deep background color |
| `colorB` | `string` | `#6b17e6` | Primary accent color |
| `colorC` | `string` | `#ff4d6a` | Secondary accent color |
| `colorD` | `string` | `#ff6b35` | Tertiary accent color |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `oklch` | Color space for color interpolation |
| `speed` | `number` | `1` | Animation speed |
| `distortion` | `number` | `0.5` | Organic distortion intensity |
| `seed` | `number` | `0` | Random seed for variation |

#### React JSX Usage
```tsx
<Shader>
  <FlowingGradient />
</Shader>
```

---

### 📦 `<FractalNoise>`
*Multi-octave fractal Brownian motion noise texture with true noise evolution*

[Official Documentation](https://shaders.com/docs/components/fractalnoise)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#000000` | First color |
| `colorB` | `string` | `#ffffff` | Second color |
| `octaves` | `number` | `4` | Number of noise octaves (more = more detail) |
| `detail` | `number` | `2` | How much finer each successive octave becomes |
| `contrast` | `number` | `0.5` | How strongly finer octaves contribute — higher values create more texture contrast |
| `speed` | `number` | `0.15` | Speed at which the noise pattern evolves in place |
| `angle` | `number` | `0` | Rotation angle in degrees |
| `seed` | `number` | `0` | Random seed for pattern variation |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for interpolation |

#### React JSX Usage
```tsx
<Shader>
  <FractalNoise />
</Shader>
```

---

### 📦 `<Godrays>`
*Volumetric light rays emanating from a point*

[Official Documentation](https://shaders.com/docs/components/godrays)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `center` | `{x: number, y: number}` | `{"x":0,"y":0}` | The center point of the god rays |
| `density` | `number` | `0.3` | Frequency of ray sectors |
| `intensity` | `number` | `0.8` | Ray visibility within sectors |
| `spotty` | `number` | `1` | Density of spots on rays (higher = more spots) |
| `speed` | `number` | `0.5` | Animation speed of the rays |
| `rayColor` | `string` | `#4283fb` | Color of the light rays |
| `backgroundColor` | `string` | `transparent` | Background color |

#### React JSX Usage
```tsx
<Shader>
  <Godrays
    intensity={0.8}
  />
</Shader>
```

---

### 📦 `<Grid>`
*Simple grid lines pattern with adjustable thickness and rotation*

[Official Documentation](https://shaders.com/docs/components/grid)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ffffff` | The color of the grid lines |
| `cells` | `number` | `10` | Number of cells along the shortest canvas edge (creates square cells) |
| `thickness` | `number` | `1` | Thickness of grid lines (normalized, 0.0-1.0) |
| `rotation` | `number` | `0` | Rotation of the grid in degrees. At 45° this produces a crosshatch/diamond pattern. |

#### React JSX Usage
```tsx
<Shader>
  <Grid
    color="#ffffff"
  />
</Shader>
```

---

### 📦 `<HexGrid>`
*Honeycomb hexagonal grid pattern*

[Official Documentation](https://shaders.com/docs/components/hexgrid)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#000000` | Cell fill color |
| `colorB` | `string` | `#ffffff` | Grid line color |
| `cells` | `number` | `8` | Number of hexagons across the shortest canvas edge |
| `thickness` | `number` | `1` | Thickness of the hex grid lines |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <HexGrid />
</Shader>
```

---

### 📦 `<ImageTexture>`
*Display an image with customizable object-fit modes*

[Official Documentation](https://shaders.com/docs/components/imagetexture)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `url` | `string` | `https://shaders.com/sample.jpg` | Upload an image or provide a URL |
| `objectFit` | `"cover" \| "contain" \| "fill" \| "scale-down" \| "none"` | `cover` | How the image should be sized within the viewport |

#### React JSX Usage
```tsx
<Shader>
  <ImageTexture />
</Shader>
```

---

### 📦 `<LinearGradient>`
*Create smooth linear color gradients*

[Official Documentation](https://shaders.com/docs/components/lineargradient)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#1aff00` | The starting color of the gradient |
| `colorB` | `string` | `#0000ff` | The ending color of the gradient |
| `start` | `{x: number, y: number}` | `{"x":0,"y":0.5}` | The starting point of the gradient |
| `end` | `{x: number, y: number}` | `{"x":1,"y":0.5}` | The ending point of the gradient |
| `angle` | `number` | `0` | Additional rotation angle of the gradient (in degrees) |
| `edges` | `"stretch" \| "transparent" \| "mirror" \| "wrap"` | `stretch` | How to handle areas beyond the gradient endpoints |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <LinearGradient />
</Shader>
```

---

### 📦 `<Marble>`
*Classic marble swirl and vein texture using noise-warped sine waves*

[Official Documentation](https://shaders.com/docs/components/marble)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#ffffff` | Base background color of the marble |
| `colorB` | `string` | `#3a2d54` | Secondary marble tone |
| `colorC` | `string` | `#0f0f0f` | Deepest marble color |
| `scale` | `number` | `2` | Scale and density of the marble vein pattern |
| `turbulence` | `number` | `10` | Amount of noise-driven distortion applied to the veins |
| `speed` | `number` | `0.05` | Animation speed |
| `seed` | `number` | `0` | Random seed for pattern variation |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <Marble />
</Shader>
```

---

### 📦 `<MultiPointGradient>`
*Five individually placed color points blended together by proximity — drag each point to shape the gradient*

[Official Documentation](https://shaders.com/docs/components/multipointgradient)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#4776E6` | Color of control point A |
| `positionA` | `{x: number, y: number}` | `{"x":0.2,"y":0.2}` | Position of control point A |
| `colorB` | `string` | `#C44DFF` | Color of control point B |
| `positionB` | `{x: number, y: number}` | `{"x":0.8,"y":0.2}` | Position of control point B |
| `colorC` | `string` | `#1ABC9C` | Color of control point C |
| `positionC` | `{x: number, y: number}` | `{"x":0.2,"y":0.8}` | Position of control point C |
| `colorD` | `string` | `#F8BBD9` | Color of control point D |
| `positionD` | `{x: number, y: number}` | `{"x":0.8,"y":0.8}` | Position of control point D |
| `colorE` | `string` | `#FF8C42` | Color of control point E |
| `positionE` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Position of control point E |
| `smoothness` | `number` | `2` | Controls how smoothly colors blend. |

#### React JSX Usage
```tsx
<Shader>
  <MultiPointGradient />
</Shader>
```

---

### 📦 `<Plasma>`
*Animated effect of glowing plasma*

[Official Documentation](https://shaders.com/docs/components/plasma)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `density` | `number` | `2` | Density of the plasma pattern |
| `speed` | `number` | `2` | Animation speed |
| `intensity` | `number` | `1.5` | Brightness and spread of the plasma glow |
| `warp` | `number` | `0.4` | How much the flow distorts and swirls |
| `contrast` | `number` | `1` | Push darks darker and lights lighter |
| `balance` | `number` | `50` | Skew color balance toward A (higher) or B (lower) |
| `colorA` | `string` | `#7018be` | Primary color |
| `colorB` | `string` | `#000000` | Secondary color |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <Plasma
    intensity={1.5}
  />
</Shader>
```

---

### 📦 `<RadialGradient>`
*Radial gradient radiating from a center point*

[Official Documentation](https://shaders.com/docs/components/radialgradient)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#ff0000` | The starting color at the center of the gradient |
| `colorB` | `string` | `#0000ff` | The ending color at the edge of the gradient |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | The center point of the radial gradient |
| `radius` | `number` | `1` | The radius of the gradient (normalized, 0.0-1.0) |
| `repeat` | `number` | `1` | Number of times the gradient repeats. Values above 1 create concentric rings. |
| `aspect` | `number` | `1` | Stretches the gradient into an ellipse. Values below 1 compress vertically, above 1 compress horizontally. |
| `skewAngle` | `number` | `0` | Rotates the ellipse axis in degrees. Only visible when Aspect is not 1. |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <RadialGradient
    radius={1}
  />
</Shader>
```

---

### 📦 `<Ripples>`
*Concentric animated ripples emanating from a point*

[Official Documentation](https://shaders.com/docs/components/ripples)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | The center point where ripples emanate from |
| `colorA` | `string` | `#ffffff` | Color of the ripple waves |
| `colorB` | `string` | `#000000` | Background color between ripples |
| `speed` | `number` | `1` | Speed of ripple animation |
| `frequency` | `number` | `20` | Number of ripples/spacing between them |
| `softness` | `number` | `0` | Softness of ripple edges |
| `thickness` | `number` | `0.5` | Thickness of each ripple band |
| `phase` | `number` | `0` | Phase offset for ripple animation |

#### React JSX Usage
```tsx
<Shader>
  <Ripples />
</Shader>
```

---

### 📦 `<SimplexNoise>`
*Organic noise with animated movement*

[Official Documentation](https://shaders.com/docs/components/simplexnoise)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#ffffff` | First color |
| `colorB` | `string` | `#000000` | Second color |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |
| `scale` | `number` | `2` | Pattern scale (higher = larger patterns) |
| `balance` | `number` | `0` | Balance between colors (negative = more colorB, positive = more colorA) |
| `contrast` | `number` | `0` | Pattern contrast (higher = sharper transitions) |
| `seed` | `number` | `0` | Random seed for pattern variation |
| `speed` | `number` | `1` | Animation speed |

#### React JSX Usage
```tsx
<Shader>
  <SimplexNoise />
</Shader>
```

---

### 📦 `<SineWave>`
*Animated wave with thickness and softness*

[Official Documentation](https://shaders.com/docs/components/sinewave)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ffffff` | The color of the sine wave |
| `amplitude` | `number` | `0.15` | The height/amplitude of the sine wave |
| `frequency` | `number` | `1` | The frequency/number of wave cycles |
| `speed` | `number` | `1` | The animation speed of the wave |
| `angle` | `number` | `0` | The rotation angle of the wave (in degrees) |
| `position` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | The center position of the wave |
| `thickness` | `number` | `0.2` | The thickness of the wave line |
| `softness` | `number` | `0.4` | Edge softness of the wave line |

#### React JSX Usage
```tsx
<Shader>
  <SineWave
    color="#ffffff"
  />
</Shader>
```

---

### 📦 `<SolidColor>`
*Fill the canvas with a single solid color*

[Official Documentation](https://shaders.com/docs/components/solidcolor)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#5b18ca` | The solid color to display |

#### React JSX Usage
```tsx
<Shader>
  <SolidColor
    color="#5b18ca"
  />
</Shader>
```

---

### 📦 `<Spiral>`
*Rotating spiral pattern with animated movement*

[Official Documentation](https://shaders.com/docs/components/spiral)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#000000` | Background color |
| `colorB` | `string` | `#ffffff` | Spiral stroke color |
| `strokeWidth` | `number` | `0.5` | Thickness of spiral stroke |
| `strokeFalloff` | `number` | `0` | Stroke losing width further from center |
| `softness` | `number` | `0` | Color transition sharpness (0 = hard edge, 1 = smooth fade) |
| `speed` | `number` | `1` | Animation speed (negative values reverse direction) |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | The center point of the spiral |
| `scale` | `number` | `1` | Scale factor for spiral bands (higher = more bands, lower = fewer bands) |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <Spiral />
</Shader>
```

---

### 📦 `<Strands>`
*Procedural wavy strands with layered animation*

[Official Documentation](https://shaders.com/docs/components/strands)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `speed` | `number` | `0.5` | Overall animation speed |
| `amplitude` | `number` | `1` | Wave height amplitude |
| `frequency` | `number` | `1` | Wave frequency |
| `lineCount` | `number` | `12` | Number of wave lines |
| `lineWidth` | `number` | `0.1` | Width of wave lines |
| `waveColor` | `string` | `#f1c907` | Color of the waves |
| `pinEdges` | `boolean` | `true` | Pin waves at edges (fade effect) |
| `start` | `{x: number, y: number}` | `{"x":0,"y":0.5}` | Starting point of the waves |
| `end` | `{x: number, y: number}` | `{"x":1,"y":0.5}` | Ending point of the waves |

#### React JSX Usage
```tsx
<Shader>
  <Strands />
</Shader>
```

---

### 📦 `<Stripes>`
*Alternating colored stripes with animation*

[Official Documentation](https://shaders.com/docs/components/stripes)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#000000` | First stripe color |
| `colorB` | `string` | `#ffffff` | Second stripe color |
| `angle` | `number` | `45` | Angle of stripes in degrees |
| `density` | `number` | `5` | Number of stripe pairs visible |
| `balance` | `number` | `0.5` | Ratio of the two colors |
| `softness` | `number` | `0` | Edge softness |
| `speed` | `number` | `0.2` | Animation speed |
| `offset` | `number` | `0` | Phase offset for pattern positioning |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for interpolation |

#### React JSX Usage
```tsx
<Shader>
  <Stripes />
</Shader>
```

---

### 📦 `<StudioBackground>`
*Multi-light studio background with ambient motion.*

[Official Documentation](https://shaders.com/docs/components/studiobackground)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#d8dbec` | Base studio surface color |
| `keyColor` | `string` | `#d5e4ea` | Color of the overhead key light |
| `keyIntensity` | `number` | `40` | Intensity of the key light |
| `keySoftness` | `number` | `50` | How diffuse the key light is |
| `fillColor` | `string` | `#d5e4ea` | Color of the side fill lights |
| `fillIntensity` | `number` | `10` | Intensity of the fill lights |
| `fillSoftness` | `number` | `70` | How diffuse the fill lights are |
| `fillAngle` | `number` | `70` | How far apart the fill lights are from center |
| `backColor` | `string` | `#c8d4e8` | Color of the upward back wash |
| `backIntensity` | `number` | `20` | Intensity of the back wash |
| `backSoftness` | `number` | `80` | How diffuse the back wash is |
| `brightness` | `number` | `20` | Overall ambient light level |
| `vignette` | `number` | `0` | Edge darkening |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.8}` | Where the spotlight meets the floor |
| `lightTarget` | `number` | `100` | How far toward the floor vs wall the spotlights aim |
| `wallCurvature` | `number` | `10` | How rounded the cove is |
| `ambientIntensity` | `number` | `50` | Intensity of drifting ambient lights |
| `ambientSpeed` | `number` | `2` | Drift speed |
| `seed` | `number` | `0` | Seed for ambient pattern |

#### React JSX Usage
```tsx
<Shader>
  <StudioBackground
    color="#d8dbec"
  />
</Shader>
```

---

### 📦 `<SunBurst>`
*Radial sunburst rays emanating from a center point*

[Official Documentation](https://shaders.com/docs/components/sunburst)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `color` | `string` | `#ffdd88` | Ray color |
| `background` | `string` | `#000000` | Background color |
| `center` | `{x: number, y: number}` | `{"x":0.5,"y":0.5}` | Center point of the sunburst |
| `rayCount` | `number` | `12` | Number of rays |
| `softness` | `number` | `0.3` | Softness of ray edges |
| `radius` | `number` | `0.8` | How far the rays extend from the center |
| `feather` | `number` | `0.5` | How gradually the rays fade at their outer edge |
| `speed` | `number` | `0.2` | Rotation speed — positive values rotate clockwise |

#### React JSX Usage
```tsx
<Shader>
  <SunBurst
    color="#ffdd88"
    radius={0.8}
  />
</Shader>
```

---

### 📦 `<Swirl>`
*Flowing swirl pattern with multi-layered noise*

[Official Documentation](https://shaders.com/docs/components/swirl)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#1275d8` | Primary gradient color |
| `colorB` | `string` | `#e19136` | Secondary gradient color |
| `speed` | `number` | `1` | Flow animation speed |
| `detail` | `number` | `1` | Level of detail and intricacy in the swirl patterns |
| `blend` | `number` | `50` | Skew color balance toward A (lower values) or B (higher values) |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <Swirl />
</Shader>
```

---

### 📦 `<Truchet>`
*Quarter-circle arc tiles that connect to form organic, maze-like flowing curves*

[Official Documentation](https://shaders.com/docs/components/truchet)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#000000` | Background color between the arcs |
| `colorB` | `string` | `#ffffff` | Arc line color |
| `cells` | `number` | `10` | Number of tiles across the shortest canvas edge |
| `thickness` | `number` | `2` | Thickness of the arc lines |
| `seed` | `number` | `0` | Random seed — changes which tiles flip, producing a different maze pattern |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <Truchet />
</Shader>
```

---

### 📦 `<VideoTexture>`
*Display a video with customizable playback and object-fit modes*

[Official Documentation](https://shaders.com/docs/components/videotexture)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `url` | `string` | `https://shaders.com/sample.mp4` | Upload a video or provide a URL |
| `objectFit` | `"cover" \| "contain" \| "fill" \| "scale-down" \| "none"` | `cover` | How the video should be sized within the viewport |
| `loop` | `boolean` | `true` | Loop the video playback |

#### React JSX Usage
```tsx
<Shader>
  <VideoTexture />
</Shader>
```

---

### 📦 `<Voronoi>`
*Cellular pattern where each pixel is colored by its distance to the nearest of many scattered points*

[Official Documentation](https://shaders.com/docs/components/voronoi)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#3186cf` | Color near each cell's center point |
| `colorB` | `string` | `#fc02dd` | Color at cell boundaries, far from any center point |
| `colorBorder` | `string` | `#000000` | Color of the cell boundary lines |
| `scale` | `number` | `6` | Number of cells across the canvas |
| `speed` | `number` | `0.5` | Animation speed — how fast the cell points drift |
| `seed` | `number` | `0` | Random seed — shifts the cell pattern without changing the overall structure |
| `edgeIntensity` | `number` | `0.5` | Controls how much of the cell interior is filled by the edge color. Low = center color dominates with a sharp boundary. High = edge color spreads further into the cell. |
| `edgeSoftness` | `number` | `0.05` | Width of the cell boundary lines. |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `oklch` | Color space for color interpolation |

#### React JSX Usage
```tsx
<Shader>
  <Voronoi />
</Shader>
```

---

### 📦 `<Weave>`
*Interlaced textile weave pattern with two thread colors going over and under each other*

[Official Documentation](https://shaders.com/docs/components/weave)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#c4c4c4` | Horizontal thread color |
| `colorB` | `string` | `#4d4d4d` | Vertical thread color |
| `cells` | `number` | `10` | Number of threads across the shortest canvas edge |
| `gap` | `number` | `0.25` | Gap between threads (0 = no gap, 0.5 = maximum gap) |
| `rotation` | `number` | `0` | Rotation of the weave pattern in degrees |

#### React JSX Usage
```tsx
<Shader>
  <Weave />
</Shader>
```

---

### 📦 `<WebcamTexture>`
*Display a live webcam feed with customizable object-fit modes*

[Official Documentation](https://shaders.com/docs/components/webcamtexture)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `objectFit` | `"cover" \| "contain" \| "fill" \| "scale-down" \| "none"` | `cover` | How the webcam feed should be sized within the viewport |
| `mirror` | `boolean` | `true` | Mirror the webcam feed horizontally (selfie mode) |

#### React JSX Usage
```tsx
<Shader>
  <WebcamTexture />
</Shader>
```

---

### 📦 `<WorleyNoise>`
*Cellular noise field — distance-based, with selectable feature combinations and fractal octaves*

[Official Documentation](https://shaders.com/docs/components/worleynoise)

#### Props
| Prop Name | Type | Default Value | Description |
| --- | --- | --- | --- |
| `colorA` | `string` | `#ffffff` | Color where the noise field is low (typically near cell centers) |
| `colorB` | `string` | `#000000` | Color where the noise field is high (typically near cell boundaries) |
| `colorSpace` | `"linear" \| "oklch" \| "oklab" \| "hsl" \| "hsv" \| "lch"` | `linear` | Color space for color interpolation |
| `scale` | `number` | `6` | Number of cells across the canvas at the base octave |
| `mode` | `"f1" \| "f2" \| "f2MinusF1" \| "f1PlusF2" \| "f1TimesF2"` | `f1` | Field type. F1 = distance to nearest point. F2 = distance to second-nearest. F2 − F1 emphasises cell boundaries. |
| `distance` | `"euclidean" \| "manhattan" \| "chebyshev"` | `euclidean` | Distance metric. Euclidean = round cells. Manhattan = diamond. Chebyshev = square. |
| `octaves` | `number` | `1` | Number of fractal layers stacked at progressively finer scales |
| `lacunarity` | `number` | `2` | Scale multiplier between octaves (only active when Octaves > 1) |
| `persistence` | `number` | `0.5` | Amplitude multiplier between octaves (only active when Octaves > 1) |
| `jitter` | `number` | `1` | How much each cell's point drifts inside its cell. 0 = rigid grid (banded look), 1 = fully random. |
| `contrast` | `number` | `1` | Steepness of the gradient between low and high regions |
| `balance` | `number` | `0` | Shifts the gradient midpoint. Negative pulls the field toward Color A, positive toward Color B. |
| `seed` | `number` | `0` | Random seed — shifts the cell pattern without changing its overall structure |
| `speed` | `number` | `0.5` | Animation speed — how fast each cell's point drifts |

#### React JSX Usage
```tsx
<Shader>
  <WorleyNoise />
</Shader>
```

---
