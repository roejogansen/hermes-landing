# Hermes Brand Book

*"Less, but better." — Dieter Rams*

---

## Colors

| Token | Hex | Usage |
|-------|-----|-------|
| **Background** | `#F5F5F0` | Primary page background (cream) |
| **Body bg** | `#F8F7F4` | Hermes site body (slightly warmer) |
| **Charcoal** | `#17071E` | Primary text, headings, CTAs |
| **White overlay** | `white/40` | Alternating section backgrounds |
| **White** | `#FFFFFF` | Cards, elevated surfaces |

### Opacity Scale (charcoal)
- `100%` — Primary text, headings, bold statements
- `80%` — Subheading text (hero subtitle)
- `60%` — Body text, secondary copy
- `50%` — Captions, helper text
- `40%` — Labels (comparison "Other" column), muted text
- `30%` — Faded headings (secondary part of split headlines)
- `20%` — Decorative numbers, dividers, arrows
- `10%` — Borders, divider lines
- `5%` — Subtle section borders, footer divider

---

## Typography

### Display / Headings — Roboto Mono
- **Family:** `'Roboto Mono', monospace`
- **Source:** [Google Fonts](https://fonts.google.com/specimen/Roboto+Mono)
- **Weights used:** 400 (regular), 500 (medium)
- **Letter-spacing:** `-0.05em` (always)
- **Character:** Technical, precise, confident

| Element | Size (desktop) | Size (mobile) | Weight | Line-height |
|---------|---------------|---------------|--------|-------------|
| Hero headline | 60px | 32px | 500 | 1.0 |
| Section heading | 2xl–4xl (24–36px) | 2xl (24px) | 500 | 1.0 |
| Large numbers | 5xl–6xl (48–60px) | 5xl (48px) | 500 | — |

### Body / UI — Kantumruy
- **Family:** `'Kantumruy', sans-serif`
- **Source:** [Google Fonts](https://fonts.google.com/specimen/Kantumruy)
- **Weights used:** 400 (regular), 700 (bold)
- **Letter-spacing:** `-0.05em` (on buttons and subheadings), default elsewhere
- **Character:** Clean, warm, readable

| Element | Size (desktop) | Size (mobile) | Weight |
|---------|---------------|---------------|--------|
| Subheading | 24px | 16–18px | 400 |
| Body text | 16–18px | 16px | 400 |
| Body emphasis | 16–18px | 16px | 700 (bold) or medium |
| Labels (uppercase) | 14px (sm) | 14px | 700 |
| Captions | 14px (sm) | 14px | 400 |
| Button text | 18–20px (lg–xl) | 18px | 700 |

---

## Letter-Spacing Reference

| Context | Value |
|---------|-------|
| All Roboto Mono headings | `-0.05em` |
| Subheading text (Kantumruy) | `-0.05em` |
| Button text | `-0.05em` |
| Uppercase labels | `tracking-wide` (Tailwind default: 0.025em) |
| Body text | default (0) |

---

## Spacing

| Pattern | Value |
|---------|-------|
| Page horizontal padding | `px-6` mobile / `px-10` or `px-12` desktop |
| Section vertical padding | `py-16` mobile / `py-20` desktop |
| Max content width | `max-w-4xl` (896px) for text sections |
| Max content width (wide) | `max-w-5xl` (1024px) or `max-w-7xl` (1280px) |
| Heading to content gap | `mb-12` mobile / `mb-16` desktop |
| Grid gap (comparison) | `gap-4` mobile / `gap-6` desktop |
| Comparison row spacing | `space-y-4` |
| Label to content gap | `mb-4` |

---

## Components

### CTA Button
```
Background: #000000 (black) or #17071E (charcoal)
Text: white
Font: Kantumruy, bold, lg–xl
Letter-spacing: -0.05em
Padding: 0.75rem 1.5rem (standard) or px-20 (hero)
Border-radius: rounded-full (pill)
Hover: bg-gray-900, translateY(-0.5px)
Arrow icon: 20×20, stroke-width 2.5, ml-2
```

### Comparison Table
```
Layout: 2-column grid, centered
Left column: muted text (charcoal/60), label (charcoal/40, uppercase, bold, sm)
Right column: strong text (charcoal, font-medium), label (charcoal, uppercase, bold, sm)
Divider: border-l border-charcoal/10, pl-4 lg:pl-6
Row spacing: space-y-4
```

### Section Heading (split tone)
```
Primary line: full charcoal
Secondary line: charcoal/30
Font: Roboto Mono, medium or black (depending on context)
Letter-spacing: -0.05em
```

### Cards
```
Background: white
Border: border-charcoal/10 or /15
Border-radius: rounded-2xl
Padding: p-8
Hover: translateY(-2px), subtle shadow
```

---

## Principles

1. **Less, but better** — Every element earns its place
2. **Quiet confidence** — No gradients, no glow, no noise
3. **Typography does the work** — Size and weight create hierarchy, not color
4. **Opacity creates depth** — One color (charcoal) at varying opacities
5. **Whitespace is design** — Generous padding, no cramming
6. **Honest** — No stock photos, no fake testimonials, no filler bullets

---

## File Inventory

| Asset | Path | Notes |
|-------|------|-------|
| Logo (SVG) | `/gymfluence-logo.svg` | Gymfluence wordmark |
| Hermes Logo | `/hermes/hfm-logo.svg` | Hermes wordmark |
| Hero video | `/gymfluence-hero.mp4` | Gym/fitness footage |
| Favicon | `/favicon.ico` | Standard |
| Apple touch | `/apple-touch-icon.png` | 180×180 |
| Favicon 32 | `/favicon-32x32.png` | 32×32 |
| Favicon 16 | `/favicon-16x16.png` | 16×16 |

---

*Last updated: 2026-03-13*
