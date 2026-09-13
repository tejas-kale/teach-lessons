---
type: unit
course: "[[Inside datavizlib's Source]]"
status: not-started
order: 8
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

The whole file is a list of dicts plus one loop over `ax.plot`. Color is chosen at collect time, not at draw time.

`LineChart.__init__` calls `super().__init__(…)` then sets `self._lines = []`. That list is the entire extra state.

## add\_data validates, then appends

It requires a pandas DataFrame and column names that exist. If `color is None`, it assigns `self._get_next_color(len(self._lines))` *before* appending, so the first line is palette\[0\], the second is palette\[1\], even if you never call `render` in between.

**Example.** Actual vs forecast on the same months:

chart = LineChart(
    title="Monthly revenue",
    x\_label="Month",
    y\_label="EUR",
    footnote="Source: finance extract",
    subtitle="Actual vs forecast",
)
chart.add\_data(df, x="month", y="actual", linestyle="solid", label="Actual")
chart.add\_data(df, x="month", y="forecast", linestyle="dotted", label="Forecast")
fig, ax = chart.render()

Two dicts in `_lines`. Two `ax.plot` calls. Because any dict has a `label`, `render` adds a legend with `frameon=False`.

## render is a thin loop

No data → `ValueError("No data added. Call add_data() first.")`. Otherwise, for each stored dict, it passes matplotlib’s own names through: `linestyle`, `linewidth`, `marker`, `markersize`, `alpha`, `label`. Then `return self.build()`.

There is no grouping logic and no shared-category check. Lines may have different x values; matplotlib will just plot what you give it.

![[4697a55cc8efedb7b8f128f6729617a36d829bbf.svg]]

## Check yourself

When is an automatic line color chosen?

**Primary source:** [line.py](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/src/datavizlib/charts/line.py) and matplotlib [`Axes.plot`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.plot.html). Tests: [test\_line.py](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/tests/test_line.py).

Want a second y-scale or a direct-label instead of a legend? Ask before stuffing it into `render()` — that method is currently one loop on purpose.

[[datavizlib 07 — Title left-aligns to axis words; footnote to the plot|← Titles]] [[datavizlib 09 — BarChart groups, stacks, and flips|Next: BarChart →]]
