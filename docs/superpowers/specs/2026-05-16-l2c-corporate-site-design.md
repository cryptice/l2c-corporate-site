# L2 Consulting AB — Corporate Site Design

**Date:** 2026-05-16
**Status:** Approved (pending user review of this doc)

## Goal

A minimal static corporate site that establishes a web presence for L2 Consulting AB. The site is informational, not lead-generating: visitors who land on it should be able to verify the company exists, see who runs it, and find a contact email.

## Scope

In scope:

- A single static page in English
- Hero, two-people section, footer with registration details
- Plain HTML + CSS, no build step, no JavaScript
- Deployment to GitHub Pages with custom domain `l2c.se`

Out of scope:

- Portfolio / case studies
- Services pricing or lead-capture forms
- Blog or news
- Multiple language versions (English only)
- A CMS or backend of any kind

## Content

### Company facts (canonical)

- **Legal name:** L2 Consulting AB
- **Org.nr:** 556992-2155
- **City:** Älvkarleby, Sweden
- **Contact email:** kontakt@l2c.se
- **Partners:** Erik Lindblad, Stina Lindblad

### Page sections

**Nav (top strip)**
- Left: brand wordmark "L2 Consulting AB"
- Right: language indicator "EN" (static, no menu — single page)

**Hero**
- Eyebrow: "Älvkarleby, Sweden"
- Headline: "Two trades. One company."
- Lede: "L2 Consulting AB is a small Swedish consultancy run by two people. We bring together work that does not usually share a roof — from IT and construction to outdoor education and floristry."

**Partners (side-by-side, two columns)**

Erik Lindblad
- Role label: "Partner"
- Tags: IT & software, Construction
- Bio: "Works across software systems and the built environment — from web platforms and developer tooling to practical construction projects."

Stina Lindblad
- Role label: "Partner"
- Tags: Outdoor teaching, Floristry
- Bio: "Teaches outdoors and works as a florist — combining education in nature with the craft of flowers and seasonal arrangements."

**Footer**
- Left: contact email `kontakt@l2c.se` (mailto link)
- Right: "L2 Consulting AB" and "Org.nr 556992-2155"

## Visual design

Direction: **Scandinavian minimal, warm paper variant** (style B from brainstorm).

- Background: cream / warm off-white (`#f4ede2`)
- Foreground text: deep warm brown (`#2b2620`)
- Accent: a single soft radial-gradient "disc" in muted gold/ochre (`#c4a878 → #8a6f48`, ~45% opacity), positioned off the top-right edge of the hero. The only decorative element on the page. It quietly evokes a sun / horizon / floral disc without being literal.
- Type: Inter (or system sans fallback) throughout. Weight 500 for headings, 400 for body. Letter-spacing slightly negative on headings (`-0.01em` to `-0.02em`), wider tracking + uppercase for small label text (eyebrow, role, footer).
- Dividers: 1px lines at 15% opacity for section separation. Dotted underline on tag lists.

### Layout

- Max content width ~760px, centered
- Horizontal padding: 40px on desktop, 24px below 640px
- Hero: 70–90px vertical padding, single column, copy capped at ~520px wide
- Partners: 2 equal columns on desktop, stacked on mobile (<640px), separated by a 1px vertical divider on desktop / horizontal on mobile
- Footer: slim horizontal strip with email left, registration right; wraps on small screens

### Responsive behavior

- One breakpoint at 640px
- Below 640px: partner columns stack; hero headline drops from 44px to 32px; padding reduces

## Technical design

### File structure

```
l2c-site/
├── index.html          # the single page
├── styles.css          # all styling
├── CNAME               # GitHub Pages custom domain
└── README.md           # short note on how to update + deploy
```

No build step. No JS. No dependencies. The font is loaded from Google Fonts via a `<link>` tag in `index.html` (with `preconnect`), with a system-sans fallback so the page renders even if the font is blocked.

### HTML structure

Semantic HTML5: `<header>` for the nav, `<main>` containing one `<section class="hero">` and one `<section class="partners">` with two nested `<article>` elements, and `<footer>` for the registration strip. Headings hierarchy: h1 for the hero headline, h2 for each partner name. Email is a `<a href="mailto:kontakt@l2c.se">` link.

### CSS approach

A single `styles.css`. CSS custom properties for the color palette and spacing scale, defined on `:root`, used throughout. CSS grid for the partners layout. No utility framework. No CSS reset beyond `box-sizing: border-box` and minimal element resets (margin/padding on body and headings).

### Accessibility

- Sufficient text contrast across all text — body, muted, and faint label text all meet WCAG AA (≥4.5:1). Verified during implementation: body 12.89:1, muted 5.53:1, faint 5.20:1.
- Logical heading order (h1 → h2)
- The decorative disc carries no information; implemented as a CSS-only element with `aria-hidden`
- Email link works as both a clickable mailto and as copyable text
- Page works without JS (since there is none) and without CSS (semantic structure still readable)

### Metadata

- `<title>L2 Consulting AB</title>`
- `<meta name="description">` with a one-sentence company summary
- `<meta name="viewport" content="width=device-width, initial-scale=1">`
- `lang="en"` on `<html>`
- Open Graph / Twitter tags: skip for v1 (no link previews expected to matter for a presence site)
- Favicon: a simple "L2" monogram as an inline SVG favicon

## Deployment

- Host: GitHub Pages
- Custom domain: `l2c.se` (apex) and `www.l2c.se`
- `CNAME` file in repo root contains `l2c.se`
- DNS: A records for the apex pointing at GitHub's four published IPs, CNAME for `www` pointing at the GitHub Pages domain
- HTTPS: enabled in GitHub repo settings ("Enforce HTTPS")

Deployment steps and exact DNS values will be in the README. The actual repo creation and DNS configuration are owner tasks, not coded artifacts.

## Non-goals reminder

This site is intentionally tiny. Don't add: contact forms, analytics, cookie banners (no tracking → no consent banner needed), multiple language toggles, a blog scaffold "for later," a CSS framework, a build pipeline. If any of those become needed in the future, they get added then — not pre-empted now.
