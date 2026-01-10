```yaml
number: 1075
title: Incorrect inferred type when indexing Pandas dataframe with column list
type: issue
state: closed
author: janosh
labels: []
assignees: []
created_at: 2025-08-21T05:21:16Z
updated_at: 2025-11-12T01:54:38Z
url: https://github.com/astral-sh/ty/issues/1075
synced_at: 2026-01-10T02:06:24Z
```

# Incorrect inferred type when indexing Pandas dataframe with column list

---

_Issue opened by @janosh on 2025-08-21 05:21_

### Summary

```py
import pandas as pd

df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6], 'C': [7, 8, 9]})
cols = ['A', 'B']
sub_df = df[cols] # inferred type is Series[Any] but should be DataFrame[Any] or maybe even DataFrame[int]
```


```
$ uv pip show pandas
Using Python 3.13.0 environment at: /Users/janosh/.venv/py313
Name: pandas
Version: 2.3.1
```

maybe slightly related https://github.com/astral-sh/ty/issues/851

### Version

ty 0.0.1-alpha.19 (e9cb838b3 2025-08-19)

---

_Comment by @AlexWaygood on 2025-08-21 13:19_

Thanks for the report! Yes, this is exactly the same as #851 -- we don't support type alias yet, and pandas-stubs uses type aliases for its `DataFrame.__getitem__` overloads; these two things together mean we end up picking the wrong overload

---

_Closed by @AlexWaygood on 2025-08-21 13:19_

---

_Comment by @carljm on 2025-11-12 01:54_

Confirmed this will be fixed by https://github.com/astral-sh/ruff/pull/21394

---
