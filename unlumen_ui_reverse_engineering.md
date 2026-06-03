# Unlumen UI Creative Engineering & Reverse-Engineering Manual

This manual provides an in-depth analysis of the mathematics, physics algorithms, GLSL shaders, and Framer Motion configurations behind the core components in the **Unlumen UI** library.

---

## 1. WebGL Water Ripple & Pixel Liquid Shaders

### Mathematics of Distance Attenuation
Shaders like the **Pixel Liquid Background** and **LiquidChrome** implement cursor-proximity distortion using a radial falloff formula based on exponential decay:

$$\text{falloff} = e^{-\text{dist} \cdot K}$$

Where:
* $\text{dist} = \sqrt{(x_{\text{pixel}} - x_{\text{mouse}})^2 + (y_{\text{pixel}} - y_{\text{mouse}})^2}$ is the Euclidean distance from the pixel to the normalized cursor coordinates.
* $K$ is the falloff coefficient (typically set to `20.0` to constrain distortion to the immediate cursor vicinity).

### Water Ripple Wave Simulation
To create organic water ripples radiating outwards, a sine-wave function is modulated by time and distance:

$$\text{ripple} = \sin(\lambda \cdot \text{dist} - t \cdot \omega) \cdot A$$

Where $\lambda$ is spatial frequency (wave density, e.g., `10.0`), $\omega$ is temporal frequency (speed of propagation, e.g., `2.0`), and $A$ is the max wave amplitude (`0.03`).
The coordinate deflection vector is calculated as:

$$\vec{u}_{\text{new}} = \vec{u} + \left( \frac{\vec{u} - \vec{u}_{\text{mouse}}}{\text{dist} + \epsilon} \right) \cdot \text{ripple} \cdot \text{falloff}$$

This offsets the texture/color lookup coordinates, yielding the refractive liquid metal deflection around the mouse.

---

## 2. Direction-Aware Spring Animation (Animate Digits)

### Spring-Jump Physics
The `AnimateDigits` component uses spring-driven position tracking rather than standard linear timelines. When a digit increments or decrements, the system dynamically calculates the travel vector to create physical momentum.

1. **Velocity / Position Reset (Spring Jump)**:
   Instead of fading out and in on a static coordinate, the spring's internal velocity is reset to simulate a "flick":
   ```typescript
   y.jump(up ? enterY : -enterY);
   opacity.jump(0);
   scale.jump(enterScale);
   blur.jump(enterBlur);
   ```
2. **Spring Restoring Force**:
   The coordinates are then set to slide back to `0` using Framer Motion's standard spring formula:
   
   $$F = -k \cdot x - c \cdot v$$
   
   Where $k$ is stiffness (spring tension, e.g., `170`), $c$ is damping (resistance to oscillation, e.g., `10`), $x$ is displacement, and $v$ is velocity. This draws the digit back to rest with a subtle, fluid bounce.

---

## 3. Mouse-Proximity Magnification (Dock Navigation Menu)

The magnification of icons inside the macOS-style `Dock` menu is calculated dynamically using distance thresholds in 2D viewport coordinates.

### Proximity Formula
Given the mouse X-coordinate $M_x$ and the horizontal center of an icon $I_x$, the distance is:

$$d = |M_x - I_x|$$

The width of the icon $W$ is mapped using a cosine interpolation curve (to smooth the magnification peak) bounded by a max range $R$:

$$W(d) = \begin{cases} 
  W_{\text{base}} + (W_{\text{max}} - W_{\text{base}}) \cdot \cos\left(\frac{\pi \cdot d}{2R}\right) & \text{if } d \le R \\ 
  W_{\text{base}} & \text{if } d > R 
\end{cases}$$

Framer Motion maps this in real-time using `useTransform` and `useMotionValue`, converting distance outputs directly to inline CSS width parameters without triggering standard React DOM rendering cycles, keeping calculations at a locked 60fps.

---

## 4. Velocity-Triggered Image Trails (Cursor Image Trail)

The image trail trailing the mouse cursor does not render on a simple timer; it operates on a spatial/velocity-based grid queue.

### Tracking Queue & Threshold
1. **Delta Distance Threshold**:
   The mouse coordinates are tracked in a queue. A new image is only appended when the distance traveled by the cursor since the last placement exceeds a specific pixel threshold ($T$):
   
   $$\Delta d = \sqrt{(x_{\text{current}} - x_{\text{last}})^2 + (y_{\text{current}} - y_{\text{last}})^2} > T \text{ px}$$
   
2. **Velocity-Modulated Scale**:
   The scale and rotation of each spawned image are proportional to the velocity of the cursor at the moment of creation:
   
   $$S = S_{\text{base}} + \min\left(v \cdot M, S_{\text{max}}\right)$$
   
   This ensures that fast, sweeping gestures create larger, more dynamic trails, while slow movements keep the trail tight and subtle.
