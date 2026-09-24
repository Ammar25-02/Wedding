# Starting a new invitation

From an empty repository to a live link, reusing this one.

## 1 · Make the repository

1. On GitHub, create a new **empty** repository (no README).
2. Copy these from this repo into it:
   - `index.html`
   - `fonts/` (the whole folder, including `OFL.txt`)
   - `tools/` — optional, but handy to keep with each invitation
3. Leave out `logo.png` and `lagu.mp3` — those are the new couple's.

## 2 · The monogram

1. Open [tools/logo-converter.html](https://ammar25-02.github.io/Wedding/tools/logo-converter.html) (works offline
   from the file, or on the live site).
2. Drop in the couple's logo — dark artwork on a light background.
3. Choose *Keep the logo's own colour*, or recolour it to the invitation's ink.
4. Check the wax-seal preview looks right, then **Download logo.png**.
5. Note the **Ratio** figure — you need it only for the doors design.

## 3 · Choose the opening

| Want | Use | Swap in from |
|---|---|---|
| Wax-stamped envelope *(default)* | [01](01-envelope-stamp.md) | already in `index.html` |
| Plain landscape envelope | [02](02-envelope-landscape.md) | `designs/envelope-landscape.html` |
| Double doors | [03](03-doors.md) | `designs/doors.html` — set the ratio |
| They have a finished card image | [04](04-card-envelope.md) | the `wedding-Invite` repo |

To swap, replace the three blocks named in that design's guide.

## 4 · The wording

Open `index.html`, find `var INVITE = {`, and fill in every field.
Search for `ISI DI SINI` to catch anything you've missed.

| Field | Engagement | Wedding |
|---|---|---|
| `majlis` | Majlis Pertunangan | Walimatulurus / Majlis Perkahwinan |
| `mukadimah` | … majlis **pertunangan** anakanda kami | … majlis **perkahwinan** anakanda kami |
| Doa (in the markup) | engagement prayer | marriage prayer |

Also update, outside `INVITE`:

- the date printed on the envelope (`.env-date` in the markup)
- the `<meta name="description">` and `<title>` near the top

## 5 · The song

1. Put the file next to `index.html`.
2. **Check its real format** — `ID3` at the start = MP3; `ftyp` in bytes 4–7 =
   M4A. Name it `.mp3` or `.m4a` to match.
3. Set `lagu: 'lagu.mp3'` (or `.m4a`). Leave it empty for no music.

## 6 · Publish

1. Commit and push.
2. On GitHub: **Settings → Pages → Deploy from a branch → `main` / root → Save.**
3. Wait 1–5 minutes, then open `https://<username>.github.io/<repo>/`.

## 7 · Test on a phone

Run the checklist in [lessons.md](lessons.md#before-you-share-the-link--checklist)
on a real iPhone, in both Safari and WhatsApp's browser, before sending the
link to anyone.
