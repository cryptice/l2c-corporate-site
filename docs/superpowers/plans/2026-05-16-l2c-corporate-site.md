# L2 Consulting AB — Corporate Site Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-page static corporate site for L2 Consulting AB in plain HTML + CSS, deployable to GitHub Pages with a custom domain.

**Architecture:** Two files (`index.html`, `styles.css`) plus deployment artifacts (`CNAME`, `README.md`, `favicon.svg`). No build step, no JS, no framework. Semantic HTML5, CSS custom properties for theme, CSS grid for partners layout, single responsive breakpoint at 640px.

**Tech Stack:** HTML5, CSS3 (custom properties, grid), Inter web font from Google Fonts. Hosting: GitHub Pages.

**Spec:** `docs/superpowers/specs/2026-05-16-l2c-corporate-site-design.md`

**Note on TDD:** This is a static HTML/CSS site with no JS — no unit tests apply. Each task uses **verification-based discipline**: explicit visual + content checks after every change. Treat the verification steps as non-negotiable as you would tests.

---

## File Structure

```
l2c-site/
├── index.html          # Single page — nav, hero, partners, footer
├── styles.css          # All styling: variables, layout, typography, responsive
├── favicon.svg         # Inline SVG "L2" monogram favicon
├── CNAME               # GitHub Pages custom domain (contains: l2c.se)
├── README.md           # How to update content + deploy
└── .gitignore          # Excludes .superpowers/, .DS_Store
```

---

### Task 1: Initialize repository scaffolding

**Files:**
- Create: `.gitignore`
- Create: `README.md`
- Create: `CNAME`

- [ ] **Step 1: Initialize git repo**

Run:

```bash
cd /Users/erik/development/l2c-site
git init
git branch -M main
```

Expected: `Initialized empty Git repository in /Users/erik/development/l2c-site/.git/`

- [ ] **Step 2: Create `.gitignore`**

Write `.gitignore`:

```
.DS_Store
.superpowers/
node_modules/
*.log
```

- [ ] **Step 3: Create `CNAME`**

Write `CNAME` (no trailing newline matters, but a single line is fine):

```
l2c.se
```

- [ ] **Step 4: Create `README.md`**

Write `README.md`:

````markdown
# L2 Consulting AB — Website

Single-page static site for L2 Consulting AB.

- **Live:** https://l2c.se
- **Stack:** Plain HTML + CSS, no build step
- **Host:** GitHub Pages

## Update content

Edit `index.html` (text and structure) and `styles.css` (look). Push to `main` — GitHub Pages redeploys automatically.

## Local preview

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Any static file server works.

## Deploy (first time)

1. Push this repo to GitHub.
2. Repo Settings → Pages → Source: `Deploy from a branch`, Branch: `main`, Folder: `/ (root)`.
3. Custom domain: `l2c.se` (already set in `CNAME`).
4. Tick **Enforce HTTPS** once the certificate is issued (can take a few minutes).

### DNS

For apex `l2c.se`, set A records to GitHub's four published IPs:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

For `www.l2c.se`, set a CNAME to `<github-username>.github.io`.

GitHub's current Pages IPs: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site
````

- [ ] **Step 5: Verify files exist**

Run:

```bash
ls -1
```

Expected output includes: `.gitignore`, `CNAME`, `README.md`.

- [ ] **Step 6: Commit**

Run:

```bash
git add .gitignore CNAME README.md
git commit -m "chore: initialize repo with deployment scaffolding"
```

---

### Task 2: Build `index.html` with semantic structure and content

**Files:**
- Create: `index.html`

- [ ] **Step 1: Write `index.html`**

Write the complete `index.html` (content is final — text is from the approved spec; no styles linked yet, that's Task 3):

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>L2 Consulting AB</title>
  <meta name="description" content="L2 Consulting AB is a small Swedish consultancy in Älvkarleby — IT, construction, outdoor teaching, and floristry.">
</head>
<body>
  <header class="nav">
    <span class="brand">L2 Consulting AB</span>
    <span class="lang">EN</span>
  </header>

  <main>
    <section class="hero">
      <p class="eyebrow">Älvkarleby, Sweden</p>
      <h1>Two trades.<br>One company.</h1>
      <p class="lede">L2 Consulting AB is a small Swedish consultancy run by two people. We bring together work that does not usually share a roof — from IT and construction to outdoor education and floristry.</p>
      <div class="disc" aria-hidden="true"></div>
    </section>

    <hr class="divider">

    <section class="partners">
      <article class="person">
        <p class="role">Partner</p>
        <h2>Erik Lindblad</h2>
        <ul class="tags">
          <li>IT &amp; software</li>
          <li>Construction</li>
        </ul>
        <p class="bio">Works across software systems and the built environment — from web platforms and developer tooling to practical construction projects.</p>
      </article>

      <article class="person">
        <p class="role">Partner</p>
        <h2>Stina Lindblad</h2>
        <ul class="tags">
          <li>Outdoor teaching</li>
          <li>Floristry</li>
        </ul>
        <p class="bio">Teaches outdoors and works as a florist — combining education in nature with the craft of flowers and seasonal arrangements.</p>
      </article>
    </section>
  </main>

  <footer class="site-footer">
    <a class="contact" href="mailto:kontakt@l2c.se">kontakt@l2c.se</a>
    <span class="reg">
      <span>L2 Consulting AB</span>
      <span>Org.nr 556992-2155</span>
    </span>
  </footer>
</body>
</html>
```

- [ ] **Step 2: Verify unstyled page renders all content**

Run:

```bash
python3 -m http.server 8000 &
sleep 1
curl -s http://localhost:8000/ | grep -E "L2 Consulting AB|Erik Lindblad|Stina Lindblad|kontakt@l2c.se|556992-2155"
kill %1 2>/dev/null
```

Expected: all of "L2 Consulting AB", "Erik Lindblad", "Stina Lindblad", "kontakt@l2c.se", and "556992-2155" appear in the output.

Then open `http://localhost:8000/` in a browser and confirm every section reads top-to-bottom as plain text: nav strip, hero with eyebrow + headline + lede, both partner sections, footer with email + org.nr. It will look unstyled — that's expected.

- [ ] **Step 3: Commit**

Run:

```bash
git add index.html
git commit -m "feat: add semantic HTML structure and content"
```

---

### Task 3: Add base styles — variables, reset, typography, font loading

**Files:**
- Create: `styles.css`
- Modify: `index.html` (add `<link>` to stylesheet and Google Fonts)

- [ ] **Step 1: Create `styles.css` with reset + variables + base typography**

Write `styles.css`:

```css
:root {
  --bg: #f4ede2;
  --fg: #2b2620;
  --fg-muted: rgba(43, 38, 32, 0.72);
  --fg-faint: rgba(43, 38, 32, 0.55);
  --divider: rgba(43, 38, 32, 0.15);
  --divider-soft: rgba(43, 38, 32, 0.18);
  --accent-from: #c4a878;
  --accent-to: #8a6f48;

  --pad-x: 40px;
  --pad-x-mobile: 24px;
  --max-w: 760px;

  --font-sans: 'Inter', system-ui, -apple-system, 'Segoe UI', Helvetica, Arial, sans-serif;
}

*, *::before, *::after {
  box-sizing: border-box;
}

html, body {
  margin: 0;
  padding: 0;
}

body {
  background: var(--bg);
  color: var(--fg);
  font-family: var(--font-sans);
  font-weight: 400;
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

h1, h2, h3, p, ul {
  margin: 0;
}

ul {
  padding: 0;
  list-style: none;
}

a {
  color: inherit;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}
```

- [ ] **Step 2: Link the stylesheet and Inter font from `index.html`**

In `index.html`, add the three `<link>` tags inside `<head>`, right after the existing `<meta name="description">`:

```html
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="styles.css">
```

- [ ] **Step 3: Verify base styles applied**

Run:

```bash
python3 -m http.server 8000 &
sleep 1
```

Open `http://localhost:8000/` in a browser. Expected:

- Background is cream/warm off-white, not white
- Text is dark warm brown, not black
- Font is Inter (rounded, neutral sans-serif) — not Times, not Helvetica
- Default browser margin is gone — content sits flush to the edges
- Bullet markers on the `<ul class="tags">` are gone

Stop the server:

```bash
kill %1 2>/dev/null
```

- [ ] **Step 4: Commit**

Run:

```bash
git add styles.css index.html
git commit -m "feat: add base styles, color palette, and Inter font"
```

---

### Task 4: Lay out nav, hero, partners, and footer

**Files:**
- Modify: `styles.css` (append layout rules)

- [ ] **Step 1: Append layout styles to `styles.css`**

Append to `styles.css`:

```css
/* ============================================================
   Layout
   ============================================================ */

.nav,
.hero,
.partners,
.site-footer {
  max-width: var(--max-w);
  margin-left: auto;
  margin-right: auto;
  padding-left: var(--pad-x);
  padding-right: var(--pad-x);
}

.divider {
  max-width: var(--max-w);
  margin: 0 auto;
  border: none;
  border-top: 1px solid var(--divider);
}

/* Nav */
.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 24px;
  padding-bottom: 24px;
}

/* Hero */
.hero {
  position: relative;
  padding-top: 70px;
  padding-bottom: 90px;
  overflow: hidden;
}

.hero h1 {
  font-size: 44px;
  line-height: 1.05;
  letter-spacing: -0.02em;
  font-weight: 500;
  max-width: 520px;
  margin: 0 0 24px;
}

.hero .lede {
  font-size: 16px;
  line-height: 1.6;
  max-width: 480px;
  color: var(--fg-muted);
}

/* Partners */
.partners {
  display: grid;
  grid-template-columns: 1fr 1fr;
  padding-left: 0;
  padding-right: 0;
}

.person {
  padding: 56px var(--pad-x);
}

.person + .person {
  border-left: 1px solid var(--divider);
}

.person h2 {
  font-size: 22px;
  font-weight: 500;
  letter-spacing: -0.01em;
  margin-bottom: 16px;
}

.person .bio {
  font-size: 14px;
  line-height: 1.6;
  color: var(--fg-muted);
}

/* Footer */
.site-footer {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 16px;
  padding-top: 36px;
  padding-bottom: 40px;
  border-top: 1px solid var(--divider);
  color: var(--fg-muted);
}

.site-footer .reg {
  display: flex;
  gap: 24px;
}
```

- [ ] **Step 2: Verify layout**

Run:

```bash
python3 -m http.server 8000 &
sleep 1
```

Open `http://localhost:8000/` in a browser at desktop width (≥1024px). Expected:

- Content is centered, capped at ~760px wide, with comfortable padding on both sides
- Nav strip shows "L2 Consulting AB" on the left and "EN" on the right, same baseline
- Hero h1 reads on two lines ("Two trades." / "One company."), large but not overflowing
- The lede paragraph is narrower than the headline and softer in color
- Erik and Stina sections sit side-by-side as two equal columns with a thin vertical divider between them
- Footer is a single row with email left, "L2 Consulting AB" + org.nr right

Stop the server:

```bash
kill %1 2>/dev/null
```

- [ ] **Step 3: Commit**

Run:

```bash
git add styles.css
git commit -m "feat: lay out nav, hero, partners, and footer"
```

---

### Task 5: Visual polish — accent disc, eyebrow, role labels, tag list

**Files:**
- Modify: `styles.css` (append component styles)

- [ ] **Step 1: Append component styles to `styles.css`**

Append to `styles.css`:

```css
/* ============================================================
   Components / details
   ============================================================ */

/* Small uppercase label utility */
.eyebrow,
.role,
.nav,
.site-footer {
  font-size: 11px;
  letter-spacing: 0.18em;
  text-transform: uppercase;
}

.nav .brand { font-weight: 600; letter-spacing: 0.18em; }
.nav .lang { color: var(--fg-faint); }

.eyebrow {
  color: var(--fg-faint);
  margin-bottom: 22px;
}

.role {
  font-size: 10px;
  letter-spacing: 0.22em;
  color: var(--fg-faint);
  margin-bottom: 14px;
}

/* Accent disc behind the hero */
.disc {
  position: absolute;
  right: -120px;
  top: -120px;
  width: 360px;
  height: 360px;
  border-radius: 50%;
  background: radial-gradient(circle at 30% 30%, var(--accent-from), var(--accent-to));
  opacity: 0.45;
  pointer-events: none;
  z-index: 0;
}

.hero > *:not(.disc) {
  position: relative;
  z-index: 1;
}

/* Tag list inside each partner section */
.tags {
  font-size: 12px;
  letter-spacing: 0.06em;
  color: var(--fg-muted);
  margin-bottom: 18px;
}

.tags li {
  padding: 4px 0;
  border-bottom: 1px dotted var(--divider-soft);
}

.site-footer .contact {
  font-weight: 500;
  letter-spacing: 0.06em;
}
```

- [ ] **Step 2: Verify polish**

Run:

```bash
python3 -m http.server 8000 &
sleep 1
```

Open `http://localhost:8000/` in a browser. Expected:

- A soft warm gold/ochre disc sits in the top-right corner of the hero, partially off-screen, with the headline reading clearly on top of it
- The eyebrow ("Älvkarleby, Sweden") sits above the headline in small uppercase letters, muted color, wider letter-spacing
- "Partner" appears above each name in the same small uppercase treatment, even smaller and more muted
- Each tag list ("IT & software" / "Construction" and "Outdoor teaching" / "Floristry") shows each tag on its own line with a faint dotted underline
- Brand wordmark in nav is uppercase tracked; "EN" is faint
- Footer is uppercase tracked; the email reads slightly bolder than the registration text on the right

Stop the server:

```bash
kill %1 2>/dev/null
```

- [ ] **Step 3: Commit**

Run:

```bash
git add styles.css
git commit -m "feat: add accent disc and typographic detail polish"
```

---

### Task 6: Responsive behavior — single breakpoint at 640px

**Files:**
- Modify: `styles.css` (append media query)

- [ ] **Step 1: Append the breakpoint rules to `styles.css`**

Append to `styles.css`:

```css
/* ============================================================
   Responsive — single breakpoint at 640px
   ============================================================ */

@media (max-width: 640px) {
  .nav,
  .hero,
  .person,
  .site-footer {
    padding-left: var(--pad-x-mobile);
    padding-right: var(--pad-x-mobile);
  }

  .divider {
    margin-left: var(--pad-x-mobile);
    margin-right: var(--pad-x-mobile);
  }

  .hero {
    padding-top: 48px;
    padding-bottom: 64px;
  }

  .hero h1 {
    font-size: 32px;
  }

  .partners {
    grid-template-columns: 1fr;
  }

  .person + .person {
    border-left: none;
    border-top: 1px solid var(--divider);
  }

  .disc {
    width: 260px;
    height: 260px;
    right: -100px;
    top: -100px;
  }

  .site-footer {
    flex-direction: column;
    align-items: flex-start;
    gap: 12px;
  }

  .site-footer .reg {
    flex-direction: column;
    gap: 4px;
  }
}
```

- [ ] **Step 2: Verify responsive behavior**

Run:

```bash
python3 -m http.server 8000 &
sleep 1
```

Open `http://localhost:8000/` and resize the browser window slowly from wide to narrow (or use device emulation in DevTools). Expected:

- Around 640px width, the partner columns stack vertically — Erik on top, Stina below, separated by a thin horizontal line
- Hero headline shrinks from 44px to 32px
- Horizontal padding tightens to 24px so content has breathing room on small screens
- Footer items stack vertically (email on top, registration block below)
- Disc shrinks proportionally and stays in the top-right corner
- No horizontal scrollbar appears at any width down to 320px

Stop the server:

```bash
kill %1 2>/dev/null
```

- [ ] **Step 3: Commit**

Run:

```bash
git add styles.css
git commit -m "feat: stack layout and shrink hero at <640px"
```

---

### Task 7: Favicon and final metadata

**Files:**
- Create: `favicon.svg`
- Modify: `index.html` (add favicon link)

- [ ] **Step 1: Create `favicon.svg`**

Write `favicon.svg`:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
  <rect width="32" height="32" rx="6" fill="#2b2620"/>
  <text x="16" y="21" font-family="Inter, system-ui, sans-serif" font-size="16" font-weight="500" text-anchor="middle" fill="#f4ede2">L2</text>
</svg>
```

- [ ] **Step 2: Link the favicon from `index.html`**

In `index.html`, inside `<head>`, add this line directly above `<link rel="stylesheet" href="styles.css">`:

```html
  <link rel="icon" type="image/svg+xml" href="favicon.svg">
```

- [ ] **Step 3: Verify favicon appears**

Run:

```bash
python3 -m http.server 8000 &
sleep 1
```

Open `http://localhost:8000/` in a browser. Expected: the browser tab shows a small dark "L2" tile favicon. (Chrome and Firefox both honor SVG favicons; if you only see a generic globe icon, hard-reload with Cmd-Shift-R.)

Also visit `http://localhost:8000/favicon.svg` directly — expected: the SVG renders as a dark rounded square with cream-colored "L2" text.

Stop the server:

```bash
kill %1 2>/dev/null
```

- [ ] **Step 4: Commit**

Run:

```bash
git add favicon.svg index.html
git commit -m "feat: add L2 monogram SVG favicon"
```

---

### Task 8: Final verification — accessibility, validity, content audit

**Files:** (no edits — verification only, unless issues are found)

- [ ] **Step 1: Validate HTML**

Run:

```bash
npx --yes html-validate index.html
```

Expected: no errors. If errors appear, fix the markup and re-run before continuing.

If `npx html-validate` is not available or refuses to run offline, paste the contents of `index.html` into https://validator.w3.org/nu/#textarea and confirm zero errors there.

- [ ] **Step 2: Verify color contrast**

The body text uses `#2b2620` on `#f4ede2`. Compute the contrast ratio:

Run:

```bash
python3 - <<'PY'
def luminance(hex):
    r, g, b = (int(hex[i:i+2], 16)/255 for i in (1, 3, 5))
    def adj(c): return c/12.92 if c <= 0.03928 else ((c+0.055)/1.055)**2.4
    R, G, B = adj(r), adj(g), adj(b)
    return 0.2126*R + 0.7152*G + 0.0722*B

L1 = luminance('#2b2620')
L2 = luminance('#f4ede2')
ratio = (max(L1, L2) + 0.05) / (min(L1, L2) + 0.05)
print(f"Contrast ratio: {ratio:.2f}:1")
print(f"Body text (AA needs ≥4.5): {'PASS' if ratio >= 4.5 else 'FAIL'}")
print(f"Large text (AA needs ≥3.0): {'PASS' if ratio >= 3.0 else 'FAIL'}")
PY
```

Expected: ratio ≥ 4.5, body text passes AA.

Also spot-check the muted text colors (`--fg-muted` at 72% opacity over cream and `--fg-faint` at 55%). The faint label text only needs to pass AA Large (≥3.0) since it is small but presentational, not core content. If either falls below threshold, increase opacity in `:root` and re-test.

- [ ] **Step 3: Content audit**

Open the deployed page locally one more time:

```bash
python3 -m http.server 8000 &
sleep 1
```

Open in a browser. Read every word of the page out loud. Verify:

- Company name spelled "L2 Consulting AB" everywhere (no "L2C", "L2 Consulting", etc.)
- Org.nr is exactly `556992-2155`
- Email is exactly `kontakt@l2c.se` and clicking it opens the mail client
- Both partner names are spelled correctly: Erik Lindblad, Stina Lindblad
- City: Älvkarleby (with the Ä)
- The bios match the spec verbatim
- No placeholder text or lorem ipsum slipped through

Stop the server:

```bash
kill %1 2>/dev/null
```

- [ ] **Step 4: Cross-browser quick check**

Open the local server in two browsers (e.g., Safari + Chrome, or Firefox + Chrome). Verify the page looks essentially identical: layout doesn't shift, the disc renders, the font loads, the responsive breakpoint behaves the same way.

- [ ] **Step 5: Commit any fixes**

If Steps 1–4 surfaced any issues that required code changes, commit them now:

```bash
git add -A
git commit -m "fix: address validation and accessibility findings"
```

If nothing needed fixing, skip this step.

- [ ] **Step 6: Tag a release**

Run:

```bash
git tag -a v1.0 -m "Initial site"
git log --oneline
```

Expected: a clean commit history showing each task as its own commit, with `v1.0` on the latest commit.

---

## Post-implementation (owner tasks, not coded)

1. Create a public GitHub repo (e.g., `l2c-site` under the appropriate account).
2. `git remote add origin <url>` and `git push -u origin main --tags`.
3. Repo Settings → Pages → Source: `Deploy from a branch`, Branch: `main`, Folder: `/ (root)`.
4. The repo's `CNAME` file already points GitHub at `l2c.se` — confirm it's detected in the Pages settings.
5. Configure DNS on the `l2c.se` registrar:
   - A records on apex → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - CNAME on `www` → `<github-username>.github.io`
6. Wait for the GitHub Pages HTTPS certificate to be issued (often <15 minutes; can take longer).
7. Enable **Enforce HTTPS** in Pages settings once the certificate is ready.
8. Verify `https://l2c.se` loads the site and `http://l2c.se` redirects to HTTPS.
