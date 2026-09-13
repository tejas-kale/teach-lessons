---
type: unit
course: "[[Inside datavizlib's Source]]"
status: not-started
order: 10
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

You have now seen every file in `src/`. Extending the library means repeating the `LineChart` / `BarChart` pattern, not copying `build()`.

The two subclasses are the same shape:

class SomethingChart(BaseChart):
    def \_\_init\_\_(...):
        super().\_\_init\_\_(...)
        self.\_items = \[\]

    def add\_data(self, df, ...):
        # validate DataFrame + columns
        self.\_items.append({...})
        return self

    def render(self, ...):
        # ax.scatter / ax.plot / ...
        return self.build()

![[184788954f6e6d9c8a2b00ffda00c5a83ce37c22.svg]]

## Where not to put new behavior

- **Scale and tick format** (your 2.2 hatch) are Axes concerns after data exists. They belong next to `render()`, or as kwargs on it — not inside `_align_y_label_to_tick`.
- **Layout collisions** (left margin, y-label `-0.10`, footnote `y=0`) belong in `base.py`, because every chart inherits them.
- **A one-off annotation** can stay in the notebook on the returned `ax`. That is why `render` returns `(fig, ax)`.

**Example.** A slopegraph is two x positions and several lines. You would still collect rows in `add_data`, call `ax.plot` in `render`, and let `build()` dress the page. You would not reimplement spines.

To make the class importable: add the class in a new module under `charts/`, then add the name to `charts/__init__.py` and `datavizlib/__init__.py` `__all__` lists. That is the entire public-API ritual from Source 0001.

## Check yourself

Where should a scatter subclass call build()?

**Primary source:** the two existing subclasses side by side — [line.py](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/src/datavizlib/charts/line.py) and [bar.py](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/src/datavizlib/charts/bar.py) — plus the `NotImplementedError` in [`BaseChart.add_data`](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/src/datavizlib/charts/base.py).

This finishes a first pass over every file in `src/`. Ask about the next hatch (limits, formatting, margins) or we can drill one method until it is boring.

[[datavizlib 09 — BarChart groups, stacks, and flips|← BarChart]]
