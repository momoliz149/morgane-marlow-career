# Direction A — "Discipline the Olive"

Implementation brief for `index.html`. Single file, no build step, no framework.

**Goal:** keep the existing visual identity and give it a hierarchy. This is mostly subtraction — two colours deleted, one typographic rule enforced, the placeholders resolved. No section is redesigned and no section is reordered.

**Scope:** punch list items 1–6 only. Do not reorder sections, do not rewrite the offers, do not add a contact form. Those are a later pass.

---

## Ground rules

1. **Do not introduce any colour that isn't in the token block in Step 1.** No new greys, no `#888`, no `rgba(0,0,0,…)` for text. Every colour comes from a token.
2. **`Relationship of Melodrame` may not be set below 44px, anywhere.** This is the single most important rule in this brief.
3. **Do not add a CSS framework, a build step, or a JS dependency.** Everything stays in the one file.
4. Preserve the existing lowercase voice in UI (nav, buttons, labels). Restore capitals on proper nouns and acronyms only — see Step 7.
5. Keep every existing `id` and anchor working. (`#about` and `#career` are mislabelled; that's punch list item 11 and is **out of scope here** — leave them as they are.)

---

## Step 1 — Replace the token block

Replace the entire `:root { … }` block with this. Every value has been checked against the ground it sits on; the ratio in the comment is the measured WCAG 2.1 contrast.

```css
:root {
  /* ── Grounds — there are exactly two ── */
  --ground:              #F8FFED;   /* the one light */
  --ground-dark:         #2B2D00;   /* the one dark  */

  /* ── Ink on light ── */
  --ink:                 #2B2D00;   /* 13.88:1 — body, headings   */
  --ink-mid:             #585B0A;   /*  7.04:1 — secondary, links */
  --ink-muted:           #72755B;   /*  4.65:1 — labels, metadata */

  /* ── Ink on dark ── */
  --ink-on-dark:         #F8FFED;   /* 13.88:1 */
  --ink-on-dark-muted:   #AAAF93;   /*  6.27:1 */

  /* ── Accent — one colour, one job ── */
  --accent:              #D3D858;
  --accent-ink:          #3F4207;   /*  6.89:1 on --accent */

  /* ── The thread — peony, used on ONE element (see Step 2) ── */
  --thread:              #A65E82;   /*  4.51:1 on --ground      */
  --thread-on-dark:      #B5859D;   /*  4.61:1 on --ground-dark */

  /* ── Surfaces & rules ── */
  --surface-on-dark:     #56590A;
  --rule:                rgba(43, 45, 0, .16);
  --rule-on-dark:        #74780D;   /*  3.00:1 vs --ground-dark */

  /* ── Type ── */
  --font-display: 'Relationship of Melodrame', Georgia, serif;
  --font-body:    'Neue Montreal', system-ui, sans-serif;

  /* ── Geometry ── */
  --radius-pill: 100px;
  --radius-card: 16px;

  --section-padding-y: 128px;
  --section-padding-x: 80px;
  --container-max:     1200px;
}
```

**Deleted tokens** — remove them and every reference:

| Removed | Replace every usage with |
|---|---|
| `--color-honeydew` | `--ground` |
| `--color-olive-dark` | `--ground-dark` (as a ground) or `--ink` (as text) |
| `--color-olive-leaf` | `--ink-mid` |
| `--color-lemon-lime` | `--accent` |
| `--color-cream` | `--ground` — cream and honeydew are 3% apart; the distinction reads as a bug, not a section break |
| `--color-icy-blue` | `--ground-dark` on the "work with me" section; nowhere else |
| `--color-sweet-peony` | `--thread`, on one element only |

Also replace the five hard-coded greys — `#aaa`, `#999`, `#888`, `#555`, `#fff` used as text — with tokens. `#aaa`, `#999` and `#888` all become `--ink-muted`.

---

## Step 2 — Section grounds

The page currently changes hue at every section, so no colour reads as *the* colour. New rule: **everything is light except one dark block.** Sections are separated by a hairline `--rule`, not by a change of hue.

| Section | Ground | Notes |
|---|---|---|
| `.site-nav` | `--ground` | scrolled state: `rgba(248,255,237,.88)` + existing blur |
| `.hero` | `--ground` | **changed from olive** — see Step 5 |
| `.about-career` | `--ground` | add `border-top: 1px solid var(--rule)` |
| `.competencies` | `--ground` | **changed from cream** — add `border-top: 1px solid var(--rule)` |
| `.work-with-me` | `--ground-dark` | **changed from ice blue** — this is the one dark block |
| `.ai-central` | `--ground` | **changed from olive-dark** — add `border-top: 1px solid var(--rule)` |
| `footer` | `--ground` | |
| `.footer__strip` | `--ground-dark` | text `--ink-on-dark-muted` |

**The one dark block is "work with me" on purpose.** It is the section that has to earn the enquiry and it currently has the least visual weight on the page.

### Inside the dark block

```css
.work-with-me            { background: var(--ground-dark); }
.work-header .text-label { color: var(--ink-on-dark-muted); }
.work-header .text-h2    { color: var(--ink-on-dark); }
.work-header .text-body  { color: var(--ink-on-dark-muted); }

.work-tile        { background: var(--surface-on-dark);
                    border: 1px solid var(--rule-on-dark);
                    border-radius: var(--radius-card); }
.work-tile:hover  { border-color: var(--accent); transform: translateY(-2px); }
.work-tile__title { color: var(--ink-on-dark); }
.work-tile__desc  { color: var(--ink-on-dark-muted); }
.work-tile__cta   { color: var(--accent); }
.work-footer-note { color: var(--ink-on-dark-muted); }
```

`.btn-outline-white` is used inside this section — rename it `.btn-on-dark` and set border and text to `--ink-on-dark`. It is also currently used in `.ai-central`, which is now light: swap that one to `.btn-secondary`.

### The AI Central stat cards (now on light)

```css
.stat-card         { background: var(--ink-mid); border-radius: var(--radius-card); }
.stat-card__number { color: var(--accent); }      /* 4.71:1 */
.stat-card__label  { color: var(--ground); }      /* 7.04:1 */
```

Section text: `.text-label` → `--ink-muted`, `.text-h3` → `--ink`, `.text-body` → `--ink-mid`.

### One nav treatment, not four

The nav currently carries four different pill styles across four links plus a filled button — an outline pill, an ice-blue pill, a peony pill, and the accent button. Nothing in it is subordinate, and the loudest element is the peony pill, which is the only link that sends people *off* the site.

Collapse it to two treatments: plain text links, plus one accent button.

```css
.nav-link {
  font-family: var(--font-body);
  font-size: 13px;
  font-weight: 500;
  color: var(--ink-mid);
  padding: 6px 2px;
  border-bottom: 2px solid transparent;
  transition: color .18s ease, border-color .18s ease;
}
.nav-link:hover        { color: var(--ink); border-bottom-color: var(--rule); }
.nav-link--thread      { color: var(--ink); border-bottom-color: var(--thread); }
.nav-pills             { gap: 22px; }
```

Replace `class="nav-pill nav-pill--outline"` and `class="nav-pill nav-pill--blue"` with `class="nav-link"`, and `class="nav-pill nav-pill--green"` with `class="nav-link nav-link--thread"`. Then **delete** `.nav-pill`, `.nav-pill--outline`, `.nav-pill--blue` and `.nav-pill--green` from the stylesheet.

(`--green` was already a misnomer — it renders peony pink. It goes with the rest.)

The `get in touch →` button stays as the single `.btn-primary`, and `:hover` loses `transform: scale(1.04)` — scale on a text link is the kind of motion that reads as unfinished. Use the colour and border transitions above instead.

### The thread

`--thread` is spent on exactly one element: the **but, why?** link in the nav. Not a filled pill — a text link with a 2px `--thread` underline. It stops being decoration and starts being a deliberate tie to the newsletter.

```css
.nav-link--thread { color: var(--ink); border-bottom: 2px solid var(--thread); }
```

`--thread-on-dark` exists for the same link if it ever appears on the dark block. Do not use `--thread` as a background anywhere.

---

## Step 3 — The 44px floor

`Relationship of Melodrame` is a high-contrast condensed display italic. Its hairlines fall below one pixel under about 40px and the letterforms turn to mush. **It appears at 44px and above, or not at all.**

### Moves to `--font-body`

Every one of these currently uses `--font-display` below the floor:

| Selector | Current size | Action |
|---|---|---|
| `.text-h3` | clamp(22–32px) | `--font-body`, `font-weight: 500` |
| `.nav-logo` | 20px | `--font-body`, `font-weight: 500`, `letter-spacing: -.01em` |
| `.timeline-item__org` | 18px | `--font-body`, `font-weight: 500` |
| `.edu-item__school` | 16px | `--font-body`, `font-weight: 500` |
| `.comp-item__title` | 18px | `--font-body`, `font-weight: 500` |
| `.work-tile__title` | 22px | `--font-body`, `font-weight: 500` |
| `.work-footer-note` | 18px | `--font-body`, `font-weight: 400` |

### Stays on `--font-display`

| Selector | Change |
|---|---|
| `.text-display` / `.hero-title` | clamp(48px, 7vw, 88px) — unchanged, already above the floor |
| `.text-h2` | **raise** to `clamp(44px, 5vw, 60px)` — the 32px lower bound is below the floor |
| `.stat-card__number` | **raise** 42px → **48px** |
| `.footer-wordmark` | clamp(64px, 11vw, 140px) — unchanged |

### Three faults to delete

1. **Fake bold.** The face ships one weight. Every `font-weight: 700` on a `--font-display` element makes the browser smear the outline and thicken the hairlines unevenly. Set them all to `400`.
2. **Fake italic.** Delete this rule entirely:
   ```css
   em, i { font-style: oblique 12deg !important; }
   ```
   Style `em` with `color: var(--ink-mid)` instead, or leave it to the body font's real italic.
3. **The duplicate `@font-face`.** There are two, both pointing at the same `.woff2`, one declared `font-style: italic`. Delete the italic one — it is telling the browser a real italic exists when it doesn't.

Also change `font-display: block` to `font-display: swap` on the remaining `@font-face`. `block` hides the text for up to three seconds on a cold load.

### Type scale

Nine steps, 1.25 ratio. Do not improvise sizes between them:

```
96 · 64 · 44 · 28 · 22 · 18 · 16 · 14 · 12
```

44 is the display-face floor. 12 is labels only, with `letter-spacing: .12em`.

---

## Step 4 — Remove the global lowercase transform

Delete `text-transform: lowercase` from the `body` rule.

The transform is currently applied to the whole document and then overridden back to uppercase on headings, which means every proper noun and acronym on the page is lowercased — "kellogg mba", "coo of ai central media", "uk–us markets", "crm", "ai tooling". On acronyms this stops reading as a style and starts reading as a typo.

**Keep** `text-transform: lowercase` on these — the lowercase voice stays in the UI:

`.btn` · `.nav-pill` / nav links · `.text-label` · `.work-tile__cta` · `.footer__col-title` · `.footer__strip` · `.footer-tagline` · `.stat-card__label`

**Remove** `text-transform: lowercase` from these — they carry prose or proper nouns:

`.hero-tagline` · `.work-tile__desc` · `.comp-item__list` · `.timeline-item__role` · `.timeline-item__years` · `.edu-item__degree` · `.lang-item` · `.footer__col a`

**Update the uppercase block.** It currently reads:

```css
h1, h2, h3,
.text-display, .text-h2, .text-h3,
.stat-card__number, .footer-wordmark,
.work-tile__title, .timeline-item__org, .nav-logo {
  text-transform: uppercase;
}
```

Uppercase now applies **only to elements still using the display face**. Reduce it to:

```css
.text-display, .hero-title, .text-h2,
.stat-card__number, .footer-wordmark {
  text-transform: uppercase;
}
```

Everything removed from that list (`.text-h3`, `.work-tile__title`, `.timeline-item__org`, `.nav-logo`) now renders as authored in the HTML — which is why Step 7 matters.

---

## Step 5 — The hero

The hero moves from olive to `--ground` (light). Two reasons: it makes the page read as one colour family rather than a dark banner bolted onto a light site, and a transparent-background portrait cutout is far more forgiving on a light ground than on mid-olive.

```css
.hero                    { background: var(--ground);
                           padding: 100px var(--section-padding-x) 0; }
.hero-inner              { align-items: end; }
.hero__text              { padding-bottom: 100px; }
.hero__text .text-label  { color: var(--ink-muted); }
.hero-title              { color: var(--ink); }
.hero-tagline            { color: var(--ink-mid); }
```

The second hero CTA is currently `.btn-outline-white`, which is invisible on a light ground. Change it to `.btn-secondary`.

### The portrait

Replace the placeholder div with the real image:

```html
<div class="hero__photo reveal reveal-d2">
  <img src="/img/morgane-hero.png"
       alt="Morgane Marlow"
       width="880" height="1100"
       fetchpriority="high" decoding="async" />
</div>
```

```css
.hero__photo     { display: flex; justify-content: flex-end; align-items: flex-end; }
.hero__photo img { width: 100%; max-width: 440px; max-height: 560px;
                   height: auto; object-fit: contain; object-position: bottom;
                   border-radius: 0; }
```

`border-radius` goes to `0` — a cutout with a transparent background has no rectangular edge to round, and the current `20px 20px 0 0` was drawing a corner the image doesn't have. Bottom padding on `.hero` goes to `0` so the figure stands on the section edge rather than floating above it.

**Delete `.hero__photo-placeholder` and its CSS entirely.**

If the PNG has a hard rectangular edge rather than a transparent cutout, say so rather than shipping it — the treatment needs to change.

---

## Step 6 — Delete the photo strip

Remove the whole block:

```html
<!-- PHOTO STRIP -->
<div class="photo-strip" aria-label="photo gallery"> … </div>
```

And its CSS: `.photo-strip`, `.photo-strip-item`, `.photo-strip-placeholder`, plus the two `.photo-strip*` rules inside the `@media (max-width: 768px)` block.

Six flat colour blocks labelled *photo 1*–*photo 6* are the most damaging thing on the live site. Nothing replaces them in this pass; the section rhythm reads better without a stripe between the AI Central section and the footer.

---

## Step 7 — Copy: restore capitals

Apply these exact edits to the HTML. Everything not listed stays lowercase.

| Current | Change to |
|---|---|
| `kellogg mba. currently coo of ai central media. london-based, perpetually curious.` | `Kellogg MBA. currently COO of AI Central Media. London-based, perpetually curious.` |
| `…from financial services in scottsdale to the halls of whitehall, from hedge fund operations in chicago…` | `…Scottsdale…Whitehall…Chicago…` |
| `i left the us after 27 years to return to the uk` | `i left the US after 27 years to return to the UK` |
| `now i'm coo of ai central media` | `now i'm COO of AI Central Media` |
| `mba, strategy · northwestern university · 2020 – 2022` | `MBA, Strategy · Northwestern University · 2020 – 2022` |
| `bs · business administration · 2010 – 2014` | `BS · Business Administration · 2010 – 2014` |
| `uk–us expansion` | `UK–US expansion` |
| `uk–us markets · financial services · banking · fintech` | `UK–US markets · financial services · banking · fintech` |
| `crm & pipeline systems` | `CRM & pipeline systems` |
| `ai tooling & workflow design` | `AI tooling & workflow design` |
| `c-suite engagement` | `C-suite engagement` |
| `ai, media, operator strategy, building in public.` | `AI, media, operator strategy, building in public.` |
| `covering ai for business leaders and operators` | `covering AI for business leaders and operators` |
| `co-founded with alex fiore. seed-stage, london-based.` | `co-founded with Alex Fiore. seed-stage, London-based.` |
| `linkedin subscribers` (stat label) | `LinkedIn subscribers` |
| `ai central media ↗` (footer + nav) | `AI Central Media ↗` |
| `linkedin ↗` (footer) | `LinkedIn ↗` |
| `london — milan — always online` | `London — Milan — always online` |
| `coo & co-founder` (timeline role) | `COO & co-founder` |
| `2024 – present · london` | `2024 – present · London` |
| `2022 – 2025 · chicago` · `2018 – 2021 · chicago` | `…· Chicago` (both) |
| `2015 – 2018 · scottsdale` · `2014 – 2015 · scottsdale` | `…· Scottsdale` (both) |
| `english · native` / `french · …` / `italian · …` | `English` / `French` / `Italian` |
| `visit ai central media ↗` | keep lowercase — it's a button |

The `<title>` and the two `og:` tags carry no acronyms and need no change in this pass.

Note that the display-face headings that were being uppercased by CSS — `.text-h3`, `.work-tile__title`, `.timeline-item__org`, `.nav-logo` — now render as authored. Check that `AI Central Media`, `UK Government — DBT`, `Northern Trust`, `The Vanguard Group`, `Kellogg School of Management` and `University of Oregon` all read correctly in the markup, since the CSS is no longer masking their case.

---

## Step 8 — Fix the mobile nav

At 390px the wordmark wraps to two lines and the "get in touch" button runs over both its neighbours. The `@media (max-width: 768px)` block hides `.nav-pills` and reveals the toggle but never touches the button, which keeps its full desktop padding — three elements sharing about 342px inside a fixed 60px bar.

```css
/* base — replace the fixed height */
.site-nav  { min-height: 72px; height: auto; }
.nav-logo  { white-space: nowrap; }

@media (max-width: 768px) {
  .site-nav          { min-height: 60px; height: auto; padding: 0 24px; gap: 12px; }
  .nav-logo          { font-size: 16px; }
  .site-nav .btn     { font-size: 12px; padding: 9px 16px; }
}
```

Shorten the nav button label to **`email →`** below 768px, or drop the arrow. The full "get in touch →" does not fit.

Also make the toggle honest to assistive tech — it currently reports nothing about its state:

```html
<button class="nav-mobile-toggle" aria-label="Menu" aria-expanded="false" aria-controls="nav-pills">
```

Give `.nav-pills` `id="nav-pills"` and have `toggleMobileNav()` set `aria-expanded` to match. While you're in that function: it writes layout via `style.cssText`, which is fragile. Move the open state to a `.nav-pills--open` class and have the function toggle the class.

---

## Step 9 — Two motion safeguards

Neither is on the punch list, but both are one-liners and both are real:

```css
/* .reveal starts at opacity:0 — if the script fails, the page is blank */
.no-js .reveal { opacity: 1; transform: none; }

@media (prefers-reduced-motion: reduce) {
  .reveal { opacity: 1; transform: none; transition: none; }
  * { animation: none !important; transition: none !important; }
}
```

Add `class="no-js"` to `<html>` and strip it in the first line of the inline script.

---

## Verification

Before reporting done:

1. **Grep for stragglers.** `--color-`, `#aaa`, `#999`, `#888`, `#555`, `rgba(255,255,255`, `oblique`, `photo-strip`, `hero__photo-placeholder`, `nav-pill` should all return zero hits in `index.html`. `font-weight: 700` should return zero hits on any `--font-display` element.
2. **Confirm the 44px floor.** Every rule setting `font-family: var(--font-display)` must also set a size whose minimum is ≥ 44px. There should be exactly five such rules.
3. **Render at 1440×900 and 390×844.** In the mobile shot the wordmark must be on one line and nothing in the nav bar may overlap.
4. **Confirm no horizontal scroll at 390px**: `document.documentElement.scrollWidth === clientWidth`.
5. **Spot-check contrast** on the dark block — `--ink-on-dark-muted` on `--ground-dark` should measure 6.27:1, and `--accent` on `--surface-on-dark` should clear 3:1.
6. **Read the page once at 100%** and confirm no acronym is lowercase and no proper noun has lost its capital.

---

## Deliberately out of scope

Items 7–17 of the punch list, in case they come up while you're in the file — **leave them alone in this pass:**

writing the three offers · reordering sections client-first · cutting six competencies to three · replacing the emoji · the `#about`/`#career` anchor swap · the contact form · verifying the outbound domains · the `og:image` · renaming the font file · the custom domain · Person schema and the CV download.

---

## One variant, if you want it

The brief puts the single dark block on **work with me** and leaves AI Central light. If you'd rather keep AI Central as the dark moment — its stat cards did look strong on dark — swap the two grounds: `.ai-central` takes `--ground-dark` with the on-dark tokens, and `.work-with-me` takes `--ground` with the light ones. Everything else in this brief is unchanged. Only ever one of the two is dark.
