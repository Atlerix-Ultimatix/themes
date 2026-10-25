# Brief: create a community theme for Atlas L1

> **Who this is for.** An AI assistant asked to create a theme for the Atlas L1 shop software.
> This version is for the **community theme repository**, fan themes inspired by an existing work
> (game, film, series, character, universe) included. Give it this document, the file
> `gabarit.atlas-theme.json` (the template) and the checker `check-theme.mjs`, then describe the
> theme you want. The assistant must follow every rule below: a file that breaks a single one is
> refused by the application.

## Your task

Produce **one file** `<name>.atlas-theme.json` that styles the application as asked, then **run it
through the checker until it says "OK"**:

```
node check-theme.mjs <name>.atlas-theme.json
```

Never deliver a file the checker refuses. Fix it from the reason shown (`REFUSED [code] detail`)
and run it again. The checker runs exactly the application's own checks: "OK" for you means
"accepted" in the shop.

Start from a copy of `gabarit.atlas-theme.json`: it is valid as is.

## What a theme can and cannot do

A theme changes how the application **looks**, **never what it does**. It cannot read data, send
anything, hide, move or cover an element, or act on the server. Any attempt refuses the whole
file. So there are **no scripts**, **no external addresses** (no image, font or stylesheet from
elsewhere) and **no added text**.

## A theme inspired by an existing work

This repository accepts **fan themes**. The work's name may appear in the `nom` field (for
example "Tribute to <work>"). In return, these rules are strict:

- **Everything is hand-made.** No image taken from the work: no extracted sprite, logo,
  screenshot, official artwork or scan. The theme's images are **drawn by you** (patterns, pixels,
  gradients), inspired by the style without tracing it.
- **No imitated logo or trademark.** Do not reproduce a logo, emblem or official lettering, even
  redrawn.
- **Colours and mood are free**: a palette, an era, a texture can recall a work without copying it.
- **Never official.** The theme never presents itself as made, approved or endorsed by the rights
  holder. The `licence` field reads
  `Unofficial fan theme; original artwork CC-BY-4.0` (or an equivalent free licence).
- **The author's name** goes in `auteur`: they answer for their creation.
- **No shocking content** and no real person's name.

## The file

A JSON object in UTF-8, **256 KB at most** in all.

| Field | Required | Rule |
|---|---|---|
| `format` | yes | always `1` |
| `nom` | yes | the theme's name, 1 to 40 characters |
| `auteur`, `licence`, `version` | no | author, licence, version: text of 80 characters at most; a free licence is recommended (`CC0-1.0`, `CC-BY-4.0`) |
| `theme` | yes | see below |
| `css` | no | stylesheet, 64 KB at most, see the rules |
| `images` | no | 8 images at most, see the rules |

**No other field is accepted**, neither at the top of the file nor inside `theme`.

### `theme`

| Field | Required | Rule |
|---|---|---|
| `fontFamily` | yes | a list of families; **the first** must be one of: `Inter Variable`, `Manrope Variable`, `IBM Plex Sans Variable`, `Source Sans 3 Variable`, `DM Sans Variable`, `Plus Jakarta Sans Variable`, `Press Start 2P`, `system-ui`, `Tahoma`, `Segoe UI`. Letters, digits, spaces, hyphens, quotes and commas only. |
| `prefersDark` | yes | `true` if the theme is designed for night first |
| `accent` | yes | a `#rrggbb` colour (primary buttons, links, selection) |
| `neutrals` | no | `{ "hue": 0 to 360, "chroma": 0 to 2 }`: the tint of the greys |
| `surfaces` | yes | `{ "menu", "fond", "carte" }` (menu bar, page background, card), each optional |
| `surfacesClair` / `surfacesSombre` | no | same keys, replacing a surface by day or by night only |
| `shape` | yes | `{ "card": length, "control": length }`; length = `0`, or a number followed by `px`, `rem` or `em` (e.g. `0.75rem`) |
| `encreMinimale` | no | 0 to 100: minimum strength of secondary text |
| `flat` | yes | `true` for a render without relief |
| `nuitSeulement` | no | `true` for a theme made for the night only: the app shows it at night whatever the device asks (the day/night switch disappears), and only its night is measured |

A surface is `{ "mode": "exact", "hex": "#rrggbb" }` (an exact colour), or
`{ "mode": "libre", "teinte": 0-360, "saturation": 0-100, "clarte": 0-100 }` (hue, saturation,
lightness), or `{ "mode": "defaut" }`. At night, a light exact colour is darkened automatically,
keeping its hue.

**Legibility is required:** on every surface, by day and by night, text must reach a contrast of
**4.5:1**. Mid greys (around `#777`) fail: pick surfaces clearly light or clearly dark.

## The stylesheet (`css`)

Every rule is automatically limited to the imported theme: write your selectors normally
(`.surface-carte { … }`), the application adds the prefix itself.

**This is an allowlist**: any property not listed here refuses the file. A theme dresses the
application; it can neither hide, move nor cover any of its elements.

### On the application's elements

| Properties | Limits |
|---|---|
| `color` | readable: neither `transparent` nor 30 % opacity or less |
| `background`, `background-color`, `background-image`, `background-size`, `background-position`, `background-repeat`, `background-attachment`, `background-origin`, `background-blend-mode` | gradients and `var(--image-<name>)`; no address |
| `border` and its sides, `border-color`, `border-style`, `border-width` | 8 px thick at most |
| `border-radius` and its corners | 48 px at most |
| `outline`, `outline-color`, `outline-style`, `outline-width`, `outline-offset` | 6 px at most |
| `box-shadow` | offsets, blur and spread of 32 px at most |
| `text-shadow` | 16 px at most |
| `font-family` | fonts from the list above |
| `font-weight`, `font-style`, `font-variant`, `text-transform` | free |
| `font-size` | 10 to 64 px (0.625 to 4 rem) |
| `letter-spacing`, `word-spacing` | −2 to 8 px, −2 to 16 px |
| `line-height` | 0.9 to 3 |
| `text-align` | `left`, `right`, `center`, `justify`, `start`, `end` |
| `text-decoration` and its variants, `text-underline-offset` | 6 to 8 px at most |
| `padding` and its sides, `gap`, `row-gap`, `column-gap` | 0 to 48 px; no percentage |
| `opacity` | 0.5 at least |
| `position` | `relative` or `static` only |
| `animation` and its variants, `transition` and its variants | see animations |
| `cursor` | a keyword (`pointer`, `default`…) |
| `accent-color`, `caret-color`, `scroll-behavior`, `overscroll-behavior` | free |

No size (`width`, `height`), no placement (`top`, `left`, `inset`, `margin`), no `display`,
`visibility`, `transform`, `filter`, `clip-path`, `overflow`, `z-index`, `pointer-events`,
`user-select` or `all` on the application's elements.

### On decorations (`::before` and `::after`)

On top of the properties above: `content`, `display`, `visibility`, `opacity`, `position`
(`absolute` or `fixed`), `inset`, `top`, `right`, `bottom`, `left`, `width`, `height`, `min-*`,
`max-*`, `margin`, `transform`, `transform-origin`, `z-index`.

- `content`: only `""`, `none`, `normal` or `var(--image-<name>)`. **No text.**
- A **sized or placed** decoration must be out of the flow (`position: absolute` or `fixed`) and
  **behind** the content (`z-index: 0` or below, in the same rule). For example:

```css
.surface-carte { position: relative; }
.surface-carte::before {
  content: "";
  position: absolute;
  inset: 0;
  z-index: -1;
  background: linear-gradient(135deg, #eef2ff, #ffffff);
}
```

### Animations

`@keyframes` only animates `color`, `background-color`, `background-position`, `border-color`,
`box-shadow`, `text-shadow`, `outline-color` and `opacity` (0.5 at least). Nothing moves or
disappears.

### Always refused

- `url(` in any form, `image-set(`, `@import`, `@font-face`, `@namespace`, `@charset`, `@layer`,
  `@property`, `@page` and any `@` rule other than `@media`, `@supports`, `@keyframes`;
- the functions `attr()`, `env()`, `image()`, `cross-fade()`, `element()`, `paint()`, `src()`,
  `expression()`;
- **any backslash** (no escapes) and the `<` character, media queries included: write
  `@media (max-width: 600px)`, not `@media (width < 600px)`;
- selectors naming `:root`, `html`, `:host` or `:has(`;
- **variables defined by the sheet** (`--my-colour: …`): the sheet only reads the application's
  (next table);
- **text**: quotes only name fonts;
- `calc()`, `min()`, `max()`, `clamp()` on the application's elements: write the values out;
- **an opaque background colour on the menu** (`.surface-menu`, `[data-app-sidebar]` and their decorations):
  the app derives the labels' ink from the menu colour, which may also come from the shop's own
  settings. Start from `var(--surface-menu)` (for example
  `linear-gradient(180deg, color-mix(in srgb, var(--surface-menu) 88%, #ffffff), var(--surface-menu))`)
  or keep colours at 25 % opacity or less; an image `var(--image-<name>)` is still allowed;
- braces that do not balance.

### Day and night

The stylesheet cannot tell whether the screen is in day or night mode. For a colour to follow on
its own, **use the application's variables** rather than fixed colours:

| Variable | Holds |
|---|---|
| `var(--surface-menu)`, `var(--surface-fond)`, `var(--surface-carte)` | the three surfaces, day or night |
| `var(--color-primary-50)` to `var(--color-primary-950)` | the accent's scale |
| `var(--color-gray-50)` to `var(--color-gray-950)` | the grey scale |
| `var(--radius-card)`, `var(--radius-control)` | the radii from `shape` |
| `var(--font-app)` | the font from `fontFamily` |

A fixed colour in the stylesheet stays the same by day and by night: keep it for accents.

### Where to hook your rules

| Selector | Element |
|---|---|
| `[data-app-menubar]` | the top bar (desktop) |
| `[data-app-sidebar]` | the left menu (desktop) |
| `[data-app-main]` | the page |
| `.surface-menu`, `.surface-fond`, `.surface-carte` | faces painted with the three surfaces |
| `[data-nav-title]`, `[data-nav-icon]`, `[data-nav-row-face]` | menu section titles, icons and rows |
| `[aria-current="page"]` | the menu row of the open page |
| `[role="dialog"]` | windows |
| `[data-menubar-panel]` | the avatar's menu |
| `[data-status-color]` | status chips (leave them their colour: it carries meaning) |
| `h1`, `table`, `thead th`, `tbody td`, `button`, `input`, `select` | common elements |

Broad rules (`button { … }`) reach the whole application: stay restrained and check that coloured
buttons keep their meaning (red = danger).

## Images (`images`)

An object `{ "name": "data:image/png;base64,…" }`:
- name in lowercase letters, digits and hyphens, 32 characters at most;
- **PNG, JPEG, WebP or GIF only**, base64-encoded inside the file itself; **no SVG**;
- the content must really be the announced type (it is checked byte by byte);
- 96 KB at most per image once decoded, 8 images at most.

In the stylesheet, an image is used through its variable: `background-image: var(--image-<name>);`.
A small repeated pattern (`background-size: 8px 8px`) costs little and looks good.

## What you deliver

1. The file `<name>.atlas-theme.json`, **accepted by the checker** (paste its "OK" line).
2. Three lines: the theme's intent, its main colours, what its stylesheet does.
3. If the theme is inspired by a work: one line confirming that **every image is original** and
   that no official element was reused.

The shop's administrator installs it in **Settings → Appearance → Imported theme → Import a
file…**, sees a preview, then installs it, and can remove it at any time.

A theme reported by a rights holder is removed from the repository without discussion.
