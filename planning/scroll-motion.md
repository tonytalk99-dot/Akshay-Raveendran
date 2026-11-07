# 3D Curved Diagonal Scroll Motion & Trajectory

## 1. Observed Trajectory Kinematics
In the reference video (`sa24.raviklaassens.com`), the discs do not translate on a flat 2D plane or simple horizontal slider. Instead, they ride a **3-dimensional Catmull-Rom spline curve** that cuts diagonally across the screen:
- **Origin / Entry (Lower-Left Depth):** Discs emerge from the lower-left background, smaller in scale, receding into negative Z-space, tilted backwards.
- **Apex / Focus (Center-Stage Forefront):** The active disc glides into the focal viewport sweetspot (`X ≈ 0.0`, `Y ≈ -0.15`, `Z ≈ 0.6`), oriented nearly perpendicular to the camera to allow clear reading of title typography and artwork.
- **Ascent / Exit (Upper-Right Elevation):** Discs ascend steeply toward the upper-right corner, expanding in apparent scale due to camera proximity before dipping into top-right horizon.

```
       Trajectory Axis
             ^             [DISC N+2] (x: +5.4, y: +2.4, z: -1.5)
             |                 \
             |              [DISC N+1] (x: +2.6, y: +0.8, z: +0.2)
             |                   /
             |            [ACTIVE DISC N] (x: 0.0, y: -0.15, z: +0.6)
             |                /
             |         [DISC N-1] (x: -2.6, y: -0.9, z: -0.6)
             |             /
             |      [DISC N-2] (x: -5.2, y: -2.0, z: -2.2)
             +---------------------------------------------------->
```

---

## 2. 3D Spline Path Definition (`CatmullRomCurve3`)

The global spatial path is calculated using a tension-adjusted 3D spline:

```typescript
const splinePoints = [
  new THREE.Vector3(-6.2, -2.8, -3.2), // Entry tail
  new THREE.Vector3(-4.2, -1.8, -1.8), // Distant left
  new THREE.Vector3(-2.4, -0.85, -0.6), // Left neighbor
  new THREE.Vector3(0.0, -0.15, 0.65), // Active focal position
  new THREE.Vector3(2.55, 0.78, 0.25), // Right neighbor
  new THREE.Vector3(4.8, 1.95, -1.2), // Distant right
  new THREE.Vector3(7.0, 3.1, -2.8), // Exit tail
];

const curve = new THREE.CatmullRomCurve3(splinePoints, false, 'centripetal', 0.5);
```

### Parametric Disc Mapping
For an array of $N = 6$ collectible discs, each disc $i \in [0, 5]$ receives a continuous normalized spline coordinate $t_i$:

$$t_i = 0.5 + (i - \text{scrollProgress}) \times \Delta s$$

Where:
- $\text{scrollProgress} \in [0, 5]$ is the virtual scroll index with sub-pixel float precision.
- $\Delta s \approx 0.165$ is the uniform spline spacing between adjacent discs.
- When $i = \text{scrollProgress}$, disc $i$ sits precisely at $t = 0.5$ (the center focal apex).

---

## 3. Dynamic Rotation, Banking & Orientation
To achieve the tactile realism seen in `Ref-1.png` through `Ref-4.png`, rotation is not static; it calculates tangent banking and viewing angles:

1. **Curve Tangent Alignment:**
   - The disc's forward normal derives from the curve tangent $\vec{T}(t) = \frac{d\vec{C}}{dt}$.
   - Pitch angle (X-axis tilt): Discs on the left tilt slightly upward ($+12^\circ$), center disc sits at near neutral ($-3^\circ$), and right discs pitch downward ($-15^\circ$).
   - Yaw angle (Y-axis face): Discs angle naturally inward toward the camera axis ($+22^\circ$ on left, $+5^\circ$ at center, $-18^\circ$ on right).
   - Roll angle (Z-axis tilt): Discs bank into the curve direction like a racing bicycle ($+6^\circ$ to $-10^\circ$).

2. **Continuous Subtle Spin:**
   - Discs rotate slowly on their internal Z-axis ($0.002\text{ rad/frame}$) when idling, giving an organic physical presence like vinyl spinning on an invisible turntable.

---

## 4. Inertia, Damping & Magnetic Snap

### A. Virtual Scroll Input Processing
- **Wheel Input:** Mouse wheel deltas `e.deltaY` scale by `0.0018` and add directly to `targetScroll`.
- **Pointer Drag / Touch Swipe:** Horizontal and diagonal drag `(deltaX * 0.7 + deltaY * -0.3) * 0.0025` updates `targetScroll`.
- **Keyboard Arrow Support:** Left / Right keys increment or decrement `targetScroll` by exactly $1.0$.

### B. Two-Stage Damping Physics
1. **Kinetic Gliding Phase (During Active Scroll/Drag):**
   ```typescript
   currentScroll += (targetScroll - currentScroll) * 0.075;
   ```
2. **Magnetic Rest Phase (Post Scroll Settling):**
   - Once input stops for $> 180\text{ms}$ and velocity falls below threshold ($|\Delta| < 0.004$):
   - `targetScroll` gently attracts to the nearest whole integer index:
     ```typescript
     targetScroll = Math.round(targetScroll);
     ```
   - Clamped strictly between `[0, totalDiscs - 1]` with soft elastic overscroll resistance at boundaries ($k = 0.25$).

---

## 5. Active Disc Selection & Hand-Drawn Sketch Halo
- When `Math.round(currentScroll)` identifies the active index:
  1. The SVG sketch ring animates to circle the active disc.
  2. The left-hand metadata ledger updates with a staggered micro-fade ($150\text{ms}$ ease-out).
  3. The bottom press quotes cross-fade to the new artwork's critical reviews.
  4. Audio tactile feedback (optional subtle vinyl mechanical click sound) confirms selection milestone.
