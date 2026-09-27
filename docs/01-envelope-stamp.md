# 01 · Portrait envelope with a wax stamp

<img src="img/stamp-sealed.jpg" width="200"> <img src="img/stamp-fade.jpg" width="200"> <img src="img/stamp-letter.jpg" width="200">

A portrait envelope sealed at its centre with a wax stamp pressed with the
couple's monogram. One tap: the seal fades away, the flap opens, a letter bearing the names
rises out, and the envelope dissolves into the invitation.

**Live:** https://ammar25-02.github.io/Wedding/ · **Source:** `index.html`

## The sequence

| Time | What happens | Class added |
|---|---|---|
| 0 ms | Seal fades out (0.6 s); hint fades; **music starts** | `.broken` on `#gate` |
| 620 ms | Flap swings up and back | `.opening` |
| 1100 ms | Flap drops behind the letter | `.flap-behind` on `#flapHold` |
| 1450 ms | Letter rises out of the pocket | `.lifted` |
| 3200 ms | Envelope fades; page becomes scrollable | `.away`, body loses `.sealed` |
| 4300 ms | Envelope hidden outright | `.gone` |

With *Reduce Motion* switched on, the same steps run faster and things fade
instead of moving.

## Lifting it into another project

Search `index.html` for these three banners and copy each block:

1. **CSS** — `The envelope` / `Portrait, seen from the back` … up to `.reveal{`
2. **Markup** — the `<div class="gate" id="gate">` block, just after `<body>`
3. **JavaScript** — `/* ---------- the envelope ---------- */` … up to `/* ---------- scroll cue ---------- */`

Also needed:

- `<body class="sealed">` — stops the page scrolling until the envelope opens
- `logo.png` — the monogram (make one with the [converter](https://ammar25-02.github.io/Wedding/tools/logo-converter.html))
- The colour tokens (`--paper`, `--ink`, `--ink-soft`, `--gold`, `--sage`) and
  the `--vh` unit from the top of the stylesheet
- The `$` helper, the `reduced` flag and `startSong()` from the script. If
  there's no music, delete the `startSong();` line.

The letter fills itself from the `INVITE` block (`majlis`, `nama1`, `nama2`,
`coverTarikh`) through `data-f` attributes — see
[building-blocks.md](building-blocks.md#the-invite-block).

## Things you can change

| What | Where | Default |
|---|---|---|
| Envelope width | `--ew` on `.gate` | `min(72vw, 296px, 40svh)` |
| Envelope proportion | `--eh` on `.gate` | `--ew × 1.34` (portrait) |
| Seal size | `--seal` on `.gate` | `clamp(86px, --ew × .36, 112px)` |
| Wax colour | the `waxBody` and `waxPress` gradients in the seal's `<svg>` | cream, `#FCF8EF` → `#D8C6A8` |
| Embossed monogram | `.seal-mark` — `width`, `filter` | 50 % wide, wax-coloured, in relief |
| How far the letter rises | `.lifted .letter` | `translateY(-52%)` |
| Date on the envelope | `.env-date` text in the markup | `07 · 11 · 2026` |
| Timing | the `setTimeout` delays in `openEnvelope()` | see table above |

### Changing the wax colour

The seal is an `<svg>` in the markup (search `class="seal-wax"`). Its colour
comes from two gradients — the wax body and the pressed disc — plus the
monogram's filter. Keep all three in one family:

| Wax | `waxBody` stops | `waxPress` stops | `.seal-mark` filter start |
|---|---|---|---|
| Cream *(current)* | `#FCF8EF` · `#EFE5D2` · `#D8C6A8` | `#F6EFE2` · `#E6D9C2` | `invert(.9) sepia(.32)` |
| Burgundy | `#B4454F` · `#8E2733` · `#5E1620` | `#9C3440` · `#7A1F2A` | `invert(.55) sepia(.4) hue-rotate(-20deg) saturate(2)` |
| Gold | `#F3DE9E` · `#D8B560` · `#A8843A` | `#E4C77C` · `#C9A450` | `invert(.8) sepia(.7) saturate(1.6)` |

The two small circles just before the pressed disc are its shadow (upper
left) and highlight (lower right). On a dark wax, tone the highlight down —
for burgundy set it to `fill="#E8A0A8" opacity=".5"` — or it shows as a bright
ring. On burgundy the monogram reads as a pale rose-gold relief rather than
wax-coloured; that is the effect of the filter above, and it looks intended.

## How it works

- **The seal is drawn, not a photo.** The wavy outline is an SVG path
  generated once — ten soft lobes whose depth varies round the edge, plus a
  slower wobble — so it looks like melted wax rather than a stamped badge. A
  recessed disc sits in the middle, with a shadow on its upper-left wall and
  light on its lower-right.
- **The embossed monogram** is the same `logo.png`, turned wax-coloured with
  `brightness(0) invert(.9) sepia(.32)`, then given a light edge above-left and
  a shadow below-right with two `drop-shadow`s. Nothing but that light and
  shadow makes it visible — exactly how a real seal's relief reads.
- **The seal fades.** On the tap `.broken` is added and the seal face's
  opacity transitions to 0 over 0.6 s (`.seal-face{transition:opacity .6s ease}`).
  Change `.6s` to make it quicker or slower. The flap starts opening at 620 ms,
  just as the seal finishes.
- **Want the crack back?** The earlier animation — the seal splitting in two,
  one initial on each half — is in git history at commit `ba2211c`. Copy the
  `@keyframes crackL` / `crackR` block and the two `.broken .seal-half`
  lines from there, and remove the `.seal-face` transition so it hides at once.
- **The flap** is a clipped triangle rotated `rotateX(-172deg)` inside a
  parent with `perspective`. The parent — not `preserve-3d` — carries the 3D, so
  ordinary `z-index` still decides what's in front. Halfway through the swing
  the flap is moved behind the letter.
- **The envelope sits low on the screen** (`padding-top` on `.gate`) because the
  opened flap and the rising letter both need room above it.

## Gotchas

- The seal is sized with the `--seal` length on **both** axes. `aspect-ratio`
  would be neater but needs Safari 15 — on older iPhones the seal vanished. A
  percentage `height` doesn't work either: it measures the envelope's height and
  comes out oval.
- After it fades, the envelope gets `visibility:hidden`. Without that it sits
  invisibly over the page and swallows every tap.
- More in [lessons.md](lessons.md).
