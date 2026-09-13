---
type: unit
course: "[[Inside datavizlib's Source]]"
status: not-started
order: 5
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

`build()` first removes chart chrome, then styles the words that remain. Data is already on the Axes when this runs (from `render()`).

The order inside `build()` is deliberate:

self.\_clean\_spines()
self.\_add\_axis\_labels()
self.ax.tick\_params(..., colors=self.background\_color, ...)
self.fig.canvas.draw()          # so bounding boxes exist
\# align labels if they were drawn
self.fig.canvas.draw()          # again after moving labels
self.\_add\_title()
self.\_add\_subtitle()
self.\_add\_footnote()

## What “clean” means here

`_clean_spines` hides the top and right spines. Left and bottom stay, but their color is the grey `background_color`, not black. Tick labels use the same grey. That matches this repo’s “remove redundant structure” pattern: keep the two spines you need as a faint frame, not a box.

![[f3f0e90718105a7208658a5773704087281e575e.svg]]

Top and right vanish. Left and bottom stay, in grey.

## Axis labels are optional and loud

If an axis label is non-blank, `_add_axis_labels` sets it with `.upper()`, left-aligned, grey, 10pt. Horizontal alignment `ha="left"` plus later `set_label_coords` is how the y-label’s *top* can sit at the top of the axis instead of centering on the spine.

Empty labels skip `set_xlabel` / `set_ylabel` entirely so matplotlib does not reserve space for a ghost title.

**Example.** You type `y_label="Meals served"`. The figure shows `MEALS SERVED`. That is not pandas. It is this one `.upper()` call.

## Check yourself

What happens to the four spines?

**Primary source:** `build`, `_clean_spines`, `_add_axis_labels` in [base.py](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/src/datavizlib/charts/base.py). Why: [Remove redundant structure](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/docs/patterns/remove-redundant-structure.md) and [Reduce cognitive load](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/docs/principles/reduce-cognitive-load.md).

If a hatch needs a visible top spine or mixed-case labels, ask before forking `build()` — that method is shared by every chart type.

[[datavizlib 04 — Margins reserve room for words|← Margins]] [[datavizlib 06 — Measure the label, then shift it|Next: measure then shift →]]
