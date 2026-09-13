---
type: unit
course: "[[Inside datavizlib's Source]]"
status: not-started
order: 3
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Subclasses own data. `BaseChart.build()` owns spines, labels, title, and footnote. `render()` is the subclass method that draws and then calls `build()`.

`BaseChart.add_data` always raises `NotImplementedError`. That is the seam: “you must say how this chart type eats a DataFrame.” `LineChart` and `BarChart` override it to *append a dict* onto `self._lines` or `self._bars` and return `self` so you can chain calls.

Neither subclass draws in `add_data`. Drawing happens in `render()`, which ends with `return self.build()`.

![[1ab0886780e6c57b130bdb50d6599de3e5c51023.svg]]

Template-method shape: subclasses fill in drawing; the base class finishes the page.

## A real meals-served call

This is the shape you already use in the exercise 2.2 notebook:

chart = BarChart(
    title="Meals served over time",
    subtitle="# of meals served",
    x\_label="Campaign year",
    y\_label="Meals served",
    footnote=FOOT,
)
chart.add\_data(df, x="Campaign Year", y="Meals Served", color=GREEN)
fig, ax = chart.render(width=0.7, show\_legend=False)
ax.set\_ylim(0, 300\_000)  # hatch: scale is not in src/ yet

`render` returns the same `(fig, ax)` pair `build()` returns, so you can still reach matplotlib after the library has dressed the chart.

## The README trap

The project README calls `chart.build()` *before* `add_data`. The tests sometimes do too. `build()` writes title text onto the figure. `render()` calls `build()` again. Two titles. Trust the source: `LineChart.render` and `BarChart.render` both end with `return self.build()`.

## Check yourself

When does a line actually appear on the Axes?

**Primary source:** `add_data` in [base.py](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/src/datavizlib/charts/base.py), then the `render` methods in [line.py](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/src/datavizlib/charts/line.py) and [bar.py](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/src/datavizlib/charts/bar.py).

If you want to hatch `y_limits` or tick formatting, ask where it should live: `render()`, `build()`, or a new method. That choice is the whole design.

[[datavizlib 02 — Every chart starts as a story frame|← Constructor]] [[datavizlib 04 — Margins reserve room for words|Next: margins →]]
