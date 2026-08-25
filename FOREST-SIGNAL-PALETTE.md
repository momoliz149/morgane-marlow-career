# Forest & Signal — palette reference

The implemented page is already in this folder as `index-forest-signal.html`. This document is the palette itself: the tokens, the rules that keep them in order, and the traps I hit building it.

---

## Fast path

The work is done. To go live:

```bash
mv index.html index-olive-backup.html
mv index-forest-signal.html index.html
```

`img/morgane-hero-cutout.png` is already in place and referenced by the new file. Diff the two first if you want to see exactly what changed — every difference is either a colour, the hero image, the three tile numerals replacing the emoji, or the footer alignment fix.

Everything below is the reference for what comes *next* — any new section, component or page you add should be built from these tokens rather than new values.

---

## The tokens

Paste-ready. Replace the whole `:root` block.

```css
:root {
  /* ── Grounds — one light, one dark ── */
  --paper:            #F1F2ED;
  --ground:           #16302A;

  /* ── Ink on paper ── */
  --ink:              #171A17;
  --ink-mid:          #3E4A45;
  --muted:            #5C706B;

  /* ── Ink on the forest ground ── */
  --on-ground:        #F1F2ED;
  --on-ground-body:   #B7B8B7;
  --on-ground-muted:  #949493;

  /* ── The one signal — actions only ── */
  --signal:           #DB633B;
  --signal-hover:     #ED7A54;
  --signal-ink:       #171A17;
  --signal-text:      #C24218;   /* the ONLY orange allowed as type, and only on --paper */

  /* ── Rules & borders ── */
  --tile-border:      #677872;
  --rule:             rgba(23, 26, 23, .14);
  --rule-on-ground:   rgba(241, 242, 237, .16);
}
```

Fourteen values. Every one measured against the ground it actually sits on:

| Token | Context | Ratio | Needs | |
|---|---|---|---|---|
| `--ink` | on --paper | 15.6 | 4.5 | ✓ |
| `--ink-mid` | on --paper | 8.22 | 4.5 | ✓ |
| `--muted` | on --paper | 4.68 | 4.5 | ✓ |
| `--signal-text` | on --paper | 4.57 | 4.5 | ✓ |
| `--ink` | on --signal (button) | 4.9 | 4.5 | ✓ |
| `--ink` | on --signal-hover | 6.28 | 4.5 | ✓ |
| `--on-ground` | on --ground | 12.52 | 4.5 | ✓ |
| `--on-ground-body` | on --ground | 7.08 | 4.5 | ✓ |
| `--on-ground-muted` | on --ground | 4.64 | 4.5 | ✓ |
| `--signal` | edge vs --ground | 3.93 | 3.0 | ✓ |
| `--tile-border` | edge vs --ground | 3.02 | 3.0 | ✓ |
| `--ground` | edge on --paper (stat card, dot) | 12.52 | 3.0 | ✓ |
| `--on-ground` | on --ground (stat number) | 12.52 | 4.5 | ✓ |
| `--on-ground-muted` | on --ground (stat label) | 4.64 | 4.5 | ✓ |

---

## Three rules

**1. Orange is never type on green.** Not on the ground, not on a card, not anywhere. The best case measures 3.93:1 against the forest ground and AA wants 4.5. `--signal-text` exists for exactly one situation: orange type on `--paper`, where it clears 4.57:1. Everywhere else orange is a *filled object* — a button, a rule, an underline. This is also the Braun idea: the signal marks where an action is, it was never a text colour.

**2. Green never asks for a click.** Grounds, rules, borders, the neutral ramp. If something is green and clickable, it's clickable because of its shape, not its colour.

**3. One signal, one job.** Orange appears on the primary button, the active nav underline, and the tile CTA underline. That's the whole list. The moment it becomes decorative the system is gone — which is exactly how the original site ended up with seven colours and no hierarchy.

---

## Where each ground goes

| Section | Ground |
|---|---|
| `.site-nav` | `--ground` (scrolled: `rgba(22,48,42,.90)` + blur) |
| `.hero` | `--ground` |
| `.about-career` | `--paper`, `border-top: 1px solid var(--rule)` |
| `.competencies` | `--paper`, `border-top: 1px solid var(--rule)` |
| `.work-with-me` | `--ground` |
| `.ai-central` | `--paper`, `border-top: 1px solid var(--rule)` |
| `footer` | `--ground` |

Two forest blocks, paper between them, footer closes dark. Sections on the same ground are separated by a hairline rule, never by a change of hue — that was the thing the original site got wrong.

---

## Component recipes

**Primary button** — `--signal` fill, `--signal-ink` text, `--signal-hover` on hover. Identical on both grounds; it never changes text colour by context.

**Secondary button on paper** — transparent, `--ink` text, `1.5px solid var(--rule)`. Inverts to `--ink` fill with `--paper` text on hover.

**Secondary button on forest** — transparent, `--on-ground` text, `1.5px solid var(--rule-on-ground)`. Inverts on hover. This is `.btn-on-dark`; don't use `.btn-secondary` on a forest section, its border disappears.

**Tiles on forest** — `background: transparent`, `1px solid var(--tile-border)`, border goes `--signal` on hover. No fill. See the trap below for why.

**Stat cards on paper** — `--ground` fill, `--on-ground` numerals, `--on-ground-muted` labels.

**Link with the signal** — `--on-ground` text with `border-bottom: 2px solid var(--signal)`. Used on the nav's "but, why?" and the tile CTAs. The orange is a mark under the type, never the type.

---

## The trap I fell into — worth knowing before you extend this

My first build gave the tiles a lifted fill, `#40554F`, chosen so `--on-ground` would clear 7:1 on it. Then I put the *dimmer* tokens on that fill without rechecking. Four failures, all from the same mistake:

| What I wrote | Measured | Needed |
|---|---|---|
| `--signal` as the tile CTA text on the fill | 2.23 | 4.5 |
| `--on-ground-muted` as the tile numeral on the fill | 2.63 | 4.5 |
| `--on-ground-body` as the tile description on the fill | 4.01 | 4.5 |
| `#C9542E` as the button hover with ink on it | 4.00 | 4.5 |

**A token is calibrated against one specific surface.** Move it onto a different surface and its ratio changes — silently, because nothing errors. The tile fill also could not clear 3:1 against the ground without going too light for its own text, which is the same conflict from a different angle.

Removing the fill solved all of it at once: with transparent tiles every token sits on `--ground`, exactly where it was measured. If you ever add a filled surface on the forest ground, it needs its own set of calibrated tokens — don't borrow the `--on-ground-*` ones.

---

## Verification

1. `grep` for `--card`, `#F8FFED`, `#D3D858`, `#585B0A`, `#2B2D00`, `#CE75A1` — all should return zero.
2. No orange as `color:` anywhere except `--signal-text` on a `--paper` section.
3. Every tile and card on the forest ground has a visible border.
4. `text-transform: lowercase` appears only on `.btn`, `.nav-link`, `.text-label`, `.work-tile__cta`, `.footer__col-title`, `.footer-tagline`. Not on `body`, `.stat-card__label` or `.footer__strip`.
5. No horizontal scroll at 390px; the nav wordmark stays on one line.
6. Display face at 44px and up only — five rules should set `font-family: var(--font-display)`.

---

## Still outstanding

Punch list items 7–17, unchanged by any of this. Two now matter more than they did:

**"details coming soon" appears three times** in what is now the most prominent section on the page. The design is no longer the weak part of that block — the copy is.

**The section order is still CV-first.** You picked a clients-lead read four rounds ago and the page still answers "has she done it" before "can she help me". The Forest & Signal grounds are already assigned per section, so reordering is moving blocks, not recolouring them.

Also still open: the `og:image` points at a file that isn't in the project, so every LinkedIn share renders blank; `aicentral.com` and `butwhy.com` need confirming as live domains; and the site is still on a `vercel.app` subdomain while the contact address is `@morganemarlow.com`.
