```yaml
number: 7546
title: Questionable PD011 autofix
type: issue
state: closed
author: s-banach
labels: []
assignees: []
created_at: 2023-09-20T12:48:56Z
updated_at: 2023-09-20T12:52:28Z
url: https://github.com/astral-sh/ruff/issues/7546
synced_at: 2026-01-12T15:54:47Z
```

# Questionable PD011 autofix

---

_@s-banach_

(For reference, PD011 replaces `Series.values` with `Series.to_numpy()`.)
If somebody wrote `Series.values` in the first place, they probably had the intention of accessing the underlying data.
The correct way to do that is `Series.array`.
In fact, when I do a quick test, it looks like `Series.values` still returns the underlying array:
```python
import pyarrow as pa
import pandas as pd
s = pd.Series([1, 2, 3, None], dtype=pd.ArrowDtype(pa.int64()))
s.values
"""
<ArrowExtensionArray>
[1, 2, 3, <NA>]
Length: 4, dtype: int64[pyarrow]
"""
```

---

_Comment by @s-banach on 2023-09-20 12:51_

Wait, maybe this isn't an autofix? Sorry for being an idiot.

---

_Closed by @s-banach on 2023-09-20 12:51_

---

_Comment by @s-banach on 2023-09-20 12:52_

Oh, it's not an autofix, but the one-line suggestion to the user is `use .to_numpy()`.
I do find that strange.

---
