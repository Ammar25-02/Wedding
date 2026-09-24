# 05 · The jemputan page

The invitation proper, in the traditional Malay order. Two visual treatments
share exactly the same wording.

## Plain (current)

<img src="img/page2.jpg" width="220">

Open paper, taupe ink, gold diamond rules — matching the rest of the site.

**Live:** https://ammar25-02.github.io/Wedding/ (open the envelope, scroll once)

### The wording, top to bottom

| Line | Field in `INVITE` | Styled as |
|---|---|---|
| السلام عليكم ورحمة الله وبركاته | *(in the markup — edit it there)* | Amiri, large; wraps to two lines on small phones |
| Father's name | `bapa` | spaced capitals |
| & | | small, soft |
| Mother's name | `ibu` | spaced capitals |
| *Dengan penuh syukur ke hadrat Ilahi, kami menjemput anda ke majlis … anakanda kami* | `mukadimah` | body text, balanced lines |
| ◇ rule | | gold hairline |
| Bride's name | `namaPerempuan` | spaced capitals, larger |
| & | | |
| Groom's name | `namaLelaki` | spaced capitals, larger |

Then: **Pada** (date, time) · **Bertempat di** (venue, address) · map buttons ·
countdown.

**Long full names shrink automatically.** If either name is longer than 18
characters ("… BINTI …"), the couple gets the class `.long`: smaller and
tighter, so each name wraps to two balanced lines instead of three.

For a **wedding** rather than an engagement, change *pertunangan* to
*perkahwinan* in `mukadimah` and `majlis`.

### Lifting it

Search `index.html` for:

- **Markup** — `<!-- the invitation proper: salam, the parents` (the block inside `#jemputan`)
- **CSS** — `The jemputan's wording` (`.parents`, `.invite`, `.couple`, `.couple.long`)
- **JavaScript** — `long full names get the smaller couple size`

## Mosque-arch variant

<img src="img/arch.jpg" width="220">

The same wording inside a mosque-arch frame of two paper layers, on a faint
eight-point-star lattice, with deep plum blossoms rising from the arch's
lower corners. Closer to a classic printed Malay invitation.

**Demo:** [designs/arch-jemputan.html](https://ammar25-02.github.io/Wedding/designs/arch-jemputan.html) ·
**Source commit:** `b085c08`

### Lifting it

In `designs/arch-jemputan.html`, copy:

- **CSS** — the banner `The arch` … up to `the oval narrows toward its foot`
- **Markup** — `<!-- the invitation proper, in a mosque-arch frame`
- **JavaScript** — `/* ---------- flowering branches at the arch's foot ---------- */`
  (it needs `seeded()`, `place()` and the flower symbols from the
  [flower generator](building-blocks.md#watercolour-flowers))

### How the arch works

- The **dome** is an SVG with a fixed `viewBox`, so it scales evenly and the
  curve never distorts. The **straight sides** beneath are a plain `div` that
  grows with the content. The words sit over both, so the salam can rise into
  the dome.
- The **inner gold line** carries on from dome to sides at the same fraction of
  the width (10 of 300 units in the SVG = `width ÷ 30` in CSS), so the join
  doesn't show.
- **Two layers** — a slightly larger, darker arch behind a white one, each with
  its own soft shadow — give the paper-cut depth.
- The **branches** are generated: twig stems rise along gentle S-curves from
  each lower corner with blossoms strung along their upper reaches. Change
  `PLUM` for other flower colours, `TWIG` for the stems.

| What | Where | Default |
|---|---|---|
| Arch width | `--aw` on `.arch` | `min(86vw, 360px)` |
| Paper colours | `--arch-fill`, `--arch-back` | white / warm grey |
| Gold line | `--arch-line` | `rgba(194,160,99,.55)` |
| Text colour | `--plum` | `#5E3446` |
| Dome shape | the two `<path d="…">` in the markup | ogee point |
| Flower colours | `PLUM` array in the script | five plums |
| Density | `s < 8` stems, `stops` array | 8 stems per side |
