---
number: 2143
title: "PD010: pivot table false positive"
type: issue
state: open
author: sbrugman
labels:
  - bug
  - type-inference
assignees: []
created_at: 2023-01-24T23:42:23Z
updated_at: 2025-04-22T18:55:36Z
url: https://github.com/astral-sh/ruff/issues/2143
synced_at: 2026-01-07T13:12:14-06:00
---

# PD010: pivot table false positive

---

_Issue opened by @sbrugman on 2023-01-24 23:42_

When a Spark DataFrame is used, PD010 will still generate an error (pandas and spark are often used within the same file):

```python
import pandas as pd
from pyspark.sql import DataFrame

dataframe.groupBy("my_column")
  .pivot("other_column")
```

> PD010 `.pivot_table` is preferred to `.pivot` or `.unstack`; provides same functionality

### Suggested solution
When type inference is too complicated, do not consider this rule when `pyspark` is in the imports.

---

_Label `bug` added by @charliermarsh on 2023-01-25 04:46_

---

_Label `type-inference` added by @charliermarsh on 2023-05-18 15:57_

---

_Comment by @leuchtum on 2023-11-05 10:44_

Another false positive, as `pd.Series` has no method `pivot_table`. The method only exist on `pd.DataFrames`.

Example:
```python
import pandas as pd

idx1 = [1, 2, 3, 4, 5]
idx2 = [10, 20, 30, 40, 50]
multi_idx = pd.MultiIndex.from_arrays([idx1, idx2])
s = pd.Series(data=[100, 200, 300, 400, 500], index=multi_idx)

unstacked = s.unstack()
```

Output:
```shell
$ ruff demo_ruff_PD010.py --select=PD010
demo_ruff_PD010.py:8:13: PD010 `.pivot_table` is preferred to `.pivot` or `.unstack`; provides same functionality
Found 1 error.
```


---

_Comment by @janosh on 2024-03-22 15:05_

here's a slightly simpler repro for multiple false-positive PD010 on `pd.Series`

```py
import pandas as pd

df = pd.DataFrame({"A": [1, 2, 3]})
df["A"].unstack()  # noqa: PD010
df.A.unstack()  # noqa: PD010

pd.Series([1,2,3]).unstack()  # noqa: PD010
```

---

_Comment by @anor4k on 2025-04-22 18:55_

Also get a false positive on polars DataFrames.

---
