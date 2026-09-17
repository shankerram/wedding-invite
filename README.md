# Banana-leaf wedding invitation

A single self-contained `index.html`. No build step, no dependencies, no image
files — the leaf texture, the thread tie, the Ganesha crest, the garlands and
bells, the hanging brass lamps, the gopuram watermark, the flower urns and the
kolam are all drawn in CSS and SVG at runtime.

Open `index.html` in a browser, or host the file anywhere (it is one file).

## Editing the details

Everything you need to change is in one block near the top of the `<script>`,
marked `EDIT EVERYTHING BELOW`:

```js
const INVITE = {
  invocation,                                   // the Devanagari line
  hosts, request,                               // card 1
  bride, brideParen, joiner, groom, groomParen, //   …down to the parentage
  dateLead, date, venueLead, venue, address,    // card 2
  slots : [ {k,v}, ... ],                       //   Muhurtham / Reception
  complimentsLead, compliments,
  mapUrl                                        // the Directions button
};
```

`slots` is a list, so the timings block on card 2 grows or shrinks with it.

**Keep each card's copy roughly the length of what is there.** The two cards are
sized to the artwork and the copy is vertically centred with no scroll, so a
much longer line pushes the block into the garland at the top or the urns at the
foot. `.copy` is a flex column: an item with no intrinsic minimum height (an
inline `<svg>`, such as the Ganesha crest) will silently shrink to absorb any
overflow rather than spill, which is why `.gan` carries `flex:none`. If you add
lines, re-check the fit rather than trusting that nothing looks broken.

## The two cards

1. Ganesha crest, `ॐ श्री गणेशाय नमः`, the hosts, the request, and both names
   with their parentage — ending on the groom's parents.
2. The date, the venue and address, Muhurtham and Reception timings, and the
   compliments line. The Directions button is a web-only extra; drop the `<a
   class="mapbtn">` from `PAGES[1].html` if you don't want it.

Both cards share one `cardArt()` frame so they read as a matched pair.

## How it works

- **The tie.** A twisted red-and-saffron cord — two intertwined dashed strokes
  over a dark rim — wraps the folded leaf and finishes in a bow. Tapping unties
  it: the loops shrink into the knot, the tails whip through and drop, the band
  goes slack and the cord falls away, and only then does the leaf unfold (about
  1s in total). Folding the card shut re-ties it.
- **The fold.** Three panels hinged in 3D: a centre spread with a lobe on each
  side. Closed, the right lobe folds across first and lands offset, so its
  rounded tip reads as a curve over the lighter lobe beneath.
  Note that `#flapR`'s `rotateY(-177deg)` flips its `translateZ(-60px)` *toward*
  the viewer, which is why `#tie` sits at `translateZ(112px)` to clear it.
- **The pages.** A stack bound at the left edge, each turning on the spine with
  a sheen that peaks mid-turn. A turned page lies flat on the left lobe, as it
  does on the real card.
- **Controls.** Tap or click the leaf to untie and open. After that, tap the
  right or left third of the card, use the arrows or dots, press the arrow keys,
  or swipe on a phone. `Esc` or the ✕ folds it shut again.
- **Motion.** Respects `prefers-reduced-motion` — the cord is simply absent, the
  transitions collapse and the falling petals are suppressed.

## Notes

- Fonts come from Google Fonts (Cormorant Garamond, Marcellus, Cinzel, Great
  Vibes, Tiro Devanagari Sanskrit) with serif fallbacks, so it degrades
  gracefully offline.
- Everything is deterministic: the flower placement uses a seeded RNG, so the
  artwork is identical on every load.
- `.pf > svg` is the page artwork. The selector is a direct-child one on purpose
  — a descendant selector also catches the crest inside `.copy` and blows it up
  to fill the page.
