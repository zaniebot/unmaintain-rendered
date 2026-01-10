---
number: 8304
title: "[BUG] Ruff raises \"undefined name\" error in notebooks with objects imported atop"
type: issue
state: closed
author: baggiponte
labels:
  - question
assignees: []
created_at: 2023-10-28T13:24:19Z
updated_at: 2023-11-01T12:45:14Z
url: https://github.com/astral-sh/ruff/issues/8304
synced_at: 2026-01-10T01:22:48Z
---

# [BUG] Ruff raises "undefined name" error in notebooks with objects imported atop

---

_Issue opened by @baggiponte on 2023-10-28 13:24_

A project I maintain, `functime`, uses ruff to lint the notebooks. I just ran ruff on [this](https://github.com/neocortexdb/functime/blob/main/docs/notebooks/evaluation.ipynb) notebook and I get a slew of errors of imported objects (e.g. `pl`) that are flagged as undefined variables:

<details>

```
docs/notebooks/evaluation.ipynb:cell 6:2:5: F821 Undefined name `pl`
docs/notebooks/evaluation.ipynb:cell 6:4:19: F821 Undefined name `train_test_split`
docs/notebooks/evaluation.ipynb:cell 8:2:5: F821 Undefined name `pl`
docs/notebooks/evaluation.ipynb:cell 8:3:5: F821 Undefined name `pl`
docs/notebooks/evaluation.ipynb:cell 8:4:5: F821 Undefined name `pl`
docs/notebooks/evaluation.ipynb:cell 10:1:16: F821 Undefined name `snaive`
docs/notebooks/evaluation.ipynb:cell 12:1:9: F821 Undefined name `rank_point_forecasts`
docs/notebooks/evaluation.ipynb:cell 13:2:10: F821 Undefined name `plot_forecasts`
docs/notebooks/evaluation.ipynb:cell 13:3:21: F821 Undefined name `pl`
docs/notebooks/evaluation.ipynb:cell 13:4:32: F821 Undefined name `pl`
docs/notebooks/evaluation.ipynb:cell 15:1:9: F821 Undefined name `rank_point_forecasts`
docs/notebooks/evaluation.ipynb:cell 16:2:10: F821 Undefined name `plot_forecasts`
docs/notebooks/evaluation.ipynb:cell 16:3:21: F821 Undefined name `pl`
docs/notebooks/evaluation.ipynb:cell 16:4:32: F821 Undefined name `pl`
docs/notebooks/evaluation.ipynb:cell 18:2:14: F821 Undefined name `lightgbm`
docs/notebooks/evaluation.ipynb:cell 18:2:61: F821 Undefined name `detrend`
docs/notebooks/evaluation.ipynb:cell 20:1:9: F821 Undefined name `rank_point_forecasts`
docs/notebooks/evaluation.ipynb:cell 21:2:10: F821 Undefined name `plot_forecasts`
docs/notebooks/evaluation.ipynb:cell 21:3:21: F821 Undefined name `pl`
docs/notebooks/evaluation.ipynb:cell 21:4:27: F821 Undefined name `pl`
docs/notebooks/evaluation.ipynb:cell 23:1:9: F821 Undefined name `rank_point_forecasts`
docs/notebooks/evaluation.ipynb:cell 24:2:10: F821 Undefined name `plot_forecasts`
docs/notebooks/evaluation.ipynb:cell 24:3:21: F821 Undefined name `pl`
docs/notebooks/evaluation.ipynb:cell 24:4:27: F821 Undefined name `pl`
docs/notebooks/evaluation.ipynb:cell 26:1:9: F821 Undefined name `rank_residuals`
docs/notebooks/evaluation.ipynb:cell 27:2:10: F821 Undefined name `plot_residuals`
docs/notebooks/evaluation.ipynb:cell 27:3:30: F821 Undefined name `pl`
docs/notebooks/evaluation.ipynb:cell 29:1:9: F821 Undefined name `rank_residuals`
docs/notebooks/evaluation.ipynb:cell 30:2:10: F821 Undefined name `plot_residuals`
docs/notebooks/evaluation.ipynb:cell 30:3:30: F821 Undefined name `pl`
docs/notebooks/evaluation.ipynb:cell 32:1:10: F821 Undefined name `plot_fva`
docs/notebooks/evaluation.ipynb:cell 34:1:10: F821 Undefined name `plot_comet`
```

</details>

Our configs:

```toml
[tool.ruff]
include = ["*.py", "*.pyi", "**/pyproject.toml", "*.ipynb"]
select = [
    "E",  # pycodestyle errors
    "W",  # pycodestyle warnings
    "F",  # pyflakes
    "I",  # isort
    "B",  # flake8-bugbear
    "UP", # pyupgrade
    "I",  # isort
]
ignore = [
    "E501", # line too long, handled by black
    "B008", # do not perform function calls in argument defaults
    "B905", # `zip()` without an explicit `strict=` parameter
]

[tool.ruff.pyupgrade]
# Preserve types, even if a file imports `from __future__ import annotations`.
keep-runtime-typing = true
```

---

_Comment by @charliermarsh on 2023-10-28 21:10_

\cc @dhruvmanila 

---

_Label `bug` added by @charliermarsh on 2023-10-28 21:10_

---

_Renamed from "[BUG] Ruff raises "undefined name" error in notebooks with objects imported from " to "[BUG] Ruff raises "undefined name" error in notebooks with objects imported atop of the notebook " by @baggiponte on 2023-10-28 21:31_

---

_Renamed from "[BUG] Ruff raises "undefined name" error in notebooks with objects imported atop of the notebook " to "[BUG] Ruff raises "undefined name" error in notebooks with objects imported atop" by @baggiponte on 2023-10-28 21:31_

---

_Comment by @dhruvmanila on 2023-11-01 06:40_

This is happening because the cell containing the imports has a cell magic defined:

```python
%%capture
import polars as pl

# ...
```


We ignore cell containing cell magics as they apply to the _entire_ cell. The reasoning behind that is mentioned here: https://github.com/astral-sh/ruff/issues/7908#issuecomment-1756749556

Separately, I'm not sure if `%%capture` is really useful in a cell containing imports as there probably won't be any output when running the cell.

I'll close this considering this as expected but feel free to ask any further questions or clarification required.

---

_Closed by @dhruvmanila on 2023-11-01 06:40_

---

_Label `bug` removed by @dhruvmanila on 2023-11-01 06:40_

---

_Label `question` added by @dhruvmanila on 2023-11-01 06:40_

---

_Comment by @baggiponte on 2023-11-01 12:12_

Ops totally right, sorry @dhruvmanila. Thank you for the prompt answers!

---

_Comment by @dhruvmanila on 2023-11-01 12:45_

No problem, happy to help :)

---
