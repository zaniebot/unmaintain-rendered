```yaml
number: 851
title: Incorrectly inferred Pandas indexing return type
type: issue
state: closed
author: kormalev
labels: []
assignees: []
created_at: 2025-07-18T11:18:57Z
updated_at: 2025-11-12T01:57:35Z
url: https://github.com/astral-sh/ty/issues/851
synced_at: 2026-01-10T02:06:24Z
```

# Incorrectly inferred Pandas indexing return type

---

_Issue opened by @kormalev on 2025-07-18 11:18_

### Summary

ty incorrectly thinks Pandas boolean indexing returns Series.

Reproducible example:
```python
import pandas as pd


def filter_data(data: pd.DataFrame) -> pd.DataFrame:
    return data[(data["a"] == 1)]


data = pd.DataFrame(
    {
        "a": [1, 2, 3, 4],
        "b": [4, 3, 2, 1],
    }
)

print(filter_data(data))
```

```text
> ty version
ty 0.0.1-alpha.14 (3ececb07e 2025-07-08)
> ty check pandas-demo.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[invalid-return-type]: Return type does not match returned value
 --> pandas-demo.py:4:40
  |
4 | def filter_data(data: pd.DataFrame) -> pd.DataFrame:
  |                                        ------------ Expected `DataFrame` because of return type
5 |     return data[data["a"] == 1]
  |            ^^^^^^^^^^^^^^^^^^^^ expected `DataFrame`, found `Series[Any]`
  |
info: rule `invalid-return-type` is enabled by default

Found 1 diagnostic
```

Pandas version is 2.3.0

### Version

_No response_

---

_Comment by @carljm on 2025-07-18 23:29_

I think this is because `Scalar` is a type alias defined using `TypeAlias`, which we don't support yet (#544), which means we match the wrong overload of `__getitem__`

---

_Closed by @carljm on 2025-07-18 23:29_

---

_Comment by @carljm on 2025-11-12 01:57_

Confirmed this will be fixed by https://github.com/astral-sh/ruff/pull/21394

---
