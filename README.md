# Jemputan Pertunangan — Ammar & Atiqah

A single-page Malay engagement invitation (majlis pertunangan). It opens on a portrait
envelope sealed with a wax stamp pressed with the couple's monogram; a tap cracks
the seal, opens the flap, lifts the
letter out, and dissolves into three pages: the cover, the jemputan (details, map and countdown), and the doa
with contacts. Watercolour botanical frames, drifting butterflies, and a
live countdown.

**Live:** https://ammar25-02.github.io/Wedding/

**Reusing these designs for another couple?** See the [design library](docs/README.md) —
a guide to every design built so far, working demos of the retired ones in
`designs/`, a step-by-step for [starting a new invitation](docs/new-invitation.md),
and the [monogram converter](tools/logo-converter.html) in `tools/`.

## Editing it

Everything you'd want to change lives in one block near the top of the
`<script>` in `index.html` — look for:

```js
var INVITE = {
```

| Field | What it does |
| --- | --- |
| `majlis` | The line above the names on the cover |
| `nama1`, `nama2` | The couple, used on the cover and in the jemputan |
| `coverTarikh` | Date as shown on the cover |
| `pantun` | The couplet under the cover date (`<br>` for a line break) |
| `mukadimah` | The invitation line between the parents and the couple |
| `tarikhPenuh`, `masa` | Date and time in the details section |
| `tempat`, `alamat` | Venue name and address |
| `bapa`, `ibu` | The parents (hosts), under the salam, in capitals |
| `namaPerempuan`, `namaLelaki` | The couple, bride first, in capitals — full names if you like |
| `masaMajlis` | What the countdown runs to — ISO format, `+08:00` for Malaysian time |
| `maps`, `waze` | Paste the share links. **An empty string hides that button.** |
| `hubungi` | Contacts. **An empty list removes the whole section.** |
| `lagu` | Filename of a song in this folder, e.g. `songs.mp3`. Empty = no music |

Nothing else needs touching — the page fills itself in from that object.

## Notes

- **The countdown is pinned to `+08:00`**, so a guest opening it from overseas
  sees the true time remaining, not their own local time.
- **Music can't autoplay on iPhones.** Browsers block audio until the visitor
  interacts, so the song starts on their first tap or scroll. The mute button
  appears only once sound is actually playing.
- **Phone numbers** accept any format. `tel:` strips the punctuation and the
  WhatsApp link converts a leading `0` to `6` for Malaysian numbers.
- **The botanical frames are generated**, not drawn by hand — each is a seeded
  scatter of the flower symbols in `<defs>`, so they differ from section to
  section but never change between visits.
- **Reduced motion** is respected: butterflies stop, sections fade instead of
  sliding.

## Typefaces

Self-hosted in `fonts/` so the page never waits on a font CDN:

- **Pinyon Script** — the names
- **Cormorant Garamond** — everything else
- **Amiri** — the Arabic

All three are under the SIL Open Font Licence; see `fonts/OFL.txt`.

## Artwork

The florals, cartouche and butterflies are all drawn in code (SVG and canvas)
for this page. Nothing is traced from or copied out of an existing template.

## Still to fill in

Search `index.html` for **`ISI DI SINI`** — every spot that needs your input is
marked with that comment. Anything showing as `[ SOMETHING ]` on the page is a
placeholder:

- [x] `tempat` — set to `Rumah`
- [x] `bapa`, `ibu` — Abdul Rahman & Kasmiah
- [ ] `namaPerempuan`, `namaLelaki` — currently the short names; add full names if wanted
- [ ] `hubungi` — names, roles and phone numbers
- [x] `maps` — set
- [ ] `waze` — empty, so that button is hidden; fill it in to show it
- [ ] `masa` — currently `2:00 PETANG`; add an end time if the majlis has one
- [ ] `pantun` — swap in your own couplet if you'd rather
- [x] `lagu` — set to `lagu.mp3`; starts as the envelope opens

## The monogram

`logo.png` is the couple's monogram, converted from the original artwork onto a
transparent background in its own mauve (`rgb(138,119,124)`). It sits on the
wax seal at the envelope's centre (shown pale, as if pressed into the wax), at the
head of the cover, and again at the close. Replace
the file to change it everywhere.

## The music

`lagu.mp3` starts as the envelope opens — deliberately not on page load. Phones
block audio until the visitor taps something, and the tap that opens the
envelope is exactly that gesture, so it plays reliably on iPhones as well.

**Keep the file's extension true to what's inside it.** GitHub Pages picks the
audio type from the extension — `.mp3` is served as `audio/mpeg`, `.m4a` as
`audio/mp4` — and Safari can refuse to play audio whose declared type doesn't
match its contents. Downloaders often get this wrong: the first song was AAC
labelled `.mp3`, and the replacement was an MP3 labelled `.m4a`. The current
file is a genuine MP3, so it's `lagu.mp3`.

To check a file: open it in a hex viewer or run `file` on it. `ID3` at the
start means MP3; `ftyp` in bytes 4–7 means M4A/MP4.
