---
type: unit
course: "[[Inside datavizlib's Source]]"
status: not-started
order: 1
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Everything importable as `datavizlib` is five Python files. Two of them only re-export names.

The library is an *src layout* package. `pyproject.toml` tells setuptools that the import root is `src/`, not the repo root. That is why `from datavizlib import BarChart` loads `src/datavizlib/…`, not a top-level `datavizlib.py`.

![[d0dc0750be1cac0f856157625ac31d120cb00cef.svg]]

Two `__init__.py` files. One implementation hierarchy.

## What `from datavizlib import BarChart` actually does

Python loads `src/datavizlib/__init__.py`. That file imports from the submodule and lists the names in `__all__`:

from datavizlib.charts.bar import BarChart
from datavizlib.charts.base import BaseChart
from datavizlib.charts.line import LineChart

\_\_all\_\_ = \["BaseChart", "BarChart", "LineChart"\]

`charts/__init__.py` does the same job for `from datavizlib.charts import BarChart`. Neither file draws a chart. They are doorbells: they decide which class names are public.

**Example.** After `pip install -e .`, both of these resolve to the same class object:

from datavizlib import LineChart
from datavizlib.charts import LineChart as Also

`LineChart is Also` is `True`. There is one class, two import paths.

## What is not in `src/`

Tests live in `tests/`. Storytelling notes live in `docs/`. Your practice notebooks live in `learning/`. None of those are imported when someone does `import datavizlib`.

## Check yourself

Which file runs first for from datavizlib import BarChart?

The three choices are padded so they share length. There is no `charts.py` or `base.py__` — those names are distractors for the real files.

**Primary source:** [PyPA, src layout vs flat layout](https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/), then read [`src/datavizlib/__init__.py`](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/src/datavizlib/__init__.py) and the `tool.setuptools` block in [`pyproject.toml`](https://github.com/phmelzer/datavizlib/blob/api-exploration-tejas/pyproject.toml).

If any import path is unclear, ask the teacher. That is the point of this workspace.

Start [[datavizlib 02 — Every chart starts as a story frame|Next: the BaseChart contract →]]
