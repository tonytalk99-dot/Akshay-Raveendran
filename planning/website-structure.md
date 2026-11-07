# Website Structure & Viewport Architecture

## 1. Architectural Philosophy & Reference Fidelity
Inspired directly by the Ravi Klaassens archive (`a24.raviklaassens.com`), the website is conceived not as a conventional vertical web page, but as a **single-viewport editorial gallery stage**. A full-bleed WebGL 3D canvas hosts physical collectible art discs drifting along a sweeping diagonal spatial curve, orchestrated by virtual scroll and mouse drag. Overlaid DOM elements form a minimalist, magazine-grade typographic HUD with generous negative space ("white space"), subtle dividers, and reactive metadata.

---

## 2. Global Viewport Layout & Z-Index Layering

```
+-----------------------------------------------------------------------------------+
| [TOP LEFT]                                [CENTER TOP]               [TOP RIGHT]  |
| ACTIVE ARTWORK TITLE (Large Display Serif)  BRAND LOGO (AURA ARCHIVE)  INDEX [6 ^]  |
| -----------------------------------------   Category Pills:                       |
| METADATA TABLE:                             • ALL EDITIONS (Active)               |
| DIRECTED / ARTIST BY  |  Shakib Siddiqui    • GENESIS DISCS                       |
| YEAR                  |  2025               • COLLABORATIONS                      |
| EDITION SPEC          |  1 of 25 (Fictional)                                      |
| MEDIUM                |  Physical Chrome Disc                                     |
|                                                                                   |
|                                                                                   |
|                           [WEBGL 3D SPATIAL STAGE]                               |
|                  Diagonal Curved Trajectory of Physical Discs                     |
|                                                                                   |
|        (Disc N-1)           (ACTIVE DISC N)                 (Disc N+1)            |
|       Receding Left         Center Stage Focus              Rising Top-Right      |
|       Smaller / Angled      Facing Front + Sketch Ring      Tilted / Ascending    |
|                                                                                   |
|                                                                                   |
|                                 [BOTTOM ROW]                                      |
|                     CURATOR CRITIQUE 1        CURATOR CRITIQUE 2    [COLLECT CTA] |
|                     ★★★★★                    ★★★★★                 [ACQUIRE DISC]|
|                     "TRANSCENDENT SPATIAL     "A MASTERCLASS IN                   |
|                      SURREALISM" — ARTFORUM    SURFACE TEXTURE"                   |
+-----------------------------------------------------------------------------------+
```

### Stacking Hierarchy
1. **Layer 0 (`z-index: 0`):** `CanvasBackground` — Warm archival off-white canvas `#F5F3EF` with subtle film-grain texture.
2. **Layer 10 (`z-index: 10`):** `ThreeCanvas` — Three.js WebGL viewport rendering camera, lighting, and the 6 collectible 3D art discs with real-time anisotropic reflections and dynamic drop shadows.
3. **Layer 20 (`z-index: 20`):** `SelectionHaloLayer` — SVG hand-drawn sketchy contour ring encircling the active center disc, tracking its 2D projected screen coordinates.
4. **Layer 30 (`z-index: 30`):** `ViewportHUD` — Fixed DOM overlay containing:
   - Header (Brand, Navigation, Category Switcher, Index Toggle)
   - Left Metadata Rail (Dynamic active item title, artist, year, medium, edition)
   - Bottom Reviews & Press Quotes (5-star ratings, publication, quote text)
   - Bottom Right Action Widget (Acquire / Detail Inspection trigger)
5. **Layer 40 (`z-index: 40`):** `IndexDrawerModal` — Expandable dropdown archive sheet triggered from the header index toggle (`Index 6 ^`).
6. **Layer 50 (`z-index: 50`):** `DiscDetailOverlay` — Editorial deep-dive overlay triggered on disc click (full narrative, high-res texture zoom, fictional certificate of authenticity, edition specs).

---

## 3. Structural Sections

### Section A: Editorial Header
- **Top Center Brand Logo:** `AURA ARCHIVE` (or minimalist monogram), matching the iconic central positioning of `A24` in the reference.
- **Category Filter Tabs:**
  - `All Works` (with active dot indicator underneath)
  - `Surrealism`
  - `Dark Fantasy`
- **Archive Index Trigger:** `Index 6 v` (or dynamic count badge `6 ^` when open). Clicking toggles the instant slide-out list.

### Section B: Active Item Title & Metadata Ledger (Top-Left)
- **Title Block:** Prominent, high-contrast serif typeface (`40px–52px`). Smoothly transitions between artworks via split-text / fade-slide animation when scroll milestones are crossed.
- **Ledger Grid:** Minimalistic tabular structure with `1px` razor-thin borders (`#E2DFD7`):
  - `ARTIST / CREATOR`: shakib.siddiqui / dreamina / studio aura
  - `YEAR`: 2024–2026
  - `EDITION`: Ultra-Limited (1 of 25 / 1 of 50) *(Fictional commerce label)*
  - `MEDIUM`: 12-Inch Precision-Milled Aluminum Composite Disc with Holographic UV Finish

### Section C: 3D Disc Diagonal River (Center Stage)
- An infinite-feeling curved trajectory traversing diagonally across the canvas from lower-left to upper-right.
- The active disc sits squarely in the optical center-left foreground, flanked by the preceding disc receding behind to the left and subsequent disc ascending into the top right.
- Hand-drawn sketch circle / halo loops around the active item to echo the iconic Ravi Klaassens design accent.

### Section D: Curatorial Endorsements & Press Quotes (Bottom)
- Two columns of stylized press reviews with 5-star rating glyphs (`★★★★★`), source label in tracking-spaced uppercase sans-serif (`COLLIDER`, `ARTFORUM`, `THE DISCERNER`), and memorable quotation in classic serif quotes.

### Section E: Floating Fast Navigation / Scroll Barometer
- Minimal indicator showing progress `01 / 06`, drag prompt ("Drag or Scroll to Traverse"), and keyboard arrow support (`←` / `→`).

---

## 4. Modal Overlays & Drawers

### 1. Index Drawer (`IndexDrawer`)
- Positioned directly beneath the header index badge.
- Clean floating white card with subtle drop shadow and hairline border.
- Lists all 6 available discs with item index, title, artist, and year.
- Active disc highlighted with inverted black badge (`[01] Not Out Of This Space  2025`).
- Clicking an item triggers an animated spline interpolation directly to that disc's scroll progress.

### 2. Product Detail & Acquisition Sheet (`DiscDetailModal`)
- Triggered by clicking the active disc or the "Acquire Disc" CTA.
- Smooth camera zoom into the disc face; DOM text glides in with high-density editorial storytelling, material specifications, artist statement, and fictional acquisition options (USD & ETH pricing, fictional smart contract address, archival packaging specs).
