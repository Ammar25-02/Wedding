# Lessons learned

Every item here was a real problem on these invitations. Check this list
before sending a new one out.

## Before you share the link — checklist

- [ ] Opened on an **iPhone**, in Safari **and** in WhatsApp's own browser
- [ ] Envelope/doors are full-size, not tiny
- [ ] The seal and "Sentuh untuk membuka" are both visible
- [ ] Tapping opens it smoothly and the music starts
- [ ] Every page is centred; nothing scrolls sideways
- [ ] Countdown shows lining figures (`10`, not `IO`) and the right time
- [ ] Map, call and WhatsApp buttons open the right place and people
- [ ] No `[ PLACEHOLDER ]` or `ISI DI SINI` text left on the page
- [ ] The time matches every other invitation for the same event

---

## Phones and Safari

**Every page needs a real document head.**
Without `<!doctype html>` and
`<meta name="viewport" content="width=device-width, initial-scale=1">`, iPhones
lay the page out 980 px wide and shrink it to a third. Everything looks tiny.

**`aspect-ratio` needs Safari 15.**
On older iPhones an element sized only by `aspect-ratio` has no height and
disappears — this is how the wax seal went missing. Give both width and height
the same length instead. (A percentage `height` doesn't work either: it
measures the *container's height*, so a circle comes out oval.)

**`inset` needs Safari 14.1; flexbox `gap` too.** Fine for current phones, but
the reason very old ones break.

**Use `svh`, not `vh` or `dvh`, for layout.**
iOS's `vh` is the height with the toolbar *hidden*, so anything placed with it
can end up behind the toolbar. `dvh` changes as the toolbar collapses, which
re-flows the page mid-scroll. `svh` is steady and always fits. The pages use a
`--vh` variable that falls back to `vh` on old browsers.

**Low Power Mode and Reduce Motion.**
Low Power Mode throttles animation on iPhones — test with it off. With Reduce
Motion on, animations should become *gentler*, not vanish: an earlier version
skipped the whole opening, so the card just appeared.

## Smooth animation

**Only animate `transform` and `opacity`.**
Animating a CSS `filter` (brightness, blur, drop-shadow) repaints the element
every frame — invisible on a laptop, stuttering on a phone. To darken a card,
fade an overlay on top of it instead.

**Take `drop-shadow` off anything that's rotating**, for the same reason.

**Don't start other work mid-animation.** Starting a canvas animation or
revealing big new sections during the lift caused a hitch at the end of it.
Wait until the movement has landed.

**3D flips: put `perspective` on the parent.**
`transform-style: preserve-3d` on a container makes the browser ignore
`z-index` and stack by 3D position — the card appeared in front of the
envelope. A flat stack plus `perspective` on the flipping element's parent keeps
normal stacking.

**Hide overlays when they're done.**
A faded-out envelope or door still sits on top of the page and swallows taps.
Set `visibility:hidden` once its animation ends.

## Layout traps

**`position:relative` brings `left`/`top` back to life.**
The card used `left:50%` inside the envelope. When its final state switched to
`position:relative`, that offset applied again and the card landed half a screen
to the right. Reset `left/right/top/bottom:auto` when changing position.

**A `<canvas>` needs CSS `width` and `height`.**
`inset:0` doesn't stretch it: it takes its pixel size, which is 2–3× the screen
on a phone, so drawings came out oversized and off the edge. Also cap its
pixel ratio at 1.5 on phones — clearing millions of pixels a frame for
decoration costs real battery.

**Reserve space for anything overlaid.** Padding one side of a box to make room
for a "scroll" hint pushed the card off-centre. Overlay the hint instead, and
keep the padding symmetric.

**`font-variant-numeric` doesn't add up.** A later rule setting only
`tabular-nums` cancels an earlier `lining-nums`. Write both together.

## Music

**Phones won't play sound until the visitor taps.** No code gets around it.
Start the audio inside the tap that opens the invitation, and always offer a
mute button.

**The file's extension must match what's inside it.**
GitHub Pages declares the type from the extension; Safari can refuse audio
whose declared type doesn't match its contents. Downloaders get this wrong
constantly — the first song was AAC in an `.mp3`, the second an MP3 in an
`.m4a`. Check before uploading: `ID3` at the start of the file means MP3;
`ftyp` in bytes 4–7 means M4A.

**A downloaded TikTok track is someone else's music**, and the repo is public —
anyone can download it from there.

## Images

**Converting a logo to transparent:** measure each pixel's distance from the
background *against the logo's ink colour, not against black*. Measuring
against black left a mid-tone mauve monogram only half opaque. The
[converter](https://ammar25-02.github.io/Wedding/tools/logo-converter.html) does it correctly.

**Proportions are baked into the CSS** in a few places — the card envelope's
`--card-ratio` and the doors' `--logo-h`. Update them when the image changes.

## GitHub and hosting

**Pull before you push.** Edits made on github.com and edits made locally
collide. Fetch and rebase first; never force-push over someone's changes.

**Editing an SVG icon's `d="…"` blanks the icon.** One web edit accidentally
emptied the map pin's path — a missing icon with no error.

**GitHub Pages is the reliable host.** It rebuilds 1–5 minutes after each push.
A Render deployment of the same repo repeatedly served old builds.

## Testing

**Headless browsers lie in specific ways:**

- Their *virtual time* doesn't run CSS transitions or media — use a real-time
  browser session to watch an animation.
- The window size you ask for isn't always the layout width — read
  `document.documentElement.clientWidth` before trusting a screenshot.
- A fake event from page script doesn't count as a user tap, so it won't unlock
  audio. Test music with a real input event.

**Always look at the actual page.** Most bugs above were found by opening the
live link on a phone, not by reading code.
