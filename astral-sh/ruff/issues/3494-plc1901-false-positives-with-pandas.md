```yaml
number: 3494
title: PLC1901 false-positives with pandas
type: issue
state: closed
author: twoertwein
labels: []
assignees: []
created_at: 2023-03-13T21:45:27Z
updated_at: 2023-03-17T02:52:07Z
url: https://github.com/astral-sh/ruff/issues/3494
synced_at: 2026-01-10T11:09:46Z
```

# PLC1901 false-positives with pandas

---

_Issue opened by @twoertwein on 2023-03-13 21:45_

```py
import pandas as pd

data =  pd.DataFrame({"a": ["", "abc", "xyz"]})

data.loc[data["a"] != ""]  # PLC1901
# data.loc[not data["a"]]   # ValueError at runtime
```
An option to avoid this false-positive would be to ignore `!= ""` comparisons when they are inside `[ ]`.

edit: or nudge people to `data.loc[data["a"].ne("")] `


---

_Closed by @charliermarsh on 2023-03-17 02:52_

---
