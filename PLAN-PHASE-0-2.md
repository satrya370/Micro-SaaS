# PLAN: Fase 0-2 — Detailed Implementation

## Fase 0: Foundation Files

### 0.1 `assets/css/style.css`
- CSS custom properties: `--paper` #FAFAF8, `--ink` #1E2440, `--duplicate-yellow` #F5C242, `--triplicate-pink` #F2A9C4, `--carbon-gray` #8A8778
- Typography: `@font-face` IBM Plex Mono (display) + IBM Plex Sans (body), weight 400/500/700
- Base reset: box-sizing, margin/padding reset
- Utility classes: container, section spacing, typography scale
- Signature "tumpukan nota" card component (3 offset layers: white → yellow → pink)
- Divider perforasi (dashed line motif)
- Button styles (solid yellow CTA, ghost, "sobek" animation)
- Grid katalog (responsive, 1-3 columns)
- Filter tag buttons
- FAQ accordion (details/summary or CSS-only)
- Header, hero, footer styles
- Animation: hero card fill, sobek CTA, prefers-reduced-motion
- Responsive breakpoints: mobile 375px+, tablet 768px+, desktop 1024px+

### 0.2 `assets/js/main.js`
- Template catalog filter by niche tag
- "Sobek" button micro-interaction (click → animate yellow layer tearing)
- Hero card fill animation on load
- FAQ accordion toggle
- Smooth scroll for anchor links
- Active filter state management

### 0.3 `robots.txt`
- Allow: GPTBot, ClaudeBot, PerplexityBot, Google-Extended
- Allow all other bots
- Sitemap reference

### 0.4 `sitemap.xml`
- All pages listed with lastmod, changefreq, priority

---

## Fase 1: Homepage (`index.html`)

### Sections (top to bottom):
1. **Header** — Brand name "Repeatable" + nav (Browse, FAQ, Free Template)
2. **Hero** — Definition Lead paragraph, tagline "Duplicate once. Run forever.", CTA "Browse templates", animated nota card
3. **What is this** — Quick Answer block, ~150 kata, definition-style
4. **Katalog grid** — 4 template cards with "tumpukan nota" style:
   - Fotografer: Client Portal (Free)
   - Airbnb: Property Management (Paid)
   - F&B: Inventory & Order (Free)
   - Kreator: Content Calendar (Paid)
   - Filter buttons: All / Fotografer / Airbnb / F&B / Kreator
5. **How it works** — 3 numbered steps (01 Pilih, 02 Duplicate, 03 Pakai), dashed divider
6. **FAQ** — 5 questions with expand/collapse, JSON-LD schema
7. **Email capture** — MailerLite embed + copy "Dapat 1 template gratis + update sistem baru"
8. **Footer** — License link, SEO footer

### Schema JSON-LD (in <head>):
- Organization
- FAQPage

---

## Fase 2: Template Pages (`/t/`)

### 2.1 `t/photographer-client-portal.html`
- Niche: Fotografer
- Definition Lead, Quick Answer, statistik pembanding
- iframe embed demo (placeholder)
- Daftar fitur granular
- FAQ 3 questions + FAQPage schema
- CTA ganda: "Get free copy" + "Buy Pro ($29)"
- Product schema JSON-LD

### 2.2 `t/airbnb-property-management.html`
- Niche: Airbnb/Host
- Same structure as above, different niche content
- Product schema JSON-LD

---

## Technical Constraints
- NO JavaScript framework — vanilla only
- Must render content server-side (view-source visible)
- Self-host fonts (no Google Fonts CDN)
- IBM Plex Mono (display/headings) + IBM Plex Sans (body)
- Tailwind CSS v4 per AGENTS.md rules — but spec says vanilla CSS... 
  → Use Tailwind v4 CDN for utility classes + custom CSS for signature design elements
- Mobile-first responsive

## Execution Order
1. `style.css` + `main.js` + `robots.txt` in parallel
2. `index.html` (depends on CSS classes)
3. Both template pages in parallel (depends on CSS + same pattern)
