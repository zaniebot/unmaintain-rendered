```yaml
number: 17139
title: "PD010: Pivot on values that are not a DataFrame getting flagged as Pandas error"
type: issue
state: closed
author: Rebellenmepper
labels: []
assignees: []
created_at: 2025-04-02T06:21:40Z
updated_at: 2025-04-02T06:50:15Z
url: https://github.com/astral-sh/ruff/issues/17139
synced_at: 2026-01-12T15:54:55Z
```

# PD010: Pivot on values that are not a DataFrame getting flagged as Pandas error

---

_@Rebellenmepper_

### Summary

For example when I use a Polars DataFrame, I get this error. However, I also get this error if I turn it to something nonsensical like 1 or 2. It seems like the code is too focused on pivot as a method name regardless of the class type. Below code yields: `.pivot_table` is preferred to `.pivot` or `.unstack`; provides same functionalityRuff[PD010](https://docs.astral.sh/ruff/rules/pandas-use-of-dot-pivot-or-unstack)
```
import pandas as pd
import polars as pl

def pivot_func(df: pl.DataFrame) -> pl.DataFrame:
    df: pl.DataFrame = df.pivot(values=["value1", "value2"], index=["index1", "index2"], columns="col")
    return df


def pivot_func2(abc: pl.DataFrame) -> pl.DataFrame:
    abc = 123
    df: pl.DataFrame = abc.pivot(values=["value1", "value2"], index=["index1", "index2"], columns="col")
    return df
```

### Version

2025.22.0 (VSCode extension)

---

_Closed by @MichaReiser on 2025-04-02 06:50_

---
