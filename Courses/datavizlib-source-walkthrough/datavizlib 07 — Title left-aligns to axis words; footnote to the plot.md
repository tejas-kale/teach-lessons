---
type: unit
course: "[[Inside datavizlib's Source]]"
status: not-started
order: 7
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Titles are not `ax.set_title`. They are `fig.text` in figure coordinates, lined up with whichever y-axis words are actually visible.

## \_get\_ylabel\_bbox

This helper returns a dict of figure-space edges `x0, x1, y0, y1`. The rule:

1.  If there is a y-label, measure that text’s window extent.
2.  Else measure the y tick labels and `Bbox.union` them (horizontal bar charts often live here: category names on the y-axis, no “Y” title).
3.  If there are no tick labels either, fall back to the left edge of the Axes, converted into figure coordinates.

`_add_title` and `_add_subtitle` both take `x = ylabel_bbox["x0"]` so the title stack shares a left edge with that content. Title `y` is `0.91` with a subtitle, `0.94` without. Subtitle is always `y=0.86` and grey.

![[0656c7ee9b55e28617467f79d2db6509d9b565d9.svg]]

Dashed orange: shared `x0` of the y-label bbox. Navy dot: axes origin used by the footnote.

## Footnote is a different anchor

`_add_footnote` does *not* use the y-label bbox. It maps Axes `(0, 0)` through `ax.transAxes` into display, then `fig.transFigure.inverted()` into figure space, and writes at `y=0.00`. So the footnote lines up with the start of the x-axis, even if the title is further left because of a wide y-label.

**Example.** Horizontal bars, `y_label=""`: titles align to category tick text. Footnote still sits under the plot’s left, not under “Direct Sales”. Two anchors, two jobs.

Default `fig.text` baseline at `y=0.00` can clip descenders (the tail of a “g”). That is the footnote hatch you noted: `va="bottom"` a little above zero would keep source lines on the page.

## Check yourself

What is the title’s x aligned to?

**Primary source:** `_get_ylabel_bbox`, `_add_title`, `_add_subtitle`, `_add_footnote` in [base.py](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/src/datavizlib/charts/base.py). `Bbox.union` is matplotlib’s [transforms.Bbox](https://matplotlib.org/stable/api/transformations.html#matplotlib.transforms.Bbox.union).

Unclear why a title jumped when you dropped the y-label? Ask — we can replay the three branches of `_get_ylabel_bbox` on one of your figures.

[[datavizlib 06 — Measure the label, then shift it|← Transforms]] [[datavizlib 08 — LineChart collects series, then plots them|Next: LineChart →]]
