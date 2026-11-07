# 3D Disc System Specification

## 1. Physical Anatomy & Reference Fidelity
In the reference recordings (`a24.raviklaassens.com`), the interactive hero objects are tactile, high-fidelity physical discs (hybrid compact disc / optical vinyl aesthetic). Each disc is a true 3D object with depth, edge bevels, clear polycarbonate center hub, and distinct material behaviors between the printed artwork surface, the clear inner clamp ring, and the beveled metallic rim.

```
                  [ 3D DISC CROSS-SECTION ]
  |<------------------ Outer Radius 1.95 ------------------>|
  | Bevel |     Printed Artwork Zone     | Clear Hub | Center Hole |
  | 0.05  |           1.25              |   0.27    |    0.38     |
  +-------+------------------------------+-----------+    (Empty)  |
  |  Rim  |   PBR Printed Front Face     | Refractiv |    Cutout   |
  | Metal |   (assets/1.webp - 6.webp)   | Plastic   |             |
  +-------+------------------------------+-----------+             |
```

---

## 2. Geometry Construction in Three.js

### Composite Mesh Architecture
Each collectible disc is constructed as a grouped composite 3D entity:

1. **Main Disc Body (`ExtrudeGeometry` or `CylinderGeometry` with shape cutout):**
   - **Outer Shape:** Circle with radius `R_outer = 1.95` units.
   - **Inner Hole Path:** Subtracted hole with radius `R_inner = 0.38` units.
   - **Thickness (Z-depth):** `0.045` units with `bevelSegments: 4`, `bevelSize: 0.008`.
   - **Tessellation:** 128 radial segments for buttery smooth curvature at close zoom.

2. **Printed Art Face (`DiscFaceMesh`):**
   - Coplanar front disc surface with UVs calibrated precisely to map the circular art files:
     - `assets/1.webp` (NOT OUT OF THIS SPACE)
     - `assets/2.webp` (ABYSSAL METROPOLIS)
     - `assets/3.webp` (ANNECY ILLUSION)
     - `assets/4.webp` (STRENGTH IN EMPATHY)
     - `assets/5.webp` (FORLORN : RODERIKA)
     - `assets/6.webp` (WHERE THE ADVENTURE ENDS)
   - Alpha channel transparency handles the central clear ring and outer rim.

3. **Inner Clear Hub Ring (`PolycarbonateHubMesh`):**
   - Annulus between `R = 0.38` and `R = 0.65`.
   - Simulates the injection-molded clear plastic clamp area of a physical CD/laserdisc.

4. **Edge Bevel & Outer Rim Ring (`MetallicRimMesh`):**
   - Annulus between `R = 1.90` and `R = 1.95`.
   - Polished mirrored aluminum foil rim that catches specular glints as the disc turns.

5. **Reverse Backing Face (`BackFaceMesh`):**
   - High-metalness brushed aluminum surface with radial micro-grooves and engraved archival serial number stamp (e.g., `AURA-ARCHIVE-EDITION-2025-N01`).

---

## 3. PBR Materials & Shaders

### A. Printed Artwork Face (`MeshPhysicalMaterial`)
- **Map:** Loaded texture from `/assets/*.webp` (sRGB color space, `anisotropy: 16`, `minFilter: LinearMipmapLinearFilter`).
- **Roughness:** `0.35` (fine satin art-print varnish).
- **Metalness:** `0.10` (organic substrate with subtle sheen).
- **Clearcoat:** `0.65` (protective lacquer gloss).
- **ClearcoatRoughness:** `0.15`.
- **Iridescence:** `0.18` with thickness range `[100nm, 400nm]` (subtle optical disc rainbow diffraction when light catches glancing angles).

### B. Polycarbonate Center Hub (`MeshPhysicalMaterial`)
- **Transmission:** `0.92` (high optical glass/acrylic clarity).
- **Opacity:** `1.0`.
- **Roughness:** `0.08` (polished smooth injection mold).
- **IOR (Index of Refraction):** `1.58` (standard polycarbonate optical index).
- **Thickness:** `0.05`.
- **Color:** `#F8FAFC`.

### C. Metallic Outer Rim & Reverse Side (`MeshStandardMaterial`)
- **Color:** `#D1D5DB` (silver aluminum).
- **Metalness:** `0.96` (true metal).
- **Roughness:** `0.18` (high specular mirror finish on outer perimeter).

---

## 4. Lighting Rig & Shadow Staging

### Lighting Configuration
1. **Key Directional Light (Warm Sun):**
   - Position: `[4.5, 6.0, 7.5]`
   - Color: `#FFFDF7` (warm daylight)
   - Intensity: `1.4`
   - Casts subtle directional drop-shadow onto an invisible shadow-catcher plane behind the discs.
2. **Fill Directional Light (Cool Sky):**
   - Position: `[-5.0, -2.5, 4.0]`
   - Color: `#E2E8F0` (soft ambient sky bounce)
   - Intensity: `0.55`
   - Prevents unlit disc areas from clipping to total black.
3. **Rim Light (Backlight Edge Accent):**
   - Position: `[1.0, 4.0, -3.5]`
   - Color: `#FFFFFF`
   - Intensity: `0.9`
   - Emphasizes the circular silhouette and bevel edge glint.
4. **Ambient Light:**
   - Intensity: `0.65`, Color: `#F1EFEA`.

### Shadow Catcher
- An invisible planar geometry located at `z = -0.8` using `ShadowMaterial` with `opacity: 0.08`.
- Creates a soft grounding shadow beneath the discs, connecting the 3D objects to the warm paper background without visual clutter.

---

## 5. Camera Configuration & Viewport Staging
- **Camera Type:** `PerspectiveCamera`
- **Field of View (FOV):**
  - Desktop (`>= 1024px`): `45.0°`
  - Mobile (`< 768px`): `58.0°` (widened to maintain 3-disc diagonal visibility)
- **Camera Position:** `[0, 0, 7.8]`
- **LookAt Target:** `[0, 0, 0]`

---

## 6. Hand-Drawn Sketch Halo (Active Indicator)
- As observed in reference `Ref-1.png` and `vid_frame_5s.jpg`, the active center disc is highlighted by a loose, organic, charcoal/pencil sketch lasso loop.
- **Implementation:** An SVG layer synchronized with the 2D projected screen coordinates of the active 3D disc mesh `discMesh.localToWorld()` $\to$ `camera.project()`.
- Rotates with gentle organic wobble (`1.2s` slow breathing loop) and dynamic stroke dasharray draw-in upon selection.
