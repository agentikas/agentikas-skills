---
name: design-system
description: Visual identity, UX/UI tokens, and component specifications for all Agentikas products. Light-first warm paper theme with Playfair Display + DM Sans typography and Agentikas red accents. Apply together with BRAND.md (voice) and geo-html-metadata.md (semantic HTML + a11y).
license: MIT
---

# Skill: Agentikas Design System

This is the definitive visual identity and UX/UI reference for all Agentikas products.
Every page, component, and project built under Agentikas MUST follow these rules.

**Apply this skill alongside:**
- `BRAND.md` at the repo root (voice, tone, terminology, values)
- `skills/geo-html-metadata.md` (semantic HTML, schema.org, a11y baseline — this skill
  assumes everything in geo-html-metadata.md is already applied)

---

## 1. Design Philosophy

Agentikas visual identity sits at the intersection of **editorial elegance** and
**technical credibility**. Think: a research lab that publishes beautifully.

**Core principles:**
- **Clarity over decoration** — every element earns its place
- **Light-first, warm paper** — warm cream background (`#F7F5F0`) is the signature
- **Typography-driven** — the text IS the design, not an afterthought
- **Quiet confidence** — no flashy animations, no gimmicks, no noise
- **Accessible by default** — WCAG 2.2 AA minimum, always (see checklist in §10)

**Inspirations:** Stripe Press (editorial quality on light paper), Linear (density and
layout discipline), Ghost (reading experience), Vercel (technical polish).

---

## 2. Color System

### Design tokens — always use CSS custom properties, never hex literals

All Agentikas surfaces MUST declare these tokens at the `:root` level of their global
stylesheet. Components then reference them by name (`var(--color-bg)`), never by value
(`#F7F5F0`). This is what enables the "Opción A / B" theming path from the polish roadmap
without refactoring components later.

```css
:root {
  /* ── Backgrounds ─────────────────────────────── */
  --color-bg:          #F7F5F0;   /* Warm paper — page background */
  --color-surface:     #FFFFFF;   /* Cards, panels, elevated content */
  --color-surface-alt: #F0EDE8;   /* Inline code, hover shade, soft badges */

  /* ── Text ────────────────────────────────────── */
  --color-text:        #0D0D0D;   /* Near-black — primary content */
  --color-text-muted:  #6B6B6B;   /* Secondary text, metadata */
  --color-text-soft:   #999999;   /* Dates, captions (AA on paper, not AAA) */
  --color-text-invert: #FFFFFF;   /* Text on dark surfaces */

  /* ── Brand ───────────────────────────────────── */
  --color-primary:         #C8352A;              /* Agentikas red — the only accent */
  --color-primary-hover:   #A52A22;              /* Hover state, AA on paper */
  --color-primary-soft:    rgba(200, 53, 42, 0.10); /* Badge backgrounds */

  /* ── Borders ─────────────────────────────────── */
  --color-border:       #D8D4CC;  /* Subtle dividers on paper */
  --color-border-hover: #BBB5AA;  /* Hover on cards */

  /* ── Functional ──────────────────────────────── */
  --color-success: #2D8A4E;
  --color-warning: #D4A017;
  --color-error:   #C8352A;       /* Intentionally same as primary */

  /* ── Focus (keyboard users) ──────────────────── */
  --focus-ring: 0 0 0 2px #F7F5F0, 0 0 0 4px #C8352A;
  --focus-offset: 2px;
}
```

### Rules
- **Never use pure black (`#000`) or pure white (`#FFF`) as text/background** — always the tokens above
- **Red (`#C8352A`) is the ONLY accent color** — no blues, greens, or purples as accents
- **Borders over shadows** — Agentikas does not use `box-shadow`, ever. State changes are expressed through border color, never elevation
- **Text hierarchy through weight and size**, not color variety
- **Muted text (`--color-text-muted`) gives ~4.8:1 contrast on paper** — fine for body copy, avoid for anything <14px
- **Soft text (`--color-text-soft`) is ~4.5:1 and is borderline** — only use it for non-essential metadata (dates, captions). Never for action labels, error states, or any text the user must read to succeed

### Future: per-blog theming (deferred)

User blogs (`slug.blog.agentikas.ai`) ship with the Agentikas defaults above by default.
The `blogs.brand_config` JSONB column exists in the schema but is not read yet — per-blog
theming is a deferred sprint (see §11 for the migration path). Until then, all user blogs
render in Agentikas warm paper, which is the best-quality default we can offer.

---

## 3. Typography

### Font stack — always declared on `:root`

```css
:root {
  /* Headings — editorial, confident */
  --font-heading: "Playfair Display", Georgia, "Times New Roman", serif;

  /* Body — clean, technical, readable */
  --font-body: "DM Sans", -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;

  /* Code — monospace for technical content */
  --font-mono: "JetBrains Mono", "Fira Code", "SF Mono", Consolas, monospace;
}

html, body { font-family: var(--font-body); }
h1, h2, h3, h4, h5, h6 { font-family: var(--font-heading); }
code, pre, kbd, samp { font-family: var(--font-mono); }
```

**In Next.js** (App Router), load both web fonts via `next/font/google` in the root
`layout.tsx` so they're inlined server-side and never block LCP. Never rely on system
font fallbacks at runtime for brand-critical type.

### Scale

| Token | Size | Weight | Line-height | Use |
|---|---|---|---|---|
| `--text-hero`    | 48px | 700 | 1.15 | Landing page hero only |
| `--text-h1`      | 36px | 700 | 1.15 | Page titles |
| `--text-h2`      | 28px | 700 | 1.25 | Section headings |
| `--text-h3`      | 20px | 600 | 1.35 | Subsection headings |
| `--text-body`    | 17px | 400 | 1.7  | Body text, paragraphs |
| `--text-body-sm` | 15px | 400 | 1.6  | Card descriptions |
| `--text-small`   | 13px | 500 | 1.5  | Metadata, dates, labels |
| `--text-badge`   | 11px | 600 | 1.4  | UPPERCASE labels, pill badges |

### Rules
- **Never use more than 3 weights** on a single page (400, 500, 700)
- **Body text minimum is 16px** on any device — never reduce below
- **Line height for body is 1.7** minimum — generous breathing room
- **Labels and badges are UPPERCASE** with `letter-spacing: 1.5px` — a signature Agentikas detail
- **`<em>` in headings uses red color, not italic** — another Agentikas signature

```css
h1 em, h2 em, h3 em { color: var(--color-primary); font-style: normal; }
```

---

## 4. Layout

### Grid and spacing tokens

```css
:root {
  /* Max content widths */
  --w-reading: 720px;    /* Blog posts, articles */
  --w-page:    1100px;   /* Landing sections, dashboards */
  --w-wide:    1400px;   /* Full-width sections, hero */

  /* Spacing scale (8px base) */
  --space-1:  4px;
  --space-2:  8px;
  --space-3:  12px;
  --space-4:  16px;
  --space-5:  24px;
  --space-6:  32px;
  --space-7:  48px;
  --space-8:  64px;
  --space-9:  96px;
  --space-10: 128px;
}
```

### Page structure

```
┌─── Nav (fixed, 56px height, translucent paper backdrop) ─┐
│  Logo          Links                      EN|ES   CTA    │
├───────────────────────────────────────────────────────────┤
│                                                           │
│  <main id="main-content"> (centered, max-width applied)   │
│  padding: 0 32px (desktop), 0 20px (mobile)               │
│                                                           │
├───────────────────────────────────────────────────────────┤
│  Footer (subtle, text-muted, small copy)                  │
└───────────────────────────────────────────────────────────┘
```

A **"Skip to main content"** link MUST be the first focusable element of every page and
target `#main-content`. See `geo-html-metadata.md` §5 for the pattern.

### Section spacing
- Between major sections: `96px` (desktop), `64px` (mobile)
- Between heading and content: `32px`
- Between cards in a grid: use `16px` (see below — do **not** use the old "1px gap" pattern)

### Card grid

```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
}

/* Responsive */
@media (max-width: 1024px) { .card-grid { grid-template-columns: repeat(2, 1fr); } }
@media (max-width: 768px)  { .card-grid { grid-template-columns: 1fr; } }
```

**Change from v1 of this skill**: the old "1px gap" pattern (cards touching with a
hairline border) worked on dark theme where the border was the separator. On warm paper,
it looks cramped and cards become indistinguishable. Use `16px` gap and let each card
breathe.

---

## 5. Components

### Nav

```css
.nav {
  position: fixed;
  top: 0;
  height: 56px;
  background: rgba(247, 245, 240, 0.85);  /* paper with alpha */
  backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--color-border);
  z-index: 100;
}
```

- Logo left: "Agenti**kas**" (kas in red), Playfair Display, 22px
- Links: DM Sans, 15px, 500 weight, `--color-text-muted`, hover: `--color-text`
- CTA button: solid `--color-primary` background, `--color-text-invert` text, 14px
- Language switcher: "EN | ES", muted, active is `--color-text` bold
- **Wrap in `<nav aria-label="Main navigation">`** (required, see geo-html-metadata.md §2)
- **Active link uses `aria-current="page"`**, not only a CSS class

### Buttons

```css
/* Primary — red, for main CTAs */
.btn-primary {
  background: var(--color-primary);
  color: var(--color-text-invert);
  padding: 10px 24px;
  border-radius: 4px;
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 500;
  border: none;
  cursor: pointer;
  transition: background-color 0.2s;
}
.btn-primary:hover  { background: var(--color-primary-hover); }
.btn-primary:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}
.btn-primary:disabled { opacity: 0.4; cursor: not-allowed; }

/* Ghost — outline, for secondary actions */
.btn-ghost {
  background: transparent;
  color: var(--color-text-muted);
  border: 1px solid var(--color-border);
  padding: 10px 24px;
  border-radius: 4px;
  font-size: 14px;
  font-weight: 500;
  transition: border-color 0.2s, color 0.2s;
}
.btn-ghost:hover {
  border-color: var(--color-border-hover);
  color: var(--color-text);
}
.btn-ghost:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}
```

- **Border radius: 4px** everywhere — no rounded-full, no sharp corners
- **Never use shadows on buttons** — only border and background changes
- **`:focus-visible` is mandatory** — the outline must be clearly visible on warm paper

### Cards

```css
.card {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: 8px;
  padding: 24px;
  transition: border-color 0.2s;
}
.card:hover {
  border-color: var(--color-border-hover);
}
.card a:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
  border-radius: 4px;
}
```

- **Border radius: 8px** on cards (softer than buttons — a subtle hierarchy signal)
- **No box shadows** — border-color change is the only hover affordance
- **If the entire card is a link**, the focus outline goes on the `<a>` itself, not the card

### Badges / Pills

```css
/* Brand badge (Agentikas Labs posts) */
.badge-labs {
  background: var(--color-primary-soft);
  color: var(--color-primary);
  padding: 4px 10px;
  border-radius: 4px;
  font-size: 12px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1.5px;
}

/* Neutral badge (tags, categories) */
.badge {
  background: var(--color-surface-alt);
  color: var(--color-text-muted);
  padding: 4px 10px;
  border-radius: 4px;
  font-size: 12px;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 1.5px;
}
```

**Change from v1**: minimum badge size is `12px`, not `10px` or `11px`. Anything smaller
fails WCAG 1.4.4 for users who can't zoom.

### Section labels

```css
.section-label {
  color: var(--color-primary);
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  margin-bottom: 16px;
  display: block;
}
```

### Forms

```css
.field {
  display: flex;
  flex-direction: column;
  gap: 6px;
  margin-bottom: 20px;
}
.field label {
  font-size: 14px;
  font-weight: 500;
  color: var(--color-text);
}
.field input, .field textarea, .field select {
  font-family: var(--font-body);
  font-size: 15px;
  padding: 10px 14px;
  border: 1px solid var(--color-border);
  border-radius: 4px;
  background: var(--color-surface);
  color: var(--color-text);
  transition: border-color 0.2s;
}
.field input:hover { border-color: var(--color-border-hover); }
.field input:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
  border-color: var(--color-primary);
}
.field [aria-invalid="true"] {
  border-color: var(--color-error);
}
.field .error-text {
  font-size: 13px;
  color: var(--color-error);
}
```

**Accessibility requirements** (non-negotiable — see `geo-html-metadata.md` §2.5):
- Every `<input>` MUST have an associated `<label htmlFor="...">`
- Error messages MUST use `role="alert" aria-live="polite"`
- Required fields MUST have `aria-required="true"` and visible `*` in the label
- Invalid fields MUST have `aria-invalid="true"` and `aria-describedby` linking to the error

---

## 6. Blog-Specific Patterns

### Article page (reading experience)

```css
.article {
  max-width: var(--w-reading);
  margin: 0 auto;
  padding: 96px 24px 64px;
}

.article h1 {
  font-family: var(--font-heading);
  font-size: 36px;
  font-weight: 700;
  line-height: 1.15;
  color: var(--color-text);
  margin-bottom: 12px;
}

.article-subtitle {
  font-size: 19px;
  color: var(--color-text-muted);
  line-height: 1.5;
  margin-bottom: 24px;
}

.article-meta {
  font-size: 13px;
  color: var(--color-text-soft);
  padding-bottom: 40px;
  margin-bottom: 40px;
  border-bottom: 1px solid var(--color-border);
}

.prose {
  font-size: 17px;
  line-height: 1.8;
  color: #333;
}

.prose h2 {
  font-family: var(--font-heading);
  font-size: 26px;
  font-weight: 700;
  color: var(--color-text);
  margin-top: 48px;
  margin-bottom: 16px;
}

.prose h3 {
  font-family: var(--font-heading);
  font-size: 20px;
  font-weight: 600;
  color: var(--color-text);
  margin-top: 36px;
  margin-bottom: 12px;
}

.prose a {
  color: var(--color-primary);
  text-decoration: underline;
  text-underline-offset: 3px;
}
.prose a:hover { color: var(--color-primary-hover); }

.prose code {
  font-family: var(--font-mono);
  background: var(--color-surface-alt);
  padding: 2px 6px;
  border-radius: 3px;
  font-size: 15px;
}

.prose pre {
  background: #0D0D0D;
  color: #e4e5e9;
  border-radius: 8px;
  padding: 20px 24px;
  overflow-x: auto;
  margin: 24px 0;
}

.prose img {
  border-radius: 8px;
  margin: 32px 0;
  max-width: 100%;
}

.prose blockquote {
  border-left: 3px solid var(--color-primary);
  padding-left: 20px;
  margin: 24px 0;
  color: var(--color-text-muted);
  font-style: italic;
}
```

### Post cards (home grid)

```css
.post-card {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: 8px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: border-color 0.2s;
}

.post-card:hover { border-color: var(--color-border-hover); }

.post-card img {
  width: 100%;
  height: 180px;
  object-fit: cover;
}

.post-card-body { padding: 20px; display: flex; flex-direction: column; gap: 8px; }

.post-card-title {
  font-family: var(--font-heading);
  font-size: 17px;
  font-weight: 700;
  color: var(--color-text);
}

.post-card-desc {
  font-size: 14px;
  color: var(--color-text-muted);
  line-height: 1.5;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.post-card-meta {
  font-size: 12px;
  color: var(--color-text-soft);
  margin-top: auto;
}
```

---

## 7. Motion and Transitions

- **Transitions: 0.2s ease** for color, background, border, opacity
- **No bounce, no spring, no elastic** — everything is subtle and fast
- **No page transitions** — instant navigation
- **Hover effects: border-color change or opacity shift**, never `transform: scale`
- **Respect `prefers-reduced-motion`** — gate any animation:

```css
@media (prefers-reduced-motion: no-preference) {
  .reveal { animation: reveal 0.5s ease forwards; }
}
@keyframes reveal {
  from { opacity: 0; transform: translateY(12px); }
  to   { opacity: 1; transform: translateY(0); }
}
```

---

## 8. Responsive Breakpoints

```css
/* Mobile first */
@media (min-width: 640px)  { /* sm — small tablets */ }
@media (min-width: 768px)  { /* md — tablets */ }
@media (min-width: 1024px) { /* lg — desktop */ }
@media (min-width: 1280px) { /* xl — wide desktop */ }
```

### Key responsive rules
- **Nav collapses to hamburger** at `768px`
- **Card grids go to single column** at `768px`
- **Section padding reduces** from `32px` to `20px` horizontal
- **Hero text scales down** from `48px` to `32px`
- **Body text stays 17px** — never reduce below 16px on mobile
- **Touch targets minimum 44x44px** (WCAG 2.5.5)

---

## 9. Do and Don't

### DO
- Use Playfair Display for headings, DM Sans for body
- Use red (`#C8352A`) as the only accent color
- Reference tokens via `var(--color-*)`, never hex literals
- Use warm paper (`#F7F5F0`) as the base background
- Use 16px gap in card grids, 8px border-radius on cards
- Use 4px border-radius on buttons and badges
- Use subtle borders, never shadows
- Use generous line-height (1.7+ for body)
- Use uppercase + letter-spacing for labels and badges
- Use `:focus-visible` with `outline: 2px solid var(--color-primary)` on all interactive elements
- Use `aria-current="page"` on active nav links
- Wrap page content in `<main id="main-content">` and provide a skip link
- Respect `prefers-reduced-motion`

### DON'T
- Don't use colors other than red as accent (no blue buttons, no green badges)
- Don't use `box-shadow` anywhere
- Don't use hex literals inside component CSS (use tokens)
- Don't use `border-radius` >8px on anything
- Don't use bold colors for backgrounds (no red backgrounds except `--color-primary-soft`)
- Don't use decorative icons or illustrations
- Don't use more than 3 font weights per page
- Don't use animations that move elements more than 12px
- Don't use `outline: none` without a `:focus-visible` replacement
- Don't use `<div onclick>` — always `<button>` or `<a>`
- Don't rely on color alone to convey state (add text, icon, or underline)
- Don't use font-size <12px for any text the user must read
- Don't reduce body text below 16px on any device
- Don't use pure black (`#000`) or pure white (`#FFF`) as text/background

---

## 10. Implementation Checklist

Before shipping any Agentikas page:

**Brand foundation**
- [ ] `:root` declares the full token set from §2 and §3
- [ ] Playfair Display + DM Sans loaded via `next/font/google` (or equivalent)
- [ ] Warm paper background (`--color-bg`) applied to `body`
- [ ] Agentikas favicon (not the framework default)
- [ ] `<title>` and `<meta name="description">` match the page content (never "Create Next App")

**Typography**
- [ ] Headings use `var(--font-heading)`, body uses `var(--font-body)`
- [ ] `<em>` in headings renders in red (not italic)
- [ ] No font-size <16px in body copy
- [ ] Labels and badges are UPPERCASE with letter-spacing
- [ ] Max 3 font weights per page

**Layout**
- [ ] `<nav aria-label="Main navigation">` wraps site navigation
- [ ] First focusable element is a skip-link to `<main id="main-content">`
- [ ] Max content width `--w-reading` for articles, `--w-page` for landing
- [ ] Section spacing: 96px desktop, 64px mobile
- [ ] Footer: subtle, text-muted

**Components**
- [ ] Cards use 8px border-radius, no box-shadow, 16px gap in grids
- [ ] Buttons: 4px radius, `.btn-primary` and `.btn-ghost` patterns only
- [ ] All interactive elements have visible `:focus-visible` styles
- [ ] Badges >=12px font-size

**Accessibility (see `geo-html-metadata.md` for the full baseline)**
- [ ] Every `<input>` has `<label htmlFor="...">`
- [ ] Error messages use `role="alert" aria-live="polite"`
- [ ] Active nav links have `aria-current="page"`
- [ ] Touch targets >= 44x44px
- [ ] No color-only state indicators
- [ ] Heading hierarchy is h1 → h2 → h3 without skips
- [ ] All images have descriptive `alt` text
- [ ] Dates wrapped in `<time datetime="...">`
- [ ] `prefers-reduced-motion` respected
- [ ] Ran an automated check: `npx @axe-core/cli http://localhost:3000/your-page` or similar

**Copy (see `BRAND.md` and `skills/brand-reviewer.md`)**
- [ ] No banned terms (agencia, consultora, empresa, disruptivo, sinergia, etc.)
- [ ] Tone matches BRAND.md voice
- [ ] `<em>` used only for meaningful emphasis in headings (renders red)

---

## 11. Future — per-blog theming migration path (out of scope)

The design above ships Agentikas defaults to every surface, including user blogs. When
per-blog theming becomes a priority, the migration path is cheap **because of the
custom-properties architecture in §2**:

1. Add a `ThemeProvider` (or plain `<style>` tag in the layout's `<head>`) that reads
   `blogs.brand_config` from Supabase and emits CSS overrides scoped to the blog:
   ```html
   <style>
     :root {
       --color-primary: var(--brand-primary, #C8352A);
       --color-bg:      var(--brand-bg, #F7F5F0);
     }
   </style>
   ```
2. Validate user input with a contrast checker before saving `brand_config` — reject any
   combination that fails WCAG 2.2 AA on `--color-primary` against `--color-bg` and
   `--color-text`.
3. Ship 3–5 curated presets first (Opción B from the product decision), never raw color
   pickers (Opción D), until real usage proves the need.
4. Fallback chain is automatic: if `brand_config` is empty, all `var(--brand-*)` lookups
   fall through to the Agentikas defaults.

**No component code needs to change** to support this — that is the whole point of the
tokens. If you catch yourself hex-coding a color inside a component, stop and use the
token instead.
