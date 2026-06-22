# ✦ Aetheris — Style Guide

A dark-first, Dragon Quest VIII–inspired portfolio system. Night skies, warm
gold lantern light, painterly stillness. Minimal markup, fast first paint,
no heavy frameworks beyond React + Tailwind + shadcn/ui.

> **Golden rule:** components only consume **semantic tokens**
> (`hsl(var(--primary))`, `bg-card`, `text-muted-foreground`). Never hardcode
> hex values or raw utilities like `text-white`, `bg-black`, `bg-[#...]` —
> they bypass theming and break the light/dark switch.

---

## 1. Design intent

| Pillar | What it means in code |
| --- | --- |
| **Magical nostalgia** | Cinzel display type, gold accents, twinkling starfield, soft floats |
| **Minimal** | One typeface pair, ~8 tokens drive the palette, no decorative chrome |
| **Dark-first** | Night palette is the default; `.light` class on `<html>` swaps to parchment |
| **Fast first paint** | DOM/CSS animations only, no canvas, no animation libs, lazy images below the fold |

---

## 2. Color tokens

All colors are stored as **HSL triplets** (no `hsl(...)` wrapper) so Tailwind's
`hsl(var(--token) / <alpha>)` syntax works everywhere.

### 2.1 Semantic tokens — dark (default)

| Token | HSL | Use for |
| --- | --- | --- |
| `--background` | `230 45% 7%` | Page background (paired with `--gradient-night`) |
| `--foreground` | `40 30% 92%` | Default body text (parchment) |
| `--card` | `230 38% 11%` | Cards, glass panels, inputs |
| `--card-foreground` | `40 30% 92%` | Text on cards |
| `--popover` | `230 40% 9%` | Popovers, menus |
| `--primary` | `42 88% 62%` | Lantern gold — CTAs, links, focus rings |
| `--primary-foreground` | `230 50% 8%` | Text on gold buttons |
| `--secondary` | `220 55% 28%` | Twilight blue — secondary surfaces |
| `--muted` | `230 25% 16%` | Muted surfaces, subtle dividers |
| `--muted-foreground` | `40 12% 68%` | Body copy of lower importance |
| `--accent` | `28 90% 58%` | Ember orange — sparing highlights |
| `--destructive` | `0 70% 55%` | Errors, destructive actions |
| `--border` | `230 30% 18%` | Borders, hairlines |
| `--input` | `230 30% 16%` | Input strokes |
| `--ring` | `42 88% 62%` | Focus ring |

### 2.2 Custom DQ8 tokens

| Token | HSL | Use for |
| --- | --- | --- |
| `--night-deep` | `232 50% 5%` | Deepest sky (bottom of page fade) |
| `--night-mid` | `230 45% 10%` | Mid-sky / large panels |
| `--night-elev` | `230 38% 14%` | Elevated panel one step above card |
| `--gold` | `42 90% 62%` | Pure lantern accent — `.text-gold` |
| `--gold-soft` | `40 75% 72%` | Eyebrows, small captions |
| `--moon` | `48 95% 78%` | Bright highlights, glow centers |
| `--ember` | `22 90% 58%` | Warm secondary glow |
| `--twilight` | `248 45% 22%` | Aurora top, deep blue washes |

### 2.3 Light mode (`.light`)

Parchment background + burnt-orange primary; **accent flips to deep blue** so
the page reads as warm paper rather than dim dark mode.

| Token | Light value | Note |
| --- | --- | --- |
| `--background` | `38 40% 96%` | Parchment |
| `--foreground` | `232 45% 12%` | Ink |
| `--primary` | `28 85% 48%` | Burnt orange |
| `--accent` | `220 60% 35%` | Deep ink blue |
| `--border` | `38 25% 82%` | Warm hairline |

### 2.4 Gradients

| Token | Where to use |
| --- | --- |
| `--gradient-night` | `body` background (fixed attachment) — overall sky |
| `--gradient-aurora` | Hero atmospheric overlay (`.gradient-aurora`) |
| `--gradient-gold` | Primary CTAs, decorative blurs (`.gradient-gold`) |
| `--gradient-fade` | Section bottoms fading into deep night (`.gradient-fade-bottom`) |

### 2.5 Shadows

| Token | Use |
| --- | --- |
| `--shadow-glow` | Around primary CTAs and lantern halos (`.shadow-glow`) |
| `--shadow-elev` | Hero illustration, important cards (`.shadow-elev`) |
| `--shadow-card` | Default card lift (`.shadow-card`) |

### Do / Don't

```tsx
// ✅ Do
<button className="bg-primary text-primary-foreground shadow-glow" />
<div style={{ background: "hsl(var(--gold) / 0.15)" }} />

// ❌ Don't
<button className="bg-yellow-500 text-black" />
<div className="bg-[#f5c563]" />
```

---

## 3. Typography

Two families only. Imported once in `src/index.css` via Google Fonts with
`display=swap`.

| Family | Weights | Use |
| --- | --- | --- |
| **Cinzel** | 500, 600, 700 | `h1`–`h4`, `.font-display`, brand mark |
| **Inter** | 300, 400, 500, 600 | Body, UI, buttons, captions |

Body has `font-feature-settings: "ss01", "cv11"` for slightly friendlier
digits and a single-storey `a` in small sizes.

### 3.1 Type scale

| Role | Class | px (mobile → desktop) | Notes |
| --- | --- | --- | --- |
| Hero H1 | `font-display text-5xl sm:text-6xl lg:text-7xl leading-[1.05] text-balance` | 48 → 60 → 72 | One accent word in `text-gold` |
| Section H2 | `font-display text-4xl sm:text-5xl` | 36 → 48 | Always preceded by an eyebrow |
| H3 / card title | `font-display text-xl` | 20 | |
| H4 | `font-display text-lg` | 18 | |
| Lead paragraph | `text-lg leading-relaxed text-muted-foreground` | 18 | First paragraph under H1 |
| Body | `text-base` | 16 | Default |
| Small / meta | `text-sm text-muted-foreground` | 14 | Timeline, captions |
| Eyebrow | `text-xs uppercase tracking-[0.3em] text-gold-soft` | 12 | Above every section title |
| Footer / micro | `text-xs text-muted-foreground` | 12 | Footnotes, ✦ marks |

### 3.2 Special treatments

- **`text-balance`** on hero headings so line breaks land naturally.
- **Accent word**: wrap a single noun in `<span class="text-gold">…</span>`
  inside the H1 — never gild a whole heading.
- **Selection**: gold tint via `::selection` (already set globally).

---

## 4. Spacing, sizing & layout

| Concern | Value |
| --- | --- |
| Container | `max-w-6xl mx-auto px-6` (Tailwind container caps at **1400px** on `2xl`) |
| Section rhythm | `py-24` standard, `py-28` for hero/about/contact |
| Hero | `min-h-[100svh] pt-28` (svh avoids iOS URL-bar jump) |
| Radius scale | `--radius: 0.75rem` → `rounded-lg` 12, `rounded-md` 10, `rounded-sm` 8 |
| Pill radii | `rounded-full` for buttons & nav, `rounded-xl` for inputs |
| Grid patterns | Hero `lg:grid-cols-[1.1fr_1fr]`, About `lg:grid-cols-[1fr_1.2fr]`, Projects `md:grid-cols-2 lg:grid-cols-3` |

---

## 5. Breakpoints (Tailwind defaults + custom cap)

| Token | Min width | Typical use |
| --- | --- | --- |
| _(base)_ | 0 | Mobile-first defaults |
| `sm:` | 640 | Two-column forms, larger headings |
| `md:` | 768 | 2-col project grid, nav links inline |
| `lg:` | 1024 | Hero side-by-side, about asymmetric grid |
| `xl:` | 1280 | Generous gutters |
| `2xl:` | 1400 | Container cap (overrides Tailwind's 1536) |

**Rule:** write mobile styles first, then add `sm:` / `lg:` overrides.

---

## 6. Motion & effects

All motion is CSS-driven and **skipped** under `prefers-reduced-motion: reduce`.

| Animation | Class | Duration · Easing | Purpose |
| --- | --- | --- | --- |
| `twinkle` | `.animate-twinkle` | 2.5–5.5s · `ease-in-out` infinite | Stars opacity/scale pulse (random per star) |
| `float-slow` | `.animate-float-slow` | 8s · `ease-in-out` infinite | Hero illustration drift |
| `drift` | `.animate-drift` | 20s alternate | Slow horizontal sky drift (decorative layers) |
| `glow-pulse` | `.animate-glow-pulse` | 4s · `ease-in-out` infinite | Primary CTA halo |
| `rise` | `.animate-rise` | 0.9s · `--transition-magic` once | Hero copy entrance |

**Easing token:** `--transition-magic: cubic-bezier(0.22, 1, 0.36, 1)` — use
for any bespoke transition so motion feels consistently "soft landing."

### 6.1 Glass recipe (`.glass`)

```css
background: hsl(var(--card) / 0.55);
backdrop-filter: blur(14px) saturate(140%);
-webkit-backdrop-filter: blur(14px) saturate(140%);
border: 1px solid hsl(var(--border) / 0.7);
```

Use for the floating nav and any panel that should sit over the starfield.

### 6.2 Starfield tuning

| Knob | Default | Effect |
| --- | --- | --- |
| `density` prop | `90` | Total star count |
| Gold ratio | ~30% (`Math.random() > 0.7`) | Share of warm stars vs. parchment |
| Size range | `0.5–2.5px` | Star diameters |
| Twinkle duration | `2.5–5.5s` random | Asynchronous shimmer |
| Parallax factor | `-0.15` | Scroll multiplier on the layer's `translate3d` |

---

## 7. Component recipes

### Primary button
```tsx
<Button className="rounded-full gradient-gold text-primary-foreground
                   shadow-glow animate-glow-pulse">…</Button>
```

### Outline button
```tsx
<Button variant="outline"
        className="rounded-full border-primary/40 bg-transparent
                   text-foreground hover:bg-primary/10 hover:text-primary">…</Button>
```

### Skill pill / tag
```tsx
<span className="rounded-full border border-primary/20 bg-primary/5
                 px-3 py-1 text-xs text-gold-soft">TypeScript</span>
```

### Input
```tsx
<Input className="h-12 rounded-xl border-border bg-card/60" />
```

### Eyebrow + heading pair
```tsx
<p className="text-xs uppercase tracking-[0.3em] text-gold-soft">Chapter II</p>
<h2 className="mt-2 font-display text-4xl sm:text-5xl">The Wanderer</h2>
```

### Timeline dot
A `<li class="relative">` with an absolutely positioned ring containing a
solid `bg-primary shadow-glow` dot, sitting on a `border-l border-primary/20`
column.

### Floating nav
`fixed top-0`, `mt-4`, `rounded-full glass`, links use
`text-muted-foreground hover:text-primary`.

---

## 8. Iconography & imagery

- **Icons:** `lucide-react` only. Common sizes `h-3 w-3` (chips), `h-4 w-4`
  (buttons), `h-5 w-5` (nav). Default stroke 1.5.
- **Image direction:** painterly night scenes, warm lantern light, cool
  indigo shadows. Avoid photographic realism and crisp digital gradients.
- **Performance:**
  - Hero image only: `fetchPriority="high"`, explicit `width`/`height`.
  - Below-the-fold images: `loading="lazy"` + `decoding="async"`.
  - Always provide descriptive `alt`. Decorative layers: `aria-hidden="true"`.

---

## 9. Accessibility

- **Contrast:** parchment-on-night meets WCAG AA at body sizes; check any
  new color pair with a contrast tool before shipping.
- **Focus:** never remove the outline. The `--ring` token (gold) is already
  wired through shadcn — `focus-visible:ring-2 focus-visible:ring-ring`.
- **Icon buttons:** always include `aria-label`.
- **Motion:** every animation honors `prefers-reduced-motion: reduce`.
- **Landmarks:** one `<header>`, multiple `<section id="…">`, one `<footer>`,
  exactly one `<h1>` per page.

---

## 10. SEO baseline

- Single H1 with the primary keyword/phrase.
- `<title>` ≤ 60 chars, `<meta name="description">` ≤ 160 chars.
- Descriptive `alt` on every meaningful image.
- Canonical link in `index.html`.
- Optional: JSON-LD `Person` schema for the dev's name, role, and links.

---

## 11. Theming

- `.light` class on `<html>` overrides token values.
- `ThemeToggle` persists choice in `localStorage["dq8-theme"]` (`"light"` |
  `"dark"`). Default = dark.
- When adding new tokens, **always** define them in both `:root` and `.light`.

---

## 12. Voice & copy

- Short, lyrical, fantasy-tinged but never cosplay. ("Crafting small magic
  in code." · "Send a Letter" · "Chapter II — The Wanderer".)
- Sentence case for headings and buttons.
- One ✦ glyph per surface, max. It's seasoning, not a logo.
- Avoid corporate verbs: _leverage, synergize, unlock, supercharge_.

---

## 13. Contribution rules

1. **New color?** Add the HSL to `:root` and `.light` in `src/index.css`,
   then expose it in `tailwind.config.ts` under `theme.extend.colors`.
   Only then use it in a component.
2. **No raw hex** in `.tsx` files.
3. **Components stay small** — one concern per file under `src/components/`.
4. **Animations** live in `src/index.css` as `@keyframes` + `.animate-*`
   utility, with a `prefers-reduced-motion` opt-out.
5. **Images** go in `src/assets/` and are imported as ES modules so Vite
   fingerprints them.

---

_Last revised: 2026-06-22 · maintained alongside `src/index.css` and
`tailwind.config.ts`._
