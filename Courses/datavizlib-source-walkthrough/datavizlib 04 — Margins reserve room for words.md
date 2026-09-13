---
type: unit
course: "[[Inside datavizlib's Source]]"
status: not-started
order: 4
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Before any data is drawn, `subplots_adjust` carves a hole in the figure for the plot, and leaves strips for title, subtitle, x-label, and footnote.

`_configure_layout` runs from the constructor. It does not look at the data. It only looks at whether `subtitle` is set and whether `x_label` is a non-blank string.

self.fig.subplots\_adjust(
    left=0.0,
    right=1.0,
    top=top\_margin,      # 0.80 with subtitle, else 0.88
    bottom=bottom\_margin # 0.15 with x\_label, else 0.10
)

Matplotlib’s `subplots_adjust` fractions are in **figure coordinates**: `0` is the bottom or left of the whole figure, `1` is the top or right. `top=0.80` means the Axes only go up to 80% of the figure height — the top 20% is empty on purpose, so `_add_title` can write at `y=0.91` or `0.94` without sitting on the data.

![[9c75e4b61d2c657b88b29c1d61df3bb902d0998f.svg]]

The grey rectangle is the Axes. Words live in the white bands. `left=0` and `right=1` give the plot the full width — which is why wide y-tick labels can clip. You already noted this in exercise 2.2.

**Example.** Same constructor, two subtitle choices:

BaseChart("Revenue", "Month", "EUR", "Source")           # top=0.88
BaseChart("Revenue", "Month", "EUR", "Source", "Actual") # top=0.80

The second chart’s plot is shorter so the subtitle at `y=0.86` has a gap above the Axes.

## Check yourself

What does a non-empty subtitle change?

**Primary source:** `_configure_layout` in [base.py](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/src/datavizlib/charts/base.py). Matplotlib: [`Figure.subplots_adjust`](https://matplotlib.org/stable/api/_as_gen/matplotlib.figure.Figure.subplots_adjust.html).

Ask if you want to walk through a proposed non-zero `left`/`right` without breaking title alignment in the next lessons.

[[datavizlib 03 — Collect, draw, then dress the figure|← Collect / draw / dress]] [[datavizlib 05 — Spines, ticks, and uppercase labels|Next: spines and type →]]
