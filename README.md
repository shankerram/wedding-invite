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

**Keep each card's copy roughly the length of what is there.** The cards are 400
square and the copy is vertically centred with no scroll, so a much longer line
pushes the block into the garland at the top or the urns at the foot. `.copy` is a flex column: an item with no intrinsic minimum height (an
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

- **The tie.** A fine twisted thread — brick red wound with ochre — wraps the
  folded leaf and finishes in a bow. Its colours and thickness are measured off
  a photograph of the real card (`#8c1a10`, `#c9a33e`, cord ≈ 2.8% of the leaf
  width), so keep them muted: a bright, evenly striped rope reads as plastic
  piping rather than thread. Tapping unties it — the loops draw into the knot,
  the ends pull through, the band relaxes — and the cord then **stays**,
  settling onto the leaf behind the card as it opens. Folding the card shut
  re-ties it.
  The path morph uses SMIL (`<animate attributeName="d">`) rather than CSS, so
  it works in Firefox too; the two shapes of each length must therefore share
  the same command structure and point count.
- **The card showing through.** While folded, `#book` is clipped to `#clipPeek`
  — a notch in the top edge and a corner at the foot — and lifted in front of
  both lobes, so the real card shows through exactly where the printed one
  does. Everywhere else it is clipped away, which is what makes lifting it
  safe. Both the clip and the lift are dropped 0.2s into the unfold, once the
  lobes have visibly moved, so nothing pops.
- **The fold.** Three panels hinged in 3D: a centre spread with a lobe on each
  side. Closed, the lobes land square on each other — their tapered clip paths
  are what make them read as two — and the card is square inside, as the
  printed one is.
  A lobe's own `rotateY(~180deg)` flips the sign of its `translateZ` and
  `translateX`, so those negative values push it *toward* the viewer and to the
  right. That is why `#tie` sits at `translateZ(112px)` to clear the folded
  lobes, and why `#flapR`'s `translateX` is 0 rather than a negative offset.
- **The pages.** A stack bound at the left edge, each turning on the spine with
  a sheen that peaks mid-turn. A turned page lies flat on the left lobe, as it
  does on the real card.
- **Controls.** Tap or click the leaf to untie and open. After that, tap the
  right or left third of the card, use the arrows or dots, press the arrow keys,
  or swipe on a phone. `Esc` or the ✕ folds it shut again.
- **Motion.** Respects `prefers-reduced-motion` — the cord is simply absent, the
  transitions collapse and the falling petals are suppressed.

## Notes

- Fonts come from Google Fonts — Playfair Display for the weighted lines, Lora
  italic for the running text, Pinyon Script for the two names, Tiro Devanagari
  Sanskrit for the invocation, Cinzel for the small caps — with serif fallbacks,
  so it degrades gracefully offline. The three families are set together at the
  top of the page-copy CSS block; swap them there and the whole card follows.
- Everything is deterministic: the flower placement uses a seeded RNG, so the
  artwork is identical on every load.
- `.pf > svg` is the page artwork. The selector is a direct-child one on purpose
  — a descendant selector also catches the crest inside `.copy` and blows it up
  to fill the page.
