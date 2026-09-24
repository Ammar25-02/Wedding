# Building blocks

Pieces shared by several designs. Each is a self-contained section of
`index.html`, marked with a comment banner.

- [The INVITE block](#the-invite-block)
- [Typefaces](#typefaces)
- [Colours](#colours)
- [Watercolour flowers](#watercolour-flowers)
- [Butterflies](#butterflies)
- [Countdown](#countdown)
- [Music](#music)
- [Map buttons](#map-buttons)
- [Sections rising into view](#sections-rising-into-view)

---

## The INVITE block

All the wording lives in one object at the top of the `<script>`:

```js
var INVITE = {
  majlis:   'Majlis Pertunangan',
  nama1:    'Ammar',
  nama2:    'Atiqah',
  coverTarikh: 'SABTU · 7 NOVEMBER 2026',
  ...
};
```

Any element with a matching `data-f` attribute is filled from it:

```html
<p class="stamp" data-f="coverTarikh">SABTU · 7 NOVEMBER 2026</p>
```

- The text already inside the element is a **fallback**, shown only if the
  script fails. Keep it roughly in step with `INVITE`.
- A value containing `<br>`, `<em>` or `<strong>` is inserted as HTML; anything
  else as plain text.
- **Adding a new field:** add it to `INVITE`, then put `data-f="yourField"` on
  the element. Nothing else to wire up.
- Every spot still needing input is marked `ISI DI SINI` — search for it.

## Typefaces

Self-hosted in `fonts/` so the page never waits on Google's servers:

| Face | File | Used for |
|---|---|---|
| Pinyon Script | `pinyon.woff2` | the couple's names in script |
| Cormorant Garamond 400 / 500 | `cormorant400.woff2`, `cormorant500.woff2` | everything else |
| Amiri (Arabic subset) | `amiri.woff2` | السلام عليكم, Bismillah |

All three are SIL Open Font Licence — keep `fonts/OFL.txt` alongside them.

**Getting another face from Google Fonts:** ask the CSS API with a modern
browser user-agent, and take the `.woff2` URL for the subset you need. For
Arabic faces pick the `/* arabic */` block — it's listed **first**, so
grabbing the last block gets you Latin instead.

**Numbers:** Cormorant's default figures are old-style, which makes "10" read
as "IO". The page sets `font-variant-numeric: lining-nums` — and anything
that also wants even-width digits must say `lining-nums tabular-nums`
together (setting only `tabular-nums` quietly cancels the lining).

## Colours

Tokens at the top of the stylesheet:

| Token | Value | Role |
|---|---|---|
| `--paper` | `#FCF9F5` | page background |
| `--ink` | `#6B5A52` | main text |
| `--ink-soft` | `#9C8C82` | labels, verse |
| `--gold` | `#C2A063` | hairlines, rules, buttons |
| `--sage` | `#A3B394` | small accents |
| `--blush` / `--blush-deep` | `#F8E5E8` / `#EBC6CE` | cartouche |

The monogram's own colour is `rgb(138,119,124)`.

## Watercolour flowers

<img src="img/cover.jpg" width="180">

The frames at the top and bottom of pages are **generated**, not drawn: flower
symbols defined once in `<defs>` (`cosmos`, `daisy`, `bud`, `sprig`, `wisp`)
are scattered along the edge by a seeded random generator, behind blurred
colour washes.

```html
<svg class="flora top"    viewBox="0 0 760 250" fill="none" aria-hidden="true"></svg>
<svg class="flora bottom" viewBox="0 0 760 250" fill="none" aria-hidden="true"></svg>
```

- **`top`** hangs from the top edge; **`bottom`** is the same drawing flipped,
  which turns it into a meadow.
- **`soft`** makes it paler. **`joins`** fades its dense edge too — use it on any
  frame sitting on the join between two pages (see the seam note below).
- **Same seed, same bouquet.** Each frame gets a seed from its position, so the
  flowers differ between frames but never change between visits.
- **Colours:** `PALETTE` (flowers) and `LEAFY` (greenery) in the script.

Code: the `<symbol>` definitions after `<body>`, the `.flora` CSS, and in the
script `seeded()`, `BOX`, `place()` and `bouquet()`.

**Avoiding seams:** blurred washes get cut off square at the SVG's edge. The
default mask fades each frame's inner edge; `.joins` also fades the outer one.
Where two pages meet, put flowers on **one** side of the join only.

## Butterflies

Drawn on a full-screen `<canvas id="flutter">`: four wings (large forewing,
small hindwing, mirrored) whose width pulses with the flap. Teal and lilac.

- Count: `hatch(window.innerWidth < 620 ? 5 : 9)`
- Colours: the `hue` line in `hatch()`
- Stops when the tab is hidden and with Reduce Motion on.
- The canvas **must** have CSS `width:100%; height:100%` — see
  [lessons.md](lessons.md).

## Countdown

```js
masaMajlis: '2026-11-07T14:00:00+08:00',
```

- **Always include `+08:00`.** Guests abroad then see the true time remaining,
  not a count to their own local 2 PM.
- On the day, the digits are replaced by *"Alhamdulillah — hari yang dinanti
  telah tiba."*
- Labels are `<dt>` elements in the markup (Hari / Jam / Minit / Saat).

## Music

```js
lagu: 'lagu.mp3',
```

- **Starts when the envelope opens**, inside that tap. Phones refuse to play
  sound until the visitor taps something, so music can't start on page load
  on an iPhone.
- Fades up over 2.5 s to 70 % volume.
- A **mute button** appears top-right only once sound is actually playing.
- Empty `lagu` → no music, and the mute button is removed.
- **The extension must match the file's real format** — see
  [lessons.md](lessons.md#music).

## Map buttons

```js
maps: 'https://maps.app.goo.gl/…',
waze: '',
```

Paste share links straight from the apps. **An empty string removes that
button.**

## Sections rising into view

Give an element the class `reveal` and it fades up as it scrolls into view
(an `IntersectionObserver`). With Reduce Motion on it only fades.
