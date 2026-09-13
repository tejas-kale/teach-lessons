---
type: unit
course: "[[Inside datavizlib's Source]]"
status: not-started
order: 9
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Same collect-then-draw shape as lines, plus geometry: shared categories, offsets for groups, running `bottom`/`left` for stacks, `bar` vs `barh` for orientation.

`add_data` stores `df, x, y, label, color, alpha`. `color` may be one value or a list whose length equals the number of rows (one color per bar). A mismatched list raises.

## Shared categories are mandatory

Before drawing, `render` takes the first series’ `x` column as the category order. Every other series must have the *same values in the same order*. That is stricter than `LineChart`.

x\_values = self.\_bars\[0\]\["df"\]\[self.\_bars\[0\]\["x"\]\].tolist()
\# all other series must match exactly

Then `x = np.arange(len(x_values))` — categories become integers 0, 1, 2, … so grouped bars can be offset in data space. Tick labels put the original strings back.

## Grouped arithmetic

For `stacked=False`:

group\_width = min(0.9, width)
gap = inner\_gap \* group\_width
bar\_width = (group\_width - gap \* (n\_series - 1)) / n\_series
base\_offsets = x - group\_width/2 + bar\_width/2
series\_offsets = base\_offsets + idx \* (bar\_width + gap)

If the gaps would eat the whole group, gap is forced to `0`. Vertical uses `ax.bar`; horizontal uses `ax.barh` with the same offsets on the other axis.

![[278ede86dc482fbcc370c447dbb7a2575b73907d.svg]]

## Stacked uses a running baseline

Vertical: `bottom = zeros`, draw, `bottom += y_data`. Horizontal: the same with `left` and `ax.barh`. This is matplotlib’s documented stacked-bar pattern, not a custom compositor.

**Example.** From the tests — two series, stacked, horizontal:

chart.add\_data(df, x="category", y="missed", label="Missed", color="#D70000")
chart.add\_data(df, x="category", y="vaccinated", label="Vaccinated", color="#55C3F0")
fig, ax = chart.render(stacked=True, orientation="horizontal")

Call order is draw order: “Missed” is the base; “Vaccinated” sits to its right.

Per-bar colors (exercise 2.2 style): pass a list the same length as the frame, e.g. highlight “Direct Sales” in navy and mute the rest.

Legend appears only if `show_legend` is true *and* at least one series has a label. Default `width=0.8`, `inner_gap=0.02`, `orientation="vertical"`.

## Check yourself

What must every bar series share?

**Primary source:** [bar.py](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/src/datavizlib/charts/bar.py), matplotlib [`Axes.bar`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.bar.html), [grouped bar example](https://matplotlib.org/stable/gallery/lines_bars_and_markers/barchart.html), [stacked bar example](https://matplotlib.org/stable/gallery/lines_bars_and_markers/bar_stacked.html). Tests: [test\_bar.py](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/tests/test_bar.py).

If grouped vs stacked still blurs, ask with one of your DataFrames — we can print `series_offsets` or the running `bottom` for those rows.

[[datavizlib 08 — LineChart collects series, then plots them|← LineChart]] [[datavizlib 10 — A new chart type only fills the drawing hole|Next: extend BaseChart →]]
