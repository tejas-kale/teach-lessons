---
type: unit
course: "[[Inside datavizlib's Source]]"
status: not-started
order: 2
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

`BaseChart` refuses to exist without a title and a footnote. Axis labels are optional. A matplotlib figure is created immediately.

Open `src/datavizlib/charts/base.py`. The constructor signature is:

def \_\_init\_\_(self, title, x\_label, y\_label, footnote, subtitle=None):

`_validate_inputs` only checks `title` and `footnote`. Empty `x_label` or `y_label` is allowed — the tests say so, because some storytelling charts drop an axis title on purpose.

**Example.** This raises. The README snippet that omits `footnote` would too:

BaseChart(title="Meals served", x\_label="Year", y\_label="Meals", footnote="")

This is legal, and later layout code treats the empty y-label as “align the title to tick labels instead”:

BaseChart(title="Meals served", x\_label="Year", y\_label="", footnote="Source: Knaflic")

## What is created before you add any data

The constructor calls `plt.subplots()`, stores the strings, then `_configure_layout()`. You already have a `fig` and an `ax` with no artists on them. Colors are copied from `ALDI_PALETTE`. Grey `#808080` is the “background” ink for ticks, leftover spines, and the subtitle — navy/orange stay free for data.

![[4cc20d2bbf8ea3002cd2cf7b33fb8490c2b7359c.svg]]

## The palette helper you will see in subclasses

`_get_next_color(index)` wraps with modulo. Line charts pass `len(self._lines)` so the second series gets the second swatch. Bar charts pass the series index in `render()`. If you pass an explicit `color=`, the helper is skipped.

## Check yourself

Which pair must be non-empty to construct a BaseChart?

**Primary source:** [`BaseChart.__init__` and `_validate_inputs`](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/src/datavizlib/charts/base.py), plus [`tests/test_base.py`](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/tests/test_base.py). Context for why a chart starts with words: [Start with context, not the chart](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/docs/principles/01-start-with-context-not-the-chart.md).

If the required-fields rule feels too strict or too loose for a chart you want to hatch, ask. We can look at the exact `raise` paths together.

[[datavizlib 01 — The five files and the public names|← Layout of src/]] [[datavizlib 03 — Collect, draw, then dress the figure|Next: add\_data, render, build →]]
