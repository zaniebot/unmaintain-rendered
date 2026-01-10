```yaml
number: 14301
title: "[PD010] - False positives with `polars`dataframes"
type: issue
state: closed
author: jlopezpena
labels: []
assignees: []
created_at: 2024-11-12T23:09:58Z
updated_at: 2024-11-12T23:11:10Z
url: https://github.com/astral-sh/ruff/issues/14301
synced_at: 2026-01-10T11:09:56Z
```

# [PD010] - False positives with `polars`dataframes

---

_Issue opened by @jlopezpena on 2024-11-12 23:09_

`ruff` version 0.7.3, `pandas` linting rules `PD` can trigger when dealing with objects from other libraries, like polars.

Minimal example:

```python
import polars as pl

pl_df = pl.DataFrame([{"a": 1, "b": 2}, {"a": 3, "b": 4}])
pl_df.pivot(on="a", values="b")
```

This code raises a warning for rule `[PD010](https://docs.astral.sh/ruff/rules/pandas-use-of-dot-pivot-or-unstack)`: 

```
`.pivot_table` is preferred to `.pivot` or `.unstack`; provides same functionality
```

The warning is invalid, because the dataframe is not a `pandas` dataframe (and `polars` does not even have a `pivot_table` method!) 


---

_Comment by @jlopezpena on 2024-11-12 23:11_

duplicate of https://github.com/astral-sh/ruff/issues/6432

---

_Closed by @jlopezpena on 2024-11-12 23:11_

---
