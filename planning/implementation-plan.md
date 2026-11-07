# Implementation Plan: Premium 3D Collectible-Art Website

## 1. Technical Stack & Architecture
- **Framework:** React 19 (SPA on Vite + TypeScript)
- **3D Graphics Engine:** Three.js (`three` + `@types/three`)
- **Styling:** Tailwind CSS v4 (`@import "tailwindcss";`)
- **Animation & Transitions:** `motion` (Framer Motion v12) + custom Lerp damping in RAF loop
- **Icons:** `lucide-react` (minimalist, razor-thin glyphs)
- **Fonts:** System serif fallback + Google Fonts (`Cinzel` display serif, `Cormorant Garamond` editorial body, `Inter` UI sans)

---

## 2. Directory Structure & Modular Components

```
src/
├── assets/                  # Verified disc textures (1.webp through 6.webp)
├── components/
│   ├── CanvasStage.tsx      # Three.js canvas mount, WebGLRenderer, RAF loop
│   ├── Disc3D.ts            # Class / factory constructing 3D composite disc mesh
│   ├── SplineTrajectory.ts  # CatmullRomCurve3 math, tangent banking & sampling
│   ├── SketchHalo.tsx       # SVG hand-drawn lasso tracking active disc in screen space
│   ├── Header.tsx           # Center logo, category switcher, Index drawer toggle
│   ├── MetadataRail.tsx     # Top-left dynamic display title & metadata table
│   ├── ReviewsRail.tsx      # Bottom curatorial quotes & 5-star rating block
│   ├── IndexDrawer.tsx      # Floating sheet dropdown listing all 6 collectibles
│   ├── DiscDetailModal.tsx  # Archival deep-dive view & fictional acquisition panel
│   └── AudioFeedback.ts     # Subtle vinyl click sound synthesis (Web Audio API)
├── data/
│   └── websiteContent.ts    # Typed TypeScript dataset synced with website-content.json
├── types/
│   └── index.ts             # Artwork, Review, Trajectory & Viewport TypeScript interfaces
├── App.tsx                  # Master state, virtual scroll coordinator, input routing
└── main.tsx                 # React DOM root entry
```

---

## 3. Step-by-Step Implementation Sequence

### Phase 1: Dependency Setup & Typography Assets
1. Install `three` and `@types/three` via single batched `install_applet_package`.
2. Configure Google Fonts (`Cinzel`, `Cormorant Garamond`, `Inter`) in `index.html`.
3. Set page metadata, title, and canvas background `#F4F2EC` in `index.html` and `metadata.json`.

### Phase 2: Data Models & Asset Verification
1. Create `src/types/index.ts` declaring interfaces for `Artwork`, `FictionalCommerce`, and `DiscSplineState`.
2. Create `src/data/websiteContent.ts` importing and validating the 6 WebP artwork paths (`/assets/1.webp` through `/assets/6.webp`).

### Phase 3: Three.js 3D Disc Engine & Texture Mapping
1. Implement `Disc3D.ts` constructing:
   - Outer extruded cylinder ring ($R=1.95$, bevel $0.008$)
   - Inner hole cutout ($R=0.38$)
   - Polycarbonate clear clamp hub ($R=0.38 \to 0.65$, transmission $0.92$)
   - Texture-mapped front printed face with UV orientation
   - Mirror-finish aluminum outer rim ($R=1.90 \to 1.95$, metalness $0.96$)
   - Brushed metal backplate with serial number engraving
2. Set up Three.js Three-Point Lighting rig and soft shadow-catcher plane.

### Phase 4: Diagonal Curved Trajectory & Inertia Physics
1. Implement `SplineTrajectory.ts`:
   - 7-point 3D Catmull-Rom spline extending diagonally from $(-6.2, -2.8, -3.2)$ to $(+7.0, +3.1, -2.8)$.
   - Real-time tangent calculation to compute bank angles (roll, pitch, yaw).
2. Create virtual scroll controller in `App.tsx`:
   - Mouse wheel, pointer drag, touch swipe, and keyboard arrow routing.
   - Smooth $lerp$ damping ($k = 0.075$) + magnetic snapping to nearest integer index on release.
   - Clamp with soft spring boundary bounce.

### Phase 5: Hand-Drawn Sketch Halo & DOM Editorial HUD
1. Implement `SketchHalo.tsx`:
   - Projects 3D active disc center to 2D screen coordinates using `vector.project(camera)`.
   - Renders animated charcoal pencil loop with subtle organic breathing oscillation.
2. Build `Header.tsx`:
   - Center brand logo `AURA ARCHIVE`, category pills with active dot, and `Index 6 v` trigger.
3. Build `MetadataRail.tsx`:
   - Display title in `Cinzel` serif with split-fade transition.
   - Tabular metadata ledger: Artist, Year, Edition, Medium.
4. Build `ReviewsRail.tsx`:
   - Two-column press endorsements with 5-star ratings and publication logos.

### Phase 6: Interactive Index Drawer & Detail Modal
1. Implement `IndexDrawer.tsx`:
   - Dropdown modal revealing all 6 collectible discs with quick-nav jump.
   - Clicking an item smoothly glides the 3D spline directly to that disc.
2. Implement `DiscDetailModal.tsx`:
   - Full-bleed inspect mode featuring artist statements, high-res texture zoom, and fictional purchase simulation with receipt generation.

### Phase 7: Responsive Tuning & Final Polish
1. Adjust camera FOV ($45^\circ \to 58^\circ$) on mobile screens.
2. Collapse left rail and reviews into clean vertical touch layout on viewports $< 768\text{px}$.
3. Verify 60 FPS performance, run `compile_applet` and `lint_applet`.

---

## 4. Verification & Reference Fidelity Checklist
- [ ] **Curved Diagonal Trajectory:** Discs travel along authentic diagonal arc from lower-left to upper-right.
- [ ] **Material Tactility:** Clear center hole, polycarbonate inner ring, satin artwork print, and metallic outer rim reflect realistic specular glints.
- [ ] **Sketch Loop Halo:** Active disc encircled by hand-drawn charcoal loop matching `Ref-1.png`.
- [ ] **Negative Space & Typography:** Expansive archival off-white canvas with razor-sharp editorial serif headlines.
- [ ] **Smooth Input:** Fluid mouse wheel, pointer drag, and touch inertia with soft magnetic snap.
- [ ] **Verified Artwork:** All 6 user artworks properly mapped and labeled.
- [ ] **Fictional Commerce:** All commercial details clearly flagged as narrative fiction.
