# Skill: Agentikas Design System

This is the definitive visual identity and UX/UI reference for all Agentikas products.
Every page, component, and project built under Agentikas MUST follow these rules.

Apply this skill alongside BRAND.md (voice/tone) and geo-html-metadata.md (SEO/AI structure).

---

## 1. Design Philosophy

Agentikas visual identity sits at the intersection of **editorial elegance** and
**technical credibility**. Think: a research lab that publishes beautifully.

**Core principles:**
- **Clarity over decoration** — every element earns its place
- **Dark-first, light-compatible** — dark theme is the primary experience
- **Typography-driven** — the text IS the design, not an afterthought
- **Quiet confidence** — no flashy animations, no gimmicks, no noise
- **Accessible by default** — WCAG 2.1 AA minimum, always

**Inspirations:** Linear (layout density, dark theme), Stripe Press (editorial quality),
Vercel (technical polish), Ghost (reading experience).

---

## 2. Color System

### Dark theme (primary)

```css
:root {
  /* Backgrounds */
  --bg-primary:    #0A0A0A;   /* Page background */
  --bg-surface:    #141414;   /* Cards, panels */
  --bg-elevated:   #1A1A1A;   /* Hover states, modals */
  --bg-overlay:    #222222;   /* Dropdowns, tooltips */

  /* Text */
  --text-primary:  #F5F5F5;   /* Headings, primary content */
  --text-secondary: #A0A0A0;  /* Subtitles, secondary info */
  --text-muted:    #666666;   /* Timestamps, metadata */
  --text-disabled: #444444;   /* Disabled states */

  /* Brand */
  --red:           #C8352A;   /* Primary accent — CTAs, badges, links */
  --red-hover:     #E04038;   /* Hover state for red */
  --red-muted:     rgba(200, 53, 42, 0.15); /* Red backgrounds (badges) */

  /* Borders */
  --border:        rgba(255, 255, 255, 0.08);  /* Subtle dividers */
  --border-hover:  rgba(255, 255, 255, 0.15);  /* Hover borders */

  /* Functional */
  --success:       #2D8A4E;
  --warning:       #D4A017;
  --error:         #C8352A;   /* Same as brand red */
}
```

### Light theme (secondary — landing pages, contact)

```css
[data-theme="light"] {
  --bg-primary:    #F7F5F0;   /* Warm paper */
  --bg-surface:    #FFFFFF;
  --bg-elevated:   #FFFFFF;

  --text-primary:  #0D0D0D;   /* Near black */
  --text-secondary: #6B6B6B;
  --text-muted:    #999999;

  --border:        #D8D4CC;
  --border-hover:  #BBB5AA;
}
```

### Rules
- **Never use pure black (#000) or pure white (#FFF)** — always slightly off
- **Red (#C8352A) is the ONLY accent color** — no blues, greens, or purples as accents
- **Borders are always subtle** — max 8-15% opacity white in dark, warm gray in light
- **Text hierarchy through opacity/weight**, not color variety

---

## 3. Typography

### Font stack

```css
/* Headings — editorial, confident */
--font-heading: "Playfair Display", Georgia, "Times New Roman", serif;

/* Body — clean, technical, readable */
--font-body: "DM Sans", -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;

/* Code — monospace for technical content */
--font-mono: "JetBrains Mono", "Fira Code", "SF Mono", Consolas, monospace;
```

### Scale

| Token | Size | Weight | Line-height | Use |
|---|---|---|---|---|
| `--text-hero` | 56px | 700 | 1.1 | Landing page hero only |
| `--text-h1` | 40px | 700 | 1.15 | Page titles |
| `--text-h2` | 28px | 700 | 1.25 | Section headings |
| `--text-h3` | 20px | 600 | 1.35 | Subsection headings |
| `--text-body` | 17px | 400 | 1.7 | Body text, paragraphs |
| `--text-body-sm` | 15px | 400 | 1.6 | Card descriptions |
| `--text-small` | 13px | 500 | 1.5 | Metadata, tags, dates |
| `--text-micro` | 11px | 500 | 1.4 | Labels, badges, uppercase |

### Rules
- **Headings always serif** (Playfair Display) — this is the Agentikas signature
- **Body always sans-serif** (DM Sans) — clean, modern readability
- **Never use more than 3 weights** on a single page (400, 500, 700)
- **Line height for body is 1.7** minimum — generous breathing room
- **Labels and badges are UPPERCASE** with letter-spacing: 1-2px
- **`<em>` in headings uses red color** — this is an Agentikas pattern from the landing

```css
h1 em, h2 em { color: var(--red); font-style: normal; }
```

---

## 4. Layout

### Grid and spacing

```css
/* Max widths */
--max-width-content: 720px;    /* Blog posts, articles (reading) */
--max-width-page:    1100px;   /* Landing sections, dashboards */
--max-width-wide:    1400px;   /* Full-width sections, hero */

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
```

### Page structure

```
┌─── Nav (fixed, 64px height, blur backdrop) ───────────┐
│  Logo          Links                     EN|ES  CTA   │
├───────────────────────────────────────────────────────┤
│                                                       │
│  Content (centered, max-width applied)                │
│  padding: 0 48px (desktop), 0 24px (mobile)           │
│                                                       │
├───────────────────────────────────────────────────────┤
│  Footer (subtle, text-muted)                          │
└───────────────────────────────────────────────────────┘
```

### Section spacing
- Between major sections: `96px` (desktop), `64px` (mobile)
- Between heading and content: `32px`
- Between cards in a grid: `1px` (Agentikas signature gap)

### Card grid
```css
/* 3-column grid with 1px gap (signature style) */
.card-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1px;
}

/* 2-column for features */
.feature-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1px;
}

/* Responsive */
@media (max-width: 768px) {
  .card-grid, .feature-grid {
    grid-template-columns: 1fr;
  }
}
```

---

## 5. Components

### Nav
```css
nav {
  position: fixed;
  top: 0;
  height: 64px;
  background: rgba(10, 10, 10, 0.8);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border);
  z-index: 100;
}
```
- Logo left: "Agenti**kas**" (kas in red), Playfair Display, 24px
- Links: DM Sans, 14px, 500 weight, text-secondary, hover: text-primary
- CTA button: solid red background, white text, 14px
- Language switcher: "EN | ES", text-muted, active is text-primary bold

### Buttons
```css
/* Primary — red, for main CTAs */
.btn-primary {
  background: var(--red);
  color: white;
  padding: 10px 24px;
  border-radius: 4px;
  font-size: 14px;
  font-weight: 500;
  transition: background 0.2s;
}
.btn-primary:hover { background: var(--red-hover); }

/* Ghost — outline, for secondary actions */
.btn-ghost {
  background: transparent;
  color: var(--text-secondary);
  border: 1px solid var(--border);
  padding: 10px 24px;
  border-radius: 4px;
  font-size: 14px;
  font-weight: 500;
  transition: border-color 0.2s, color 0.2s;
}
.btn-ghost:hover {
  border-color: var(--border-hover);
  color: var(--text-primary);
}
```
- **Border radius: 4px** everywhere — no rounded-full, no sharp corners
- **Never use shadows on buttons** — only border and background changes
- **Disabled: opacity 0.4**, cursor not-allowed

### Cards
```css
.card {
  background: var(--bg-surface);
  border: 1px solid var(--border);
  padding: 32px;
  transition: border-color 0.2s;
}
.card:hover {
  border-color: var(--border-hover);
}
```
- **No box shadows** — only border changes on hover
- **No border-radius on cards** — sharp edges (Agentikas signature)
- Cards in grids have **1px gap** (the border becomes the separator)

### Badges / Pills
```css
/* Brand badge (Agentikas Labs posts) */
.badge-labs {
  background: var(--red-muted);
  color: var(--red);
  padding: 4px 10px;
  border-radius: 4px;
  font-size: 11px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1px;
}

/* Neutral badge (tags, categories) */
.badge {
  background: rgba(255, 255, 255, 0.06);
  color: var(--text-secondary);
  padding: 4px 10px;
  border-radius: 4px;
  font-size: 11px;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 1px;
}
```

### Section labels
```css
/* Red uppercase label above section headings */
.label {
  color: var(--red);
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  margin-bottom: 16px;
  display: block;
}
```

---

## 6. Blog-Specific Patterns

### Article page (reading experience)
```css
.article {
  max-width: 720px;
  margin: 0 auto;
  padding: 64px 24px;
}

.article h1 {
  font-family: var(--font-heading);
  font-size: 40px;
  font-weight: 700;
  line-height: 1.15;
  margin-bottom: 16px;
}

.article .prose {
  font-family: var(--font-body);
  font-size: 17px;
  line-height: 1.8;
  color: var(--text-primary);
}

.article .prose h2 {
  font-family: var(--font-heading);
  font-size: 28px;
  margin-top: 48px;
  margin-bottom: 16px;
}

.article .prose code {
  font-family: var(--font-mono);
  background: var(--bg-elevated);
  padding: 2px 6px;
  border-radius: 3px;
  font-size: 15px;
}

.article .prose pre {
  background: var(--bg-surface);
  border: 1px solid var(--border);
  border-radius: 4px;
  padding: 24px;
  overflow-x: auto;
}

.article .prose a {
  color: var(--red);
  text-decoration: underline;
  text-underline-offset: 3px;
}

.article .prose img {
  border-radius: 4px;
  margin: 32px 0;
}
```

### Post cards (home grid)
```css
.post-card {
  background: var(--bg-surface);
  border: 1px solid var(--border);
  padding: 24px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  transition: border-color 0.2s;
}

.post-card:hover {
  border-color: var(--border-hover);
}

.post-card-title {
  font-family: var(--font-heading);
  font-size: 20px;
  font-weight: 700;
  color: var(--text-primary);
}

.post-card-meta {
  font-size: 13px;
  color: var(--text-muted);
}

.post-card-views {
  font-size: 13px;
  color: var(--text-muted);
}
```

---

## 7. Motion and Transitions

- **Transitions: 0.2s ease** for color, background, border, opacity
- **No bounce, no spring, no elastic** — everything is subtle and fast
- **Page transitions: none** — instant navigation
- **Hover effects: border-color change or opacity shift**, never scale or translate
- **Loading states: subtle opacity pulse**, not spinners

```css
/* Standard transition */
transition: color 0.2s, background-color 0.2s, border-color 0.2s;

/* Reveal animation (for sections on scroll) */
@keyframes reveal {
  from { opacity: 0; transform: translateY(12px); }
  to   { opacity: 1; transform: translateY(0); }
}
.reveal { animation: reveal 0.5s ease forwards; }
```

---

## 8. Responsive Breakpoints

```css
/* Mobile first */
@media (min-width: 640px)  { /* sm  — small tablets */ }
@media (min-width: 768px)  { /* md  — tablets */ }
@media (min-width: 1024px) { /* lg  — desktop */ }
@media (min-width: 1280px) { /* xl  — wide desktop */ }
```

### Key responsive rules
- **Nav collapses to hamburger** at `768px`
- **Card grids go to single column** at `768px`
- **Section padding reduces** from `48px` to `24px` horizontal
- **Hero text scales down** from `56px` to `36px`
- **Body text stays 17px** — never reduce below 16px on mobile

---

## 9. Do and Don't

### DO
- Use Playfair Display for headings, DM Sans for body
- Use red (#C8352A) as the only accent color
- Use 1px gap in card grids
- Use sharp corners on cards (no border-radius)
- Use 4px border-radius on buttons and badges
- Use subtle borders, never shadows
- Use dark theme as primary
- Use generous line-height (1.7+ for body)
- Use uppercase + letter-spacing for labels and badges

### DON'T
- Don't use colors other than red as accent (no blue buttons, no green badges)
- Don't use box-shadows on cards or sections
- Don't use rounded corners on cards (>0px)
- Don't use border-radius >4px on anything
- Don't use bold colors for backgrounds (no red backgrounds except badges)
- Don't use decorative icons or illustrations
- Don't use more than 3 font weights per page
- Don't use animations that move elements more than 12px
- Don't use gradient text
- Don't use hamburger menus on desktop
- Don't reduce body text below 16px on any device

---

## 10. Implementation Checklist

Before shipping any Agentikas page:

- [ ] Dark theme is the default experience
- [ ] Playfair Display for headings, DM Sans for body
- [ ] Red (#C8352A) is the only accent color
- [ ] Nav: fixed, 64px, blur backdrop, logo left, links right
- [ ] Cards: no shadows, no border-radius, 1px gap in grids
- [ ] Buttons: 4px radius, red primary, ghost secondary
- [ ] Labels: red, uppercase, letter-spacing 2px
- [ ] Body text: 17px, line-height 1.7+
- [ ] Max content width: 720px (reading), 1100px (pages)
- [ ] Section spacing: 96px desktop, 64px mobile
- [ ] Responsive: single column at 768px
- [ ] `<em>` in headings renders in red, not italic
- [ ] Footer: subtle, text-muted, "Powered by Agentikas"
