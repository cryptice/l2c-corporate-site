# L2C Site — Swedish Version + Language Switcher Design

**Date:** 2026-05-16
**Status:** Approved (pending user review of this doc)

## Goal

Add a Swedish translation of the L2 Consulting AB site, and a switcher in the top-right of the nav so visitors can toggle between English and Swedish without leaving the page.

## Scope

In scope:

- Translate every visible string into Swedish
- One HTML file with both languages embedded
- Top-right nav switcher showing `EN | SV` — current language emphasized, inactive faded
- English is the default on every visit (no preference memory)
- JS swaps which language is visible by changing `<html lang="...">`
- Update `<title>` and `<meta name="description">` on switch as well

Out of scope:

- Server-side language detection or routing
- Separate URLs per language (`/sv/`, `?lang=sv`)
- Persisting the user's choice between visits (localStorage, cookies)
- Browser-locale auto-detection
- A third language

## Design decisions and trade-offs

**This change deliberately introduces JavaScript** to the site, breaking the original "no JS" constraint. The trade-off is acceptable because:
- The site continues to function without JS — English-only, since CSS hides Swedish elements by default. No content is lost.
- Avoiding JS would require either separate URLs (more files, more places to keep in sync) or a server with content negotiation (overkill for this site).
- The script is ~15 lines, all in one place, easy to read and maintain.

**Default English with no memory** keeps the first-visit experience predictable for the international audience the English version is aimed at. A returning Swedish visitor pays a one-click cost; that's acceptable for the simplicity gain.

## Translations

| Element | English | Swedish |
|---|---|---|
| `<title>` | `L2 Consulting AB` | `L2 Consulting AB` (unchanged — proper noun) |
| `<meta description>` | "L2 Consulting AB is a small Swedish consultancy in Älvkarleby — IT, construction, outdoor teaching, and floristry." | "L2 Consulting AB är ett litet svenskt konsultföretag i Älvkarleby — IT, bygg, utomhuspedagogik och floristik." |
| Hero eyebrow | `Älvkarleby, Sweden` | `Älvkarleby, Sverige` |
| Hero headline | `Two trades.<br>One company.` | `Två yrken.<br>Ett företag.` |
| Hero lede | "L2 Consulting AB is a small Swedish consultancy run by two people. We bring together work that does not usually share a roof — from IT and construction to outdoor education and floristry." | "L2 Consulting AB är ett litet svenskt konsultföretag som drivs av två personer. Vi samlar arbete som vanligtvis inte delar tak — från IT och bygg till utomhuspedagogik och floristik." |
| Role label | `Partner` | `Partner` (unchanged — used in Swedish business too) |
| Erik tag 1 | `IT & software` | `IT & mjukvara` |
| Erik tag 2 | `Construction` | `Bygg` |
| Stina tag 1 | `Outdoor teaching` | `Utomhuspedagogik` |
| Stina tag 2 | `Floristry` | `Floristik` |
| Erik bio | "Works across software systems and the built environment — from web platforms and developer tooling to practical construction projects." | "Arbetar med både mjukvarusystem och bebyggd miljö — från webbplattformar och utvecklarverktyg till praktiska byggprojekt." |
| Stina bio | "Teaches outdoors and works as a florist — combining education in nature with the craft of flowers and seasonal arrangements." | "Undervisar utomhus och arbetar som florist — förenar utbildning i naturen med blomsterhantverk och säsongsbetonade arrangemang." |
| Footer email | `kontakt@l2c.se` | unchanged |
| Footer reg. | `Org.nr 556992-2155` | unchanged |

## Technical design

### HTML structure

Each translatable string is wrapped in a pair of inline spans:

```html
<p class="eyebrow">
  <span lang="en">Älvkarleby, Sweden</span>
  <span lang="sv">Älvkarleby, Sverige</span>
</p>
```

For elements where the inner markup matters (e.g., the headline with `<br>`), both spans contain their own copy of the inline markup:

```html
<h1>
  <span lang="en">Two trades.<br>One company.</span>
  <span lang="sv">Två yrken.<br>Ett företag.</span>
</h1>
```

The `<html>` element starts as `<html lang="en">` (already correct). JS changes the attribute to `sv` when the user picks Swedish.

### Switcher markup

The current single `<span class="lang">EN</span>` in the nav is replaced with:

```html
<nav class="lang-switcher" aria-label="Language">
  <button type="button" data-lang="en" aria-pressed="true">EN</button>
  <span class="lang-sep" aria-hidden="true">|</span>
  <button type="button" data-lang="sv" aria-pressed="false">SV</button>
</nav>
```

`<button>` is the right semantic — these trigger script behavior, not navigation. `aria-pressed` tells screen readers which language is active.

### CSS for visibility

A rule added in styles.css:

```css
:root[lang="en"] [lang="sv"],
:root[lang="sv"] [lang="en"] {
  display: none;
}
```

This hides whichever language is *not* currently selected. With no JS, English remains visible by default (since `<html lang="en">` is set in source) and the Swedish spans are hidden.

### Switcher styles

The switcher uses the existing nav typography (small uppercase letters via the existing `.nav` rule), with `--fg-faint` for the inactive button and `--fg` for the active one. Buttons get `background: none; border: none; padding: 0; color: inherit; font: inherit; cursor: pointer;` to look like plain text. The `lang-sep` keeps the `--fg-faint` color always.

### JavaScript

A small inline `<script>` at the bottom of `<body>`:

```html
<script>
  (function () {
    const root = document.documentElement;
    const buttons = document.querySelectorAll('.lang-switcher button');
    const titles = { en: 'L2 Consulting AB', sv: 'L2 Consulting AB' };
    const descs = {
      en: 'L2 Consulting AB is a small Swedish consultancy in Älvkarleby — IT, construction, outdoor teaching, and floristry.',
      sv: 'L2 Consulting AB är ett litet svenskt konsultföretag i Älvkarleby — IT, bygg, utomhuspedagogik och floristik.'
    };
    const descMeta = document.querySelector('meta[name="description"]');

    buttons.forEach(function (btn) {
      btn.addEventListener('click', function () {
        const lang = btn.dataset.lang;
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

That's the entire script: ~20 lines, no dependencies, no module system.

### Why not `<noscript>`?

A `<noscript>` notice ("Switch to Swedish requires JavaScript") is unnecessary because the no-JS experience is identical to the default (English) and visitors who can't switch are not blocked — they just see the English version, which is what most international visitors expect anyway.

### Accessibility

- `<button>` elements are keyboard-focusable; pressing Enter or Space activates them
- `aria-pressed` correctly conveys toggle state
- Changing `<html lang>` lets screen readers switch pronunciation
- The hidden language uses `display: none` so screen readers skip it (it doesn't get read in the wrong pronunciation)
- The `|` separator has `aria-hidden="true"` so it isn't announced

### File impact

- `index.html`: extended with the paired language spans, the new switcher markup, and the inline script
- `styles.css`: adds the `:root[lang]` hiding rules and the `.lang-switcher` styles
- No new files

## Verification

- HTML still validates after changes
- Page loads showing English with no JS
- Clicking SV swaps every visible string to Swedish, including the page title (visible in browser tab) and `<meta description>` (visible via DevTools)
- Clicking EN swaps back
- Keyboard tab to the switcher buttons works; Enter activates
- Color contrast unchanged (palette did not move)
