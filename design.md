---
name: Carpediem AI
colors:
  primary: oklch(0.92 0.006 70)
  secondary: oklch(0.70 0.010 65)
  tertiary: oklch(0.66 0.20 238)
  highlight: oklch(0.64 0.20 65)
  neutral: oklch(0.14 0.012 248)
typography:
  h1:
    fontFamily: Prata, Georgia, serif
    fontSize: clamp(3rem, 1.8rem + 5.5vw, 6.5rem)
  body-md:
    fontFamily: Figtree, sans-serif
    fontSize: clamp(0.96rem, 0.88rem + 0.4vw, 1.08rem)
  label-caps:
    fontFamily: Figtree, sans-serif
    fontSize: clamp(0.72rem, 0.68rem + 0.2vw, 0.83rem)
rounded:
  sm: 6px
  md: 8px
  lg: 12px
  xl: 16px
  pill: 100px
spacing:
  xs: 8px
  sm: 12px
  md: 16px
  lg: 24px
  xl: 32px
  2xl: 48px
  3xl: 64px
  4xl: 96px
  5xl: 128px
---

## Overview

Human AI — Built to Last. A two-page enterprise AI site driven by 80%
tech blue energy with 20% warm amber humanity. The palette shifts from
deep cool-charcoal foundations through vibrant tech blue accents, with
muted amber/gold as a warm counterpoint reserved for interaction states.
Typography pairs Prata's serif gravitas with Figtree's clean readability.
Slow-drift background gradients, breathing glow effects, and pulse-shimmer
interactions make the surface feel alive without overwhelming.

## Page Structure

The site is split across two pages:

### Homepage (`/`)
Hero (with client logo marquee + stat badge) → What We Do → Who We Are →
AINS Framework → Services & Pricing → Case Studies → CTA → Footer

A focused 7-section single-page structure that leads with social proof
(client logos visible in hero, team section moved up) and cuts directly
to services and methodology without unnecessary detours.

### Approach Page (`/approach`)
Success Metrics → Why Carpediem AI → MENA/KSA → CTA → Footer

A secondary detail page for visitors who want depth: metrics framework,
company moat arguments, and regional expansion strategy. Linked from the
main page nav and footer.

### Supporting Palette

| Token | Value | Usage |
|---|---|---|
| `--bg-base` | oklch(0.18 0.016 245) | Secondary surfaces (footer) |
| `--bg-surface` | oklch(0.20 0.018 240) | Card backgrounds |
| `--bg-elevated` | oklch(0.24 0.020 235) | Hover/focus surfaces |
| `--bg-glass` | oklch(0.15 0.014 245 / 0.7) | Scrolled nav background |
| `--accent-muted` | oklch(0.55 0.14 238) | Borders, decorative lines |
| `--accent-subtle` | oklch(0.35 0.10 235) | Ornament rings, card hover borders |
| `--accent-surface` | oklch(0.25 0.09 238 / 0.2) | Badge backgrounds, glow effects |
| `--navy` | oklch(0.44 0.14 265) | Square ornaments, secondary accents |
| `--navy-muted` | oklch(0.45 0.10 262) | Ornament borders, secondary pill borders |
| `--border` | oklch(0.26 0.015 235) | Default borders (cards, sections, nav) |
| `--border-light` | oklch(0.30 0.020 230) | Outline button borders |
| `--text-muted` | oklch(0.60 0.012 60) | Metadata, copyright, secondary labels |

---

## Background System

### Page Level
`background-color: var(--bg-deep)` with 3 fixed radial ellipses of
cool blue tones at ~0.13 opacity, animated with a slow 35s
`driftGradient` for a subtle "alive" feel. A grain texture overlay
sits on top via SVG `<feTurbulence>` at 3.5% opacity — this gives the
entire page a tactile, matte-paper finish.

### Section Gradient Patterns
Each section has cool blue radial gradient ellipses fading in from the
edges. Defined as 2-3 `radial-gradient(ellipse ...)` layers on
`::before` or a dedicated `__bg` div.

A `mask-image` soft-fade system prevents hard edges:
```css
mask-image:
  radial-gradient(ellipse 100% 20% at 50% 0%, transparent 0%, black 100%),
  radial-gradient(ellipse 100% 20% at 50% 100%, transparent 0%, black 100%);
mask-composite: intersect;
```
This creates a vertical fade at section top and bottom.

### Section Divider
Each section has a scalloped bottom edge via:
```css
section::after {
  background: var(--bg-deep);
  mask: radial-gradient(14px at 20px -2px, black 99%, transparent 100%);
  mask-size: 40px 100%;
  mask-repeat: repeat-x;
}
```

### Texture Overlays
- **Grain**: Fixed SVG turbulence at 3.5% opacity over entire page.
- **Crosshatch diagonal**: Hero-only pattern at 4% opacity using
  `repeating-linear-gradient(45deg, ...)` for a subtle fabric-like
  texture.

### "Alive" Animations
Three continuous background animations make the site feel alive:
- **`driftGradient` (35s)**: Body background slowly drifts, simulating
  ambient light shifting across the page.
- **`breatheGlow` / `breatheGlowSoft` (7-10s)**: Hero and section glow
  layers subtly pulse in opacity and scale.
- **`accentPulse` (4s)**: Primary CTA buttons subtly pulse their glow
  shadow, drawing attention without being distracting.

---

## Ornaments & Motifs

### Circle Ornaments
Used in every section. Nested rings with `border-radius: 50%`.
```css
width: 180-240px; height: 180-240px;  /* desktop */
width: 100-140px; height: 100-140px;  /* mobile */
border: 1px solid var(--accent-subtle);
opacity: 0.12-0.15;
```
Inner rings via `::before` (20% inset, `--accent-muted`) and
optional `::after` (40% inset, `--accent-subtle`).

### Sharp Square Ornaments
Distinctive signature element. Zero border-radius, nested inner
square.
```css
width: 80-120px; height: 80-120px;
border: 1px solid var(--navy-muted);
border-radius: 0;
opacity: 0.06-0.12;
```
`::before` at 15% inset with same border. Hidden on mobile.

### Diamond Character (◆)
Used as: nav/footer logo icon, hero watermark (large, pulsing),
section labels, moat watermark (grows on hover), metrics section
divider. Always in accent blue.

### Corner Brackets (┌ └)
Decorative brackets for section corners (used in MENA section and
`.section-corner` class). In `--accent-muted` at 0.3 opacity,
monospace font.

### Section Accent Bar
Blue accent bar under section titles via `::after`.
```css
width: 3.5rem; height: 3px;
background: var(--accent);
border-radius: 2px;
```
Animates from 0 to full width on scroll reveal.

### Pill Badges & Labels
Rounded tags with border: `border-radius: 100px`, `--accent-subtle`
border, `--accent-surface` background for accent pills;
`--navy-muted` border for navy pills. Gold/highlight pills use
`--highlight-surface` bg for secondary 20% warmth accents.

---

## Animation Language

| Name | Duration | Easing | Description |
|---|---|---|---|---|
| `driftGradient` | 35s | ease-in-out | Body background gradients slowly drift across the page |
| `driftGradientSlow` | 35s | ease-in-out | Slower variant for additional background layers |
| `breatheGlow` | 7s | ease-in-out | Hero and section glow layers pulse opacity + scale |
| `breatheGlowSoft` | 7s | ease-in-out | Softer opacity-only variant for subtle backgrounds |
| `accentPulse` | 4s | ease-in-out | Primary CTA buttons subtly pulse their glow shadow |
| `shimmerSweep` | 3s | linear | Moving highlight across button surfaces on hover |
| `sectionFloat` | 24-32s | ease-in-out | Circle/square ornaments float vertically + slight rotation |
| `floatOrnament` | 18-28s | ease-in-out | Hero-specific ornament float |
| `watermarkPulse` | 12s | ease-in-out | Hero diamond watermark scales + rotates |
| `pulse` | 2.5s | ease-in-out | Badge dot opacity/scale |
| `btnGlow` | 3s | linear | Conic gradient border glow on primary buttons (hover-triggered) |
| `ctaGlowPulse` | 8-10s | ease-in-out | Large radial glow orbs scale in/out |
| `ctaSpin` | 60s | linear | CTA mandala rotates 360° |
| `scrollDown` | 2.2s | ease-in-out | Hero scroll indicator line scales from top |
| `scrollClients` | 28s | linear | Client logo marquee scroll |

### Scroll Reveal
- Initial: `opacity: 0`, `translateY(2rem)`, `scale(0.97)`
- Visible: `opacity: 1`, `translateY(0)`, `scale(1)`
- Timing: 0.9s `cubic-bezier(0.22, 1, 0.36, 1)`
- Delays: `.reveal-delay-1` through `.reveal-delay-5` (0.15-0.95s)
- Triggered by IntersectionObserver

### Reduced Motion
`@media (prefers-reduced-motion: reduce)` disables all animations
including drift gradients, breathing glows, accent pulses, ornament
floats, reveals, pulsing, and spinning. Accent bars stay at full width.

---

## Component Patterns

### Navigation
- Fixed top, transparent → `--bg-glass + backdrop-filter: blur(16px)`
  on scroll
- Height: 4.5rem → 3.75rem on scroll
- Logo: diamond icon + "Carpediem AI" in display font
- Links: secondary text, underline-on-hover accent
  - Link targets auto-adjust: relative `#section` on homepage,
    absolute `/#section` on approach page
- "Approach" link replaces "MENA/KSA" — points to `/approach`
- CTA button: primary (blue), compact padding
- Mobile: hamburger toggle → full-width dropdown at `--bg-base`

### Cards (Services, Metrics)
```css
background: var(--bg-surface);
border: 1px solid var(--border);
border-radius: var(--radius-md);
```
Hover: border → `--accent-subtle`, bg → `--bg-elevated`,
translateY(-2px). Optionally reveal a radial glow
(`--accent-surface`) behind content on hover.

### Buttons
- **Primary**: `--accent` bg, `--bg-deep` text, shadow, lift on hover
- **Outline**: transparent, `--border-light` border, accent hover
- **Ghost**: text only, underline-on-hover, arrow slide on hover
- **Large variant**: `1rem 2rem` padding for hero/CTA

### Framework Timeline
Alternating left/right layout (desktop), single-column (mobile).
Connector line gradient: navy → transparent.
Numbered circles with hover fill effect (gold bg + glow — the interaction color).

### Metrics Layout
Leading metrics: 2-column grid. Lagging: 3-column grid.
Diamond divider between groups: centered ◆ with gradient lines.
Callout card: gold left border, gradient bg.

---

## Logo & Iconography

- **Primary logo**: Diamond character (◆) in accent blue + wordmark
  "Carpediem AI" in Prata display font
- **Diamond uses**: Logo (nav/footer), watermark (hero/moat),
  section divider (metrics), section label prefix
- **Icon style**: Simple SVG arrows for buttons (arrow-right path),
  inline arrow characters for CTAs
- **No raster icons or images** are used anywhere on the site

---

## Interactive States

| Element | Normal | Hover | Transition |
|---|---|---|---|---|
| Primary button | Blue bg, shadow | Lighter blue, lift -2px, conic glow | 0.4s ease |
| Outline button | Transparent, border-light | Blue border, blue tint bg, lift | 0.4s ease |
| Ghost button | Text only | Blue color, underline, arrow slide | 0.4s ease |
| Card | Surface bg, border | Elevated bg, gold border, lift | 0.4s ease |
| Link | Secondary text | Primary text, blue underline | 0.4s ease |
| Pill | Subtle border | Surface tint bg, brighter border | 0.4s ease |
| Framework num | Circle outline | Gold fill, glow shadow, scale | 0.4s ease |

All transitions use `cubic-bezier(0.22, 1, 0.36, 1)`.

---

## Responsive Behavior

| Breakpoint | Changes |
|---|---|
| ≤1024px | Callout stamp hides |
| ≤900px | Metrics grids → 2 cols |
| ≤768px | Section padding reduced, grids → 1 col, ornaments shrink,
          square ornaments hidden, nav hamburger, hero ornaments
          hidden except primary circle, framework → single track |
| ≤640px | Metrics → 1 col, further ornament size reduction |

---

## Do's & Don'ts

**Do:**
- Use OKLCH for color — the palette relies on perceptual uniformity
- Keep backgrounds dark (`--bg-deep` through `--bg-elevated`)
- Use blue (`--accent`) as the primary static color (80%)
- Reserve gold/amber (`--highlight`) for hover/interaction states (20%)
- Use navy (`--navy`) for square ornaments and secondary decorative elements
- Nest ornament rings (circles = 20% inset, squares = 15% inset)
- Float ornaments slowly with `ease-in-out`
- Let "alive" animations (driftGradient, breatheGlow) run continuously
- Pair Prata headings with Figtree body text
- Use diamond (◆) sparingly as a signature motif
- Lead with social proof: client logos in hero, team visible early

**Don't:**
- Use pure white (`#ffffff`) — always use the cool off-white
- Apply border-radius to square ornaments (keep them sharp)
- Use gold/amber for static decorative elements — it's for interaction only
- Overuse diamond watermark — once per section max
- Place square ornaments in mobile layouts (hide them)
- Use raster images when CSS/SVG will do
- Bury social proof below the fold
