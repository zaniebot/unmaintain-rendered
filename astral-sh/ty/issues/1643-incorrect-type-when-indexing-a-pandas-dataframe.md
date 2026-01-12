```yaml
number: 1643
title: "Incorrect type when indexing a pandas dataframe through `.loc` and selecting a single column"
type: issue
state: closed
author: diego-pm
labels:
  - bug
  - type-inference
assignees: []
created_at: 2025-11-26T08:47:55Z
updated_at: 2025-11-28T19:38:25Z
url: https://github.com/astral-sh/ty/issues/1643
synced_at: 2026-01-12T15:54:25Z
```

# Incorrect type when indexing a pandas dataframe through `.loc` and selecting a single column

---

_@diego-pm_

### Summary

I have been searching for issues related to this but haven't found this case. Some pandas related errors mentioned in the issues stated that it was due to `ty` not supporting `TypeAlias`, but the current version 0.0.1-alpha.28 does support it (and I have seen many errors related to pandas disappear in my codebase due to this but not this one). Maybe the error is due to a missing feature in `ty` that I am not aware of.

The issue is that when selecting a column trought `.loc` in a pandas dataframe, the infered type should be Series, but ty infers a DataFrame.

```python
from typing_extensions import reveal_type
import pandas as pd

def foo(s: pd.Series[int], df: pd.DataFrame):
    reveal_type(df.loc[[1, 2], "col1"]) # revealed type: DataFrame
```

I have tested this also with pyright and it correctly reveals that the type is `Series`.


Setup:

Used `ty check file.py`, the default configuration was not modified.

pandas version: 2.2.1
pandas-stubs version: 2.2.1.240316
python version: 3.10.16

### Version

ty 0.0.1-alpha.28

---

_Comment by @sharkdp on 2025-11-26 09:06_

Thank you for the detailed report!

We did implement support for implicit type aliases and PEP 613 `TypeAlias`es in the latest version, but we still don't have support for generic implicit/PEP-613 type aliases. We plan to add support for them soon. In fact, we do infer `Series[Any]` on [this feature branch](https://github.com/astral-sh/ruff/pull/21553).

---

_Label `bug` added by @sharkdp on 2025-11-26 09:07_

---

_Label `type-inference` added by @sharkdp on 2025-11-26 09:07_

---

_Assigned to @sharkdp by @sharkdp on 2025-11-26 09:07_

---

_Added to milestone `Beta` by @carljm on 2025-11-26 18:56_

---

_Closed by @sharkdp on 2025-11-28 19:38_

---
