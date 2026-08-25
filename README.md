# Banana-leaf wedding invitation

A single self-contained `index.html`. No build step, no dependencies, no image
files — the leaf texture, the mandap arch, the garlands, the gopuram, the
marigold toran and the tassels are all drawn in CSS and SVG at runtime.

Open `index.html` in a browser, or host the file anywhere (it is one file).

## Editing the details

Everything you need to change is in one block near the top of the `<script>`,
marked `EDIT EVERYTHING BELOW`:

```js
const INVITE = {
  invocation   : "Shubham Astu",
  blessingLine : "...",              // HTML allowed, <br> for line breaks
  groom, bride, joiner, heroDate,    // page 1
  muhurtham    : { title, when, time, venue, addr },   // page 2
  events       : [ {k,v,w}, ... ],   // page 3 — add or remove freely
  eventsNote,
  familyLeft, familyRight, rsvp,     // page 4
  mapUrl                             // the Directions button
};
```

`events` is a list, so page 3 grows or shrinks with it. Keep each page's copy
roughly the same length as the placeholder text — the panels are sized to the
artwork, and a much longer line will crowd the temple scene or the toran.

## How it works

- **The fold.** Three panels hinged in 3D: a centre spread with a lobe on each
  side. Closed, the right lobe folds across first and lands offset, so its
  rounded tip reads as a curve over the lighter lobe beneath. Tapping unfolds
  both lobes flat.
- **The pages.** A stack bound at the left edge, each turning on the spine with
  a sheen that peaks mid-turn. A turned page lies flat on the left lobe, as it
  does on the real card.
- **Controls.** Tap or click the leaf to open. After that, tap the right or
  left third of the card, use the arrows or dots, press the arrow keys, or
  swipe on a phone. `Esc` or the ✕ folds it shut again.
- **Motion.** Respects `prefers-reduced-motion` — transitions collapse and the
  falling petals are suppressed.

## Notes

- Fonts come from Google Fonts with serif fallbacks, so it degrades gracefully
  offline.
- Everything is deterministic: the flower placement uses a seeded RNG, so the
  artwork is identical on every load.
