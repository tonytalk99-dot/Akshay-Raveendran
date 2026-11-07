# Motion Design & Interactive Physics Specification

## 1. Interaction Principles
The motion design balances **tactile physical weight** with **effortless digital fluidity**:
- **Physicality over abstraction:** Discs obey inertia, friction, and spatial banking.
- **Micro-responsive feedback:** Every cursor movement induces subtle parallax, anisotropic sheen shift, and magnetic lift.
- **Zero jarring jumps:** All state transitions employ spring damping ($lerp$) with continuous derivative continuity ($C^2$ smoothness).

---

## 2. Cursor Proximity & 3D Gyroscopic Tilt

### Global Viewport Parallax
As the user moves their cursor across the viewport, the entire 3D stage undergoes subtle camera and mesh tilt to create perceptible depth:

$$\Delta\theta_x = -(\text{mouseY} / \text{windowHeight} - 0.5) \times 0.12\text{ rad}$$
$$\Delta\theta_y = (\text{mouseX} / \text{windowWidth} - 0.5) \times 0.15\text{ rad}$$

- Camera position shifts by up to `0.20` units in opposite direction, generating natural foreground-background ocular parallax.
- High-frequency mouse jitter is filtered through a 1st-order low-pass filter ($\alpha = 0.08$).

### Local Disc Hover & Magnetic Lift
When the mouse raycaster intersects an individual disc mesh:
1. **Z-Lift (Magnetic Attraction):**
   - The hovered disc elevates forward toward the viewer by `+0.28` units along its local normal.
   - Scale increments smoothly from `1.0` to `1.045`.
2. **Dynamic Surface Tilt:**
   - The disc angles slightly toward the cursor position within its bounding radius:
     $$\text{disc.rotation.x} += (\text{localCursorY} \times 0.25 - \text{disc.rotation.x}) \times 0.1$$
     $$\text{disc.rotation.y} += (\text{localCursorX} \times 0.25 - \text{disc.rotation.y}) \times 0.1$$
3. **Specular Highlight Tracking:**
   - As the disc tilts, the virtual directional light catches the clear polycarbonate inner hub and metallic rim, producing a realistic swept reflection.

---

## 3. Hand-Drawn Sketch Halo Animation
The active disc halo (referencing `Ref-1.png` and `vid_frame_5s.jpg`) has two distinct animated states:

1. **Draw-In Trigger (On Disc Change):**
   - SVG stroke-dashoffset transitions from `100%` to `0%` over `450ms` using `cubic-bezier(0.16, 1, 0.3, 1)`.
   - Simulates a real charcoal pencil tracing two quick loose loops around the disc perimeter.
2. **Ambient Breathing Wobble (Idle State):**
   - Subtle sine-wave modulation applied to the SVG path points:
     $$\text{scale}(t) = 1.0 + 0.015 \times \sin(2\pi \times 0.8 \times t)$$
     $$\text{rotation}(t) = \text{baseAngle} + 1.2^\circ \times \sin(2\pi \times 0.5 \times t)$$
   - Gives the halo an organic, alive, editorial sketch feel.

---

## 4. DOM HUD Transitions & Easing

### Title & Metadata Transition Choreography
When the active index shifts from $N \to N+1$:
- **Step 1 ($0\text{ms}$):** Previous title slides down by `8px` and fades from opacity `1.0 -> 0.0` ($120\text{ms}$).
- **Step 2 ($80\text{ms}$):** Metadata values (Artist, Year, Edition, Medium) cross-fade with a staggered slide (`80ms` delay, `180ms` duration).
- **Step 3 ($140\text{ms}$):** New title enters from top (`-12px -> 0px`) and fades from opacity `0.0 -> 1.0` ($320\text{ms}$, `cubic-bezier(0.16, 1, 0.3, 1)`).
- **Step 4 ($180\text{ms}$):** Bottom reviews cross-fade with 5-star rating fill animation.

### Index Modal Dropdown Transition
- Dropdown container scales vertically from `0.95 -> 1.0` with origin top-center.
- Backdrop blurs smoothly from `0px -> 8px` with dimming scrim opacity `0.0 -> 0.15`.
- Individual list items stagger in with `15ms` cascaded delays.

---

## 5. Easing Curve Dictionary
| Easing Name | Curve Definition | Usage |
| :--- | :--- | :--- |
| **Editorial Quintic** | `cubic-bezier(0.16, 1, 0.3, 1)` | Disc deceleration, Title transitions, Modal reveals |
| **Magnetic Spring** | `cubic-bezier(0.34, 1.56, 0.64, 1)` | Disc hover lift, Active indicator snap |
| **Smooth Parallax** | Linear Lerp ($k = 0.075$) | Mouse tracking, Virtual scroll damping |
| **Gentle Fade** | `cubic-bezier(0.4, 0, 0.2, 1)` | Review quotes cross-fade, Backdrop blur |
