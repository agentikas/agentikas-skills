# Skill: GEO — HTML Metadata for Machines and AI Agents

Generative Engine Optimization (GEO) is the practice of structuring HTML so that AI agents,
search engines, and LLMs can find, understand, cite, and act on your content without guessing.

This skill defines the mandatory metadata and HTML patterns for every Agentikas project.
Apply these rules to every page you build or review.

Sources: MX (Machine Experience) by CogNovaMX, Google Structured Data docs, Bing Webmaster
Guidelines, WCAG 2.1 AA, Schema.org.

---

## 1. Discovery Layer — Make your content findable

AI agents and crawlers must be able to find and render your content.

### robots.txt
```
User-agent: *
Allow: /

# Allow AI training crawlers
User-agent: GPTBot
Allow: /

User-agent: ClaudeBot
Allow: /

User-agent: Google-Extended
Allow: /

Sitemap: https://yourdomain.com/sitemap.xml
```

### sitemap.xml
- Every public page must be in the sitemap
- Include `<lastmod>` dates (ISO 8601)
- Update sitemap when content changes
- For blogs: auto-generate from published posts

### Server-Side Rendering
- **Critical:** AI crawlers cannot parse JavaScript-rendered content
- Every page must return full HTML from the server (SSR or SSG)
- Test: `curl -s https://yourpage.com | grep '<h1>'` must return content
- Next.js with `force-dynamic` or static export satisfies this

### IndexNow (optional, recommended)
- Notify Bing/Yandex immediately when content changes
- Faster indexing than waiting for crawl

---

## 2. Semantic HTML — Structure for machines

Use semantic elements so agents understand page structure without CSS.

### Required semantic elements
```html
<header>     <!-- Site header, nav -->
<nav>        <!-- Navigation links -->
<main>       <!-- Primary page content (one per page) -->
<article>    <!-- Self-contained content (blog post, product) -->
<section>    <!-- Thematic grouping within article -->
<aside>      <!-- Related but secondary content -->
<footer>     <!-- Site footer, credits -->
<time>       <!-- Dates and times with datetime attribute -->
```

### Heading hierarchy
```html
<h1>Page Title</h1>              <!-- One per page, matches <title> -->
  <h2>Section</h2>               <!-- Major sections -->
    <h3>Subsection</h3>          <!-- Details within section -->
```
- **Never skip levels** (h1 → h3 without h2)
- Headings must describe content structure, not visual styling

### Links and buttons
```html
<!-- GOOD: descriptive link text -->
<a href="/menu">Browse our full menu</a>

<!-- BAD: generic text -->
<a href="/menu">Click here</a>

<!-- GOOD: semantic button -->
<button type="submit">Reserve a table</button>

<!-- BAD: div pretending to be a button -->
<div class="btn" onclick="submit()">Reserve</div>
```

### Images
```html
<img
  src="/photo.jpg"
  alt="Interior of Los Granainos restaurant showing terrace with sea views"
  width="800"
  height="600"
  loading="lazy"
/>
```
- `alt` must be descriptive (not "image1.jpg" or "photo")
- Include dimensions to prevent layout shift

### Forms
```html
<form>
  <label for="email">Email address</label>
  <input
    id="email"
    type="email"
    name="email"
    required
    aria-required="true"
    aria-invalid="false"
    aria-describedby="email-error"
  />
  <span id="email-error" role="alert" aria-live="polite"></span>
  <button type="submit">Subscribe</button>
</form>
```

### Time and dates
```html
<time datetime="2026-04-08">April 8, 2026</time>
<time datetime="2026-04-08T21:00:00+02:00">9:00 PM CEST</time>
```
- Always use `datetime` attribute with ISO 8601

---

## 3. Meta Tags — The machine's first impression

### Required meta tags (every page)
```html
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Page Title — Site Name</title>
  <meta name="description" content="Concise description under 155 chars" />
  <link rel="canonical" href="https://yourdomain.com/this-page" />
  <meta name="robots" content="index, follow" />
</head>
```

### Open Graph (Facebook, LinkedIn, AI agents)
```html
<meta property="og:type" content="article" />
<meta property="og:title" content="Page Title" />
<meta property="og:description" content="Description for social sharing" />
<meta property="og:image" content="https://yourdomain.com/image.jpg" />
<meta property="og:url" content="https://yourdomain.com/this-page" />
<meta property="og:site_name" content="Agentikas Labs" />
<meta property="og:locale" content="es_ES" />
```

### Twitter/X Cards
```html
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="Page Title" />
<meta name="twitter:description" content="Description" />
<meta name="twitter:image" content="https://yourdomain.com/image.jpg" />
```

### Article-specific (blog posts)
```html
<meta property="article:published_time" content="2026-04-08T10:00:00Z" />
<meta property="article:modified_time" content="2026-04-08T12:00:00Z" />
<meta property="article:author" content="Salva Moreno" />
<meta property="article:section" content="WebMCP" />
<meta property="article:tag" content="WebMCP" />
<meta property="article:tag" content="AI agents" />
```

### Language alternates (multilingual sites)
```html
<link rel="alternate" hreflang="es" href="https://yourdomain.com/es/page" />
<link rel="alternate" hreflang="en" href="https://yourdomain.com/en/page" />
<link rel="alternate" hreflang="x-default" href="https://yourdomain.com/page" />
```

---

## 4. Schema.org JSON-LD — Structured data for AI

JSON-LD is the preferred format (Google recommendation). Place in `<head>` or end of `<body>`.

### Blog post
```json
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "Your Article Title",
  "description": "Article summary under 155 chars",
  "author": {
    "@type": "Person",
    "name": "Salva Moreno",
    "url": "https://agentikas.ai"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Agentikas Labs",
    "logo": {
      "@type": "ImageObject",
      "url": "https://agentikas.ai/logo.png"
    }
  },
  "datePublished": "2026-04-08",
  "dateModified": "2026-04-08",
  "image": "https://yourdomain.com/image.jpg",
  "url": "https://yourdomain.com/article-slug",
  "keywords": ["WebMCP", "AI agents", "restaurants"],
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "https://yourdomain.com/article-slug"
  }
}
```

### Organization / Business
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Agentikas Labs",
  "url": "https://agentikas.ai",
  "logo": "https://agentikas.ai/logo.png",
  "description": "First agentic software lab in Spain",
  "email": "hola@agentikas.ai",
  "foundingDate": "2026",
  "sameAs": [
    "https://github.com/agentikas",
    "https://linkedin.com/company/agentikas"
  ]
}
```

### Restaurant
```json
{
  "@context": "https://schema.org",
  "@type": "Restaurant",
  "name": "Los Granainos",
  "description": "Family restaurant since 1987",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Calle Example 1",
    "addressLocality": "Cala de Mijas",
    "addressCountry": "ES"
  },
  "telephone": "+34600000000",
  "servesCuisine": "Andalusian",
  "priceRange": "€€",
  "openingHours": "Mo-Su 12:00-23:00",
  "acceptsReservations": true,
  "menu": "https://losgranainos.com/menu"
}
```

### Product with pricing (critical for AI comparison)
```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "@id": "https://yoursite.com/products/xyz789",
  "name": "Product Name",
  "description": "Clear, factual description",
  "offers": {
    "@type": "Offer",
    "price": "29.99",
    "priceCurrency": "EUR",
    "availability": "https://schema.org/InStock"
  },
  "additionalProperty": [
    {
      "@type": "PropertyValue",
      "name": "Weight",
      "value": "250g"
    }
  ]
}
```

### Pricing rules (from MX)
- Always use ISO 4217 currency codes (EUR, USD, GBP)
- Explicit decimal format — never embed prices in paragraph text
- Include `priceCurrency` — agents cannot infer currency from context

---

## 5. ARIA and State Communication

AI agents navigating interactive pages need DOM-reflected state.

### State must be in the DOM, not just visual
```html
<!-- GOOD: state in DOM -->
<button aria-disabled="true" disabled>Processing...</button>
<div role="alert" aria-live="polite">Reservation confirmed</div>
<input aria-invalid="true" aria-describedby="error-msg" />

<!-- BAD: state only visual -->
<div class="spinner"></div>  <!-- Agent can't see "loading" -->
<div style="color: green">Success</div>  <!-- Agent can't see "success" -->
```

### Progress tracking
```html
<div data-state="loading">Processing your reservation...</div>
<div data-state="success">Reservation confirmed for 4 people</div>
<div data-state="error">No tables available at that time</div>
```

### Key ARIA attributes for agents
| Attribute | Purpose | Example |
|---|---|---|
| `aria-required="true"` | Field is mandatory | Form inputs |
| `aria-invalid="true/false"` | Validation state | After form submit |
| `aria-describedby="id"` | Links input to error message | Error handling |
| `aria-disabled="true"` | Element not interactive | Disabled buttons |
| `aria-live="polite"` | Content updates dynamically | Status messages |
| `role="alert"` | Important feedback | Confirmations, errors |

---

## 6. Content Structure for Citation

AI agents cite content at the fact level. Structure accordingly.

### One fact per element
```html
<!-- GOOD: citable -->
<p>WebMCP is supported in Chrome since <time datetime="2026-02">February 2026</time>.</p>
<p>Over <strong>800 million</strong> ChatGPT users can discover WebMCP-enabled businesses.</p>

<!-- BAD: multiple facts buried in one paragraph -->
<p>WebMCP launched in February 2026 and already has 800M potential users
through ChatGPT, while also being supported by Google and Microsoft who
announced it at a joint press conference.</p>
```

### Statistics and data
```html
<!-- Always mark up numbers with context -->
<p>Lighthouse Performance score: <data value="100">100/100</data></p>
<p>Setup time: <time datetime="PT48H">48 hours</time></p>
```

---

## 7. Checklist — Apply to every Agentikas page

### Before shipping any page:

**Discovery**
- [ ] Page returns full HTML from server (not JS-rendered shell)
- [ ] Included in sitemap.xml
- [ ] robots.txt allows crawling
- [ ] Canonical URL set

**Head**
- [ ] `<title>` is unique, descriptive, under 60 chars
- [ ] `<meta name="description">` under 155 chars
- [ ] Open Graph tags complete (type, title, description, image, url)
- [ ] Twitter Card tags complete
- [ ] Language alternates for multilingual pages
- [ ] Canonical URL matches the preferred version

**Structure**
- [ ] One `<h1>` per page
- [ ] Heading hierarchy follows h1→h2→h3 without skips
- [ ] Semantic elements used (`<main>`, `<article>`, `<nav>`, `<header>`, `<footer>`)
- [ ] Links have descriptive text (no "click here")
- [ ] Images have descriptive `alt` text
- [ ] Dates use `<time datetime="...">` 

**Schema.org**
- [ ] JSON-LD in `<head>` or `<body>`
- [ ] Correct `@type` for the page content
- [ ] All required properties present
- [ ] Prices use ISO 4217 currency codes
- [ ] Validated with Google Rich Results Test

**Interactivity**
- [ ] Buttons are `<button>`, not styled `<div>`
- [ ] Form inputs have `<label>` with `for`/`id`
- [ ] State changes reflected in DOM (not just visual)
- [ ] Error messages use `role="alert"` + `aria-live`

---

## Key principle

> "Design for machines with zero-tolerance requirements, and you automatically
> create structure that benefits everyone." — MX, CogNovaMX

Machines cannot infer. Machines cannot guess. Every piece of information must be
explicit in the HTML. If an AI agent has to guess what your page means, you've
already lost.
