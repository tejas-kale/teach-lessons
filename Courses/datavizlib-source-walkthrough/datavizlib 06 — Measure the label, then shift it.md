---
type: unit
course: "[[Inside datavizlib's Source]]"
status: not-started
order: 6
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

The library does not guess where a label sits. It draws, asks matplotlib for a pixel box, converts that box into axes coordinates, and adds a shift.

Matplotlib keeps several coordinate systems. The ones this file uses:

- **Axes** — `(0,0)` bottom-left of the plot, `(1,1)` top-right. `ax.transAxes`.
- **Figure** — `(0,0)` bottom-left of the whole page. `fig.transFigure`.
- **Display** — pixels (or points, depending on backend). `get_window_extent` returns this.

![[49446e2d8cd96a6fbc17a0ffc22fc29fd44ffea9.svg]]

Click the white frame, the inner plot, or the orange dot.

Three spaces, one figure. Alignment code hops between them on purpose.

## The y-label recipe

`_align_y_label_to_tick`:

1.  Park the label at `(-0.10, 0.5)` in axes coordinates — a known parking spot.
2.  `draw()` so the renderer can measure text.
3.  `get_window_extent` → transform with `ax.transAxes.inverted()` to get a box in axes space.
4.  `shift = 1.0 - bbox_axes.y1` — how far the top of the text is from the top of the Axes.
5.  Set coords to `(-0.10, 0.5 + shift)`.

The x-label recipe is the same idea on the other axis: park at `(0.5, -0.10)`, measure left edge `x0`, shift so `x0` meets `0.0`.

**Example.** Suppose after parking, the y-label’s top is at axes `y = 0.62`. Then `shift = 1.0 - 0.62 = 0.38`, and the new y is `0.5 + 0.38 = 0.88`. The top of “MEALS SERVED” now kisses the top of the axis line.

The *x* of the y-label stays `-0.10`. That is why a long tick like `300,000` can collide with the label — the parking x does not depend on tick width. You already wrote that down in the 2.2 notebook.

`fig.canvas.draw()` is not decorative. Without a draw, `get_window_extent` can be empty or stale. That is why `build()` draws, aligns, then draws again.

## Check yourself

In which space is the y-label shift computed?

**Primary source:** Matplotlib [Transformations tutorial](https://matplotlib.org/stable/users/explain/artists/transforms_tutorial.html) and [`Axis.set_label_coords`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axis.Axis.set_label_coords.html). Then `_align_y_label_to_tick` / `_align_x_label_to_tick` in [base.py](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/src/datavizlib/charts/base.py).

If the parking coordinate `-0.10` is the hatch you want, ask — we can trace it against a real tick bbox next.

[[datavizlib 05 — Spines, ticks, and uppercase labels|← Spines]] [[datavizlib 07 — Title left-aligns to axis words; footnote to the plot|Next: title and footnote anchors →]]
