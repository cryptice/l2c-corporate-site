# Swedish Version + Language Switcher Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a Swedish translation of the L2C site and a top-right `EN | SV` switcher that toggles the visible language without leaving the page.

**Architecture:** Single `index.html` contains both languages: every translatable string is wrapped in a pair of inline `<span lang="en">` and `<span lang="sv">` elements. CSS hides the spans for the inactive language based on `<html lang="…">`. A ~20-line inline script swaps `<html lang>`, `<title>`, and `<meta description>` when a switcher button is clicked.

**Tech Stack:** HTML5, CSS3, vanilla JavaScript (no dependencies).

**Spec:** `/Users/erik/development/l2c-site/docs/superpowers/specs/2026-05-16-l2c-i18n-switcher-design.md`

**Note on TDD:** This is a static HTML/CSS/JS site with no test harness. Each task uses **verification-based discipline**: explicit content + behavior checks after every change. Treat the verification steps as non-negotiable.

---

## File Structure

- `index.html` — modified: every translatable string wrapped in paired language spans; existing `<span class="lang">EN</span>` replaced with the switcher; one inline `<script>` added at the bottom of `<body>`
- `styles.css` — modified: append two rule blocks (language visibility, switcher styles)
- No new files

---

### Task 1: Translate content (HTML pairs + CSS visibility)

**Files:**
- Modify: `/Users/erik/development/l2c-site/index.html`
- Modify: `/Users/erik/development/l2c-site/styles.css`

After this task the page still renders identical English to the visitor, but the Swedish translation is in the source and hidden via CSS. The switcher does not exist yet.

- [ ] **Step 1: Add the language visibility rule to `styles.css`**

Append this block to the end of `/Users/erik/development/l2c-site/styles.css`:

```css
/* ============================================================
   Language visibility — paired with the i18n switcher
   ============================================================ */

:root[lang="en"] [lang="sv"],
:root[lang="sv"] [lang="en"] {
  display: none;
}
```

- [ ] **Step 2: Replace the body of `index.html` content with paired-span versions**

The HTML changes touch the hero, the two partner sections, and the meta description default. The full new `<body>` content is below — replace the existing `<body>...</body>` with this exactly. The `<head>` (nav links, title, viewport, etc.) is unchanged in this step.

Here is the exact new `<body>` (note: the nav still has the original `<span class="lang">EN</span>` — that gets replaced in Task 2):

```html
<body>
  <header class="nav">
    <span class="brand">L2 Consulting AB</span>
    <span class="lang">EN</span>
  </header>

  <main>
    <section class="hero">
      <p class="eyebrow">
        <span lang="en">Älvkarleby, Sweden</span>
        <span lang="sv">Älvkarleby, Sverige</span>
      </p>
      <h1>
        <span lang="en">Two trades.<br>One company.</span>
        <span lang="sv">Två yrken.<br>Ett företag.</span>
      </h1>
      <p class="lede">
        <span lang="en">L2 Consulting AB is a small Swedish consultancy run by two people. We bring together work that does not usually share a roof — from IT and construction to outdoor education and floristry.</span>
        <span lang="sv">L2 Consulting AB är ett litet svenskt konsultföretag som drivs av två personer. Vi samlar arbete som vanligtvis inte delar tak — från IT och bygg till utomhuspedagogik och floristik.</span>
      </p>
      <div class="disc" aria-hidden="true"></div>
    </section>

    <hr class="divider">

    <section class="partners">
      <article class="person">
        <p class="role">
          <span lang="en">Partner</span>
          <span lang="sv">Partner</span>
        </p>
        <h2>Erik Lindblad</h2>
        <ul class="tags">
          <li><span lang="en">IT &amp; software</span><span lang="sv">IT &amp; mjukvara</span></li>
          <li><span lang="en">Construction</span><span lang="sv">Bygg</span></li>
        </ul>
        <p class="bio">
          <span lang="en">Works across software systems and the built environment — from web platforms and developer tooling to practical construction projects.</span>
          <span lang="sv">Arbetar med både mjukvarusystem och bebyggd miljö — från webbplattformar och utvecklarverktyg till praktiska byggprojekt.</span>
        </p>
      </article>

      <article class="person">
        <p class="role">
          <span lang="en">Partner</span>
          <span lang="sv">Partner</span>
        </p>
        <h2>Stina Lindblad</h2>
        <ul class="tags">
          <li><span lang="en">Outdoor teaching</span><span lang="sv">Utomhuspedagogik</span></li>
          <li><span lang="en">Floristry</span><span lang="sv">Floristik</span></li>
        </ul>
        <p class="bio">
          <span lang="en">Teaches outdoors and works as a florist — combining education in nature with the craft of flowers and seasonal arrangements.</span>
          <span lang="sv">Undervisar utomhus och arbetar som florist — förenar utbildning i naturen med blomsterhantverk och säsongsbetonade arrangemang.</span>
        </p>
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
```

- [ ] **Step 3: Verify the page still renders correctly in English**

Run from `/Users/erik/development/l2c-site`:

```bash
python3 -m http.server 8000 &
sleep 1
curl -s http://localhost:8000/ | grep -E "Två yrken|Älvkarleby, Sverige|Bygg|Utomhuspedagogik" | wc -l
```

Expected: a number ≥ 4 — the Swedish strings are present in the source.

Then open `http://localhost:8000/` in a browser. Expected:

- Page looks identical to the previous (pre-Task 1) version: English headlines, English eyebrow, English bios
- No Swedish text is visible anywhere
- Layout is unchanged: hero, two partner columns, footer
- The nav still shows the old `EN` indicator on the right (the switcher comes in Task 2)

Then stop the server:

```bash
kill %1 2>/dev/null
```

- [ ] **Step 4: Quick validate HTML**

Run:

```bash
cd /Users/erik/development/l2c-site
npx --yes html-validate index.html
```

Expected: no errors. If errors appear, fix the markup before continuing.

- [ ] **Step 5: Commit**

```bash
cd /Users/erik/development/l2c-site
git add index.html styles.css
git commit -m "feat: add Swedish translations alongside English (hidden via CSS)"
```

---

### Task 2: Switcher markup, styles, and toggle script

**Files:**
- Modify: `/Users/erik/development/l2c-site/index.html` (replace the `<span class="lang">EN</span>` in the nav; add inline `<script>` at end of `<body>`)
- Modify: `/Users/erik/development/l2c-site/styles.css` (append switcher styles)

After this task the visitor can click `EN` or `SV` to swap the page content and the `<title>` / `<meta description>`.

- [ ] **Step 1: Replace the nav language indicator with the switcher**

In `/Users/erik/development/l2c-site/index.html`, find this line in the `<header class="nav">` block:

```html
    <span class="lang">EN</span>
```

Replace it with:

```html
    <nav class="lang-switcher" aria-label="Language">
      <button type="button" data-lang="en" aria-pressed="true">EN</button>
      <span class="lang-sep" aria-hidden="true">|</span>
      <button type="button" data-lang="sv" aria-pressed="false">SV</button>
    </nav>
```

- [ ] **Step 2: Append switcher styles to `styles.css`**

Append this block to the end of `/Users/erik/development/l2c-site/styles.css`:

```css
/* ============================================================
   Language switcher in the nav
   ============================================================ */

.lang-switcher {
  display: inline-flex;
  align-items: center;
  gap: 8px;
}

.lang-switcher button {
  background: none;
  border: none;
  padding: 0;
  margin: 0;
  font: inherit;
  letter-spacing: inherit;
  text-transform: inherit;
  color: var(--fg-faint);
  cursor: pointer;
}

.lang-switcher button[aria-pressed="true"] {
  color: var(--fg);
  cursor: default;
}

.lang-switcher button:focus-visible {
  outline: 2px solid var(--fg);
  outline-offset: 3px;
  border-radius: 1px;
}

.lang-sep {
  color: var(--fg-faint);
}
```

- [ ] **Step 3: Add the toggle script at the end of `<body>`**

In `/Users/erik/development/l2c-site/index.html`, locate the closing `</body>` tag and insert this `<script>` block immediately BEFORE it (so the script is the last child of `<body>`):

```html
  <script>
    (function () {
      const root = document.documentElement;
      const buttons = document.querySelectorAll('.lang-switcher button');
      const titles = {
        en: 'L2 Consulting AB',
        sv: 'L2 Consulting AB'
      };
      const descs = {
        en: 'L2 Consulting AB is a small Swedish consultancy in Älvkarleby — IT, construction, outdoor teaching, and floristry.',
        sv: 'L2 Consulting AB är ett litet svenskt konsultföretag i Älvkarleby — IT, bygg, utomhuspedagogik och floristik.'
      };
      const descMeta = document.querySelector('meta[name="description"]');

      buttons.forEach(function (btn) {
        btn.addEventListener('click', function () {
          const lang = btn.dataset.lang;
          if (root.getAttribute('lang') === lang) return;
          root.setAttribute('lang', lang);
          document.title = titles[lang];
          if (descMeta) descMeta.setAttribute('content', descs[lang]);
          buttons.forEach(function (b) {
            b.setAttribute('aria-pressed', b.dataset.lang === lang ? 'true' : 'false');
          });
        });
      });
    })();
  </script>
```

- [ ] **Step 4: Verify in the browser**

Run from `/Users/erik/development/l2c-site`:

```bash
python3 -m http.server 8000 &
sleep 1
```

Open `http://localhost:8000/` in a browser. Verify:

1. The top-right of the nav shows `EN | SV`. `EN` is in the main foreground color, `SV` is faded, with a slim `|` between them.
2. Tab key reaches the `EN` and `SV` buttons; a visible focus ring appears around the focused button.
3. Click `SV`. Expected within ~1 frame:
   - Every visible string becomes Swedish (`Älvkarleby, Sverige`, `Två yrken. Ett företag.`, `Bygg`, `Utomhuspedagogik`, the bios, etc.)
   - The brand wordmark stays `L2 Consulting AB`, footer stays `L2 Consulting AB` and `Org.nr 556992-2155`, the email link stays `kontakt@l2c.se`
   - The browser tab title remains `L2 Consulting AB`
   - `SV` is now the bold foreground; `EN` is faded
4. Click `EN`. Page swaps back to English. `EN` bold, `SV` faded.
5. Open DevTools → Elements. Inspect the `<meta name="description">` content after switching to Swedish — expected to read the Swedish translation. Switch back to English — expected to read the English translation.
6. Open DevTools → Settings (gear) → Debugger → tick "Disable JavaScript". Reload the page. Expected: site loads in English. Nav shows `EN | SV` but clicks do nothing (no JS). No Swedish text is visible anywhere. Re-enable JS and reload before continuing.

Stop the server:

```bash
kill %1 2>/dev/null
```

- [ ] **Step 5: Validate HTML**

Run:

```bash
cd /Users/erik/development/l2c-site
npx --yes html-validate index.html
```

Expected: no errors.

- [ ] **Step 6: Commit**

```bash
cd /Users/erik/development/l2c-site
git add index.html styles.css
git commit -m "feat: add EN | SV language switcher"
```

- [ ] **Step 7: Push and let GitHub Pages redeploy**

```bash
cd /Users/erik/development/l2c-site
git push
```

After a successful Pages build (visible in repo Actions), open `https://l2c.se` in a real browser and re-run the click test from Step 4. Expected: same behavior as locally.
