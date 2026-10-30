# Vesper.ai — Operational AI Infrastructure

Recreate the high-fidelity, single-viewport dark landing page for **Vesper.ai** with liquid-metal aesthetic styling, choreographed entrance keyframes, responsive viewport containment, and zero flash-of-white loading guarantee.

## User Review & Critical Decisions

> [!IMPORTANT]
> The architectural direction and user-confirmed parameters are locked:
> - **Hero Background Visual**: Confirmed to use the provided CloudFront MP4 looping video within `.hero-photo` with overlay scrim and fallback handling.
> - **Navigation & Action Anchors**: Standard in-page hash links matching the exact specifications (`#top`, `#benefits`, `#how-it-works`, `#faqs`, `#pricing`, `#start`, `#demo`).
> - **Single Viewport & Zero-Flash Delivery**: Direct inline styling and CSS rules prioritizing immediate `#000000` paint, complete responsive breakpoints (from 2560px down to 320px mobile drawer), and fail-safe entrance motion.

---

### 1. Overview & Core Concept

- **What It Does**: A showcase single-viewport landing page presenting Vesper.ai's operational AI platform. Highlights automated workflows, reduction in manual operations, and enterprise team adoption through a cinematic dark theme.
- **Target Audience**: Enterprise operations leaders, CTOs, automation engineers, and workflow architects looking for adaptive AI agent infrastructure.
- **Key Value**: Immediate high-impact immersion into the Vesper brand with liquid-metal glass controls, bottom-anchored hero composition, and responsive multi-tier typography.

---

### 2. User Experience & Visual Design

#### Visual Identity & Theme
- **Canvas & Neutrals**: Pure black `#000000` base canvas (`--bg: #000000`) with no white flashes. Structural borders using `rgba(255, 255, 255, 0.16)` and muted text in `#9a9a9a` / `#d8d8d8`.
- **Typography Pairing**:
  - Primary UI, logo, navigation pills, buttons, badge, lede, and stats: `"Inter", system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`.
  - Hero accent phrase (*AI agents*): `"Instrument Serif", "Times New Roman", Times, serif` in italic 400 with distinctive `#9a9a9a` styling.
  - Variable font web font loading with Google Fonts fallback.
- **Liquid-Metal Controls**:
  - Navigation pills: Metallic gradient (`linear-gradient(105deg, #050505 0%, #2a2a2a 48%, #4a4a4a 100%)`) with metallic border and active shine sweep on hover.
  - Solid buttons: Liquid silver gradient (`#ffffff` to `#cfcfcf`) with crisp inset highlight and ambient glow on hover.
  - Ghost buttons: Glass frosted backdrops with 16px blur, subtle rim light, and blue-tinted hover illumination.

#### Layer Stack & Hierarchy
```
┌────────────────────────────────────────────────────────┐
│ .grain (z-index: 100, pointer-events: none)            │
├────────────────────────────────────────────────────────┤
│ .page (z-index: 1, min-h: 100dvh, grid: auto 1fr auto) │
│   ├─ .menu-backdrop (mobile blur overlay)              │
│   ├─ header.header (3-column grid: Logo / Nav / CTA)   │
│   ├─ main.hero#top (bottom-aligned hero stack)         │
│   └─ footer.stats (3 quantitative operational metrics) │
├────────────────────────────────────────────────────────┤
│ .hero-photo + ::after scrim (background visual layer)   │
├────────────────────────────────────────────────────────┤
│ html / body (pure black #000000)                       │
└────────────────────────────────────────────────────────┘
```

#### Entrance Motion Choreography
- Base resting opacity is `1` for all elements (zero blank screen if JS or CSS fails).
- Keyframe entrance classes (`appear--scale`, `appear--soft`, `appear--mask`, `appear--pop`, `appear--btn`, `appear--side`, `appear--stat`) mapped to staggered delays (`--d` from `0.08s` to `1.44s`).
- Self-cleaning `animationend` handlers attach `.is-in` class; fail-safe two-frame rAF watchdog detects any stalled animations and forces completion.

---

### 3. Key Product Decisions & Trade-Offs

- **Decision 1: Direct Single-File Entry Integration**
  - *Chosen Approach*: Deliver the complete experience directly in `index.html` with synchronized React mounting in `src/App.tsx`, ensuring both raw static viewing and Vite preview execute identically.
  - *Why*: Satisfies the prompt requirement ("One HTML file with inline CSS and a small IIFE") while preserving full Vite dev server and build pipeline compatibility.
- **Decision 2: Video Background Architecture**
  - *Chosen Approach*: Deliver `.hero-photo` with an HTML5 `<video>` element configured with `autoplay muted loop playsinline` sourcing the specified CloudFront MP4, overlayed with the specified scrim and CSS gradient fallback.
  - *Why*: Ensures seamless playback across Safari, Chrome, and Firefox while gracefully handling autoplay restrictions or network drops.
- **Decision 3: Mobile Viewport & Menu Orchestration**
  - *Chosen Approach*: Full-screen mobile backdrop with 24px backdrop blur, vertical navigation transformation, hamburger morph animation, and safe-area inset calculations.

---

### 4. Technical Architecture & Component Mapping

```
[ Browser / Viewport ]
       │
       ▼
┌───────────────────────────────────────────────────────────┐
│ index.html (Pure black inline styles, preconnects, fonts) │
│                                                           │
│  ├── <link rel="icon"> (SVG Data URI)                     │
│  ├── <style> (Tokens, Universal Reset, Liquid Styles,     │
│  │            Keyframes, Responsive Media Queries)        │
│  │                                                        │
│  ├── .hero-photo (<video autoplay loop muted playsinline>)│
│  │                                                        │
│  ├── .page                                                │
│  │    ├── .menu-backdrop                                  │
│  │    ├── header.header                                   │
│  │    │     ├── a.logo (SVG mark + Vesper.ai)             │
│  │    │     ├── nav#site-nav (4 liquid-metal links)       │
│  │    │     ├── a.btn.btn-solid ("Start for Free")        │
│  │    │     └── button.burger (3-bar mobile toggle)       │
│  │    │                                                   │
│  │    ├── main.hero#top                                   │
│  │    │     ├── .badge (Operational AI Infrastructure)    │
│  │    │     ├── h1 (Two masked lines with <em>AI agents</em>)
│  │    │     ├── p.lede (Platform value narrative)         │
│  │    │     └── .hero-actions (Solid + Ghost CTAs)        │
│  │    │                                                   │
│  │    └── footer.stats                                    │
│  │          ├── Stat 1: 4.2M+ workflows automated         │
│  │          ├── Stat 2: 92% reduction in manual ops       │
│  │          └── Stat 3: 180+ operational teams onboarded  │
│  │                                                        │
│  └── <script> (Menu controller IIFE + Animation Watchdog) │
└───────────────────────────────────────────────────────────┘
```

#### Verification Plan
- Verify immediate black background with zero white flash during DOM initialization.
- Test responsive breakpoints (desktop lock $\ge$ 901px, compact heights 850px & 720px, mobile layout $\le$ 900px, narrow screens $\le$ 560px).
- Verify mobile hamburger menu toggle, backdrop blur, Esc key handling, and auto-close on resize.
- Confirm compilation with `npm run build` via `compile_applet`.
