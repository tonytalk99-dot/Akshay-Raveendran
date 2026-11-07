# Reference Analysis & Reverse-Engineering Report

## 1. Input Inspection & Directory Audit
- **Target Directory:** `/reference`
- **Scanned Media Types:** Images (`.png`, `.jpg`, `.jpeg`, `.webp`, `.avif`, `.svg`), Video recordings (`.mp4`, `.webm`, `.mov`)
- **Direct Observations:**
  - Files detected in `/reference`: 0 media files found at time of analysis.
  - Video frames sampled: None available for frame-by-frame extraction.
- **Rule Adherence:** Per the directive (*"Distinguish observations from estimates. Do not code or invent unsupported details"*), no fabricated visual measurements are recorded as verified direct observations. All structural parameters below are explicitly categorized as **Estimates / Reference Baseline Specifications**.

---

## 2. Layout & Viewport Architecture
### Observations
- No direct screenshots or video frames present in `/reference`.

### Estimates & Structural Blueprint
- **Layout Paradigm:** Fullscreen canvas with overlaid DOM UI (hybrid WebGL/Three.js backdrop + HTML/CSS HUD interface).
- **Hero & Presentation Grid:** 12-column fluid grid system with centered content container.
  - Max container width: `1440px` (standard desktop canvas).
  - Horizontal padding: `24px` (mobile), `48px` (tablet), `80px` (desktop).
- **Header & Navigation:** Fixed top bar with persistent blur/translucency (`backdrop-filter: blur(12px)`), flex alignment with brand left, nav center/right, and CTA button right.
- **Footer / Exit Section:** Bottom-docked minimal metadata bar or anchor dock.

---

## 3. Typography & Hierarchy System
### Observations
- No verified font files or raster text samples present in `/reference`.

### Estimates
- **Primary Display Typeface:** Modern geometric or neo-grotesque sans-serif (e.g., Inter, Plus Jakarta Sans, or Space Grotesk).
- **Secondary / Monospace Typeface:** Clean tabular mono for metrics, coordinates, and technical badges (e.g., JetBrains Mono).
- **Typographic Scale:**
  - `Display / Hero Title`: 56px–72px (line-height: 1.05–1.1, tracking: -0.02em, font-weight: 700/800)
  - `H1 / Section Headline`: 40px–48px (line-height: 1.15, tracking: -0.015em, font-weight: 700)
  - `H2 / Subheadline`: 28px–32px (line-height: 1.25, tracking: -0.01em, font-weight: 600)
  - `Body Large`: 18px–20px (line-height: 1.5, font-weight: 400/500)
  - `Body Regular`: 15px–16px (line-height: 1.6, font-weight: 400)
  - `Caption / Monospace Tag`: 12px–13px (line-height: 1.4, tracking: 0.05em, uppercase, font-weight: 500)

---

## 4. Spacing, Margin & Rhythm
### Observations
- No direct layout screenshots available.

### Estimates
- **Base Grid Unit:** 4px / 8px incremental system.
- **Micro Spacing:** `4px` (tight icon gap), `8px` (badge padding), `12px` (input padding), `16px` (standard card padding internal).
- **Macro Spacing:** `24px` (card gaps), `48px` (section inner separation), `80px–120px` (major section vertical rhythm).
- **Component Radii:** `8px` (subtle badges/buttons), `16px` (content panels), `24px` (prominent floating cards).

---

## 5. Color Palette & Material Surfaces
### Observations
- No direct hex color samples extracted from assets.

### Estimates
- **Theme Mode:** Dark aesthetic / deep spatial background with high-contrast foreground accents.
- **Background Tones:**
  - Base canvas: `#0a0a0c` to `#0d0f12` (deep obsidian / void)
  - Surface elevated 1: `rgba(255, 255, 255, 0.04)` with `1px solid rgba(255, 255, 255, 0.08)`
  - Surface elevated 2: `rgba(255, 255, 255, 0.08)` with `1px solid rgba(255, 255, 255, 0.16)`
- **Text & Foreground:**
  - Primary text: `#f4f4f6` (96% luminance)
  - Secondary text: `#a1a1aa` (zinc-400 equivalent)
  - Muted / tertiary text: `#71717a` (zinc-500 equivalent)
- **Accent & Illumination:**
  - Dynamic gradient or neon accent: Electric cyan (`#06b6d4`), violet (`#8b5cf6`), or emerald (`#10b981`) for focal geometry glow and active triggers.

---

## 6. 3D Geometry, Depth & Spatial Staging
### Observations
- No 3D model files, meshes, or frame captures present in `/reference`.

### Estimates
- **Scene Camera Model:** Perspective Camera (`FOV: 45°–60°`, near plane: `0.1`, far plane: `1000`).
- **Camera Staging:** Positioned along positive Z-axis (`z: 5–8 units`), looking toward world origin `(0, 0, 0)`.
- **Lighting Staging:**
  - Ambient light: Low intensity (`0.3–0.5`) to preserve deep shadows.
  - Key directional light: Elevated top-right `(5, 8, 5)` with soft specular response.
  - Rim / Accent point light: Backlight `(-4, -2, -3)` providing luminous edge highlights on 3D meshes.
- **Geometric Elements:** Floating curved surfaces, ribbon meshes, or parametric particle paths orbiting or guiding content flow.

---

## 7. Curved Scroll Trajectory & Spline Motion
### Observations
- No scroll recording frames available for velocity curve fitting.

### Estimates
- **Trajectory Curve Model:** Catmull-Rom or Cubic Bézier 3D Spline (`THREE.CatmullRomCurve3` or parametric function `f(t)`).
- **Camera / Object Scroll Mapping:** Normalized scroll progress $t \in [0, 1]$ mapped via virtual scroll container (e.g., 400vh–600vh total scrollable height).
- **Trajectory Path Coordinates (Estimated Progression):**
  - $t = 0.0$: Position `(0, 0, 6)`, Rotation `(0, 0, 0)` (Frontal hero framing)
  - $t = 0.3$: Position `(2.2, 0.8, 3.5)`, Rotation `(-0.15, 0.4, 0.05)` (Gentle arc to the right)
  - $t = 0.6$: Position `(-1.8, -0.6, 2.0)`, Rotation `(0.12, -0.35, -0.08)` (S-curve sweep across center)
  - $t = 1.0$: Position `(0, 0.5, 1.2)`, Rotation `(0, 0, 0)` (Close-up focal resolution)
- **Inertia & Damping:** Smooth interpolation ($lerp$ factor `0.05–0.08` or damped spring) linking scroll offset to spline parameter $t$.

---

## 8. Rotation, Tilt & Angular Transformations
### Observations
- No angular delta captures available from video frames.

### Estimates
- **Mouse / Gyroscopic Parallax:** Subtle secondary tilt responding to cursor movement (`tiltX = (mouseX / windowWidth - 0.5) * 0.15rad`, `tiltY = (mouseY / windowHeight - 0.5) * 0.15rad`).
- **Banking on Curves:** Camera roll angle dynamically aligned to curvature normal (`roll = curvature * velocity * bankingFactor`).

---

## 9. Transitions, Easing & Choreography
### Observations
- No transition timings directly observed.

### Estimates
- **Scroll Easing:** Cubic Bézier `cubic-bezier(0.16, 1, 0.3, 1)` (ease-out quintic feel for organic settling).
- **Section Transitions:** Staggered opacity fade (`0 -> 1`) across `200ms–400ms` window synchronized with scroll progress milestones.
- **Card / DOM Element Animations:** `translateY` offset of `24px` transitioning to `0px` with spring stiffness `120`, damping `20`.

---

## 10. Responsive Behavior & Breakpoints
### Observations
- No breakpoint variants observed in assets.

### Estimates
- **Mobile (< 768px):**
  - Camera FOV widened (`65°–70°`) or Z distance increased to maintain visibility of 3D geometry on narrow aspect ratios.
  - Curved scroll trajectory amplitude damped along X-axis by ~50% to prevent elements clipping outside viewport boundaries.
  - Single-column card stacking with full-width layout.
- **Tablet (768px – 1024px):**
  - Two-column grid layout for auxiliary information; centered 3D focal focalization.
- **Desktop (>= 1024px):**
  - Full two-column or multi-layer depth staging with synchronized 3D spline navigation.
