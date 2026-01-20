```yaml
number: 2569
title: ty highlights the entire class in diagnostic to indicate type variable
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - diagnostics
assignees: []
created_at: 2026-01-20T04:18:21Z
updated_at: 2026-01-20T05:06:38Z
url: https://github.com/astral-sh/ty/issues/2569
synced_at: 2026-01-20T05:34:07Z
```

# ty highlights the entire class in diagnostic to indicate type variable

---

_@dhruvmanila_

### Summary

For the following code:

```py
from narwhals.typing import FrameT


def fn(frame: FrameT) -> FrameT:
    return frame.with_columns(nw.lit("hello world").alias("foo"))
```

Running `ty check` will output a `invalid-argument-type` diagnostic which has a "info" part that highlights the entire class:

```
error[invalid-argument-type]: Argument to bound method `with_columns` is incorrect
 --> main.py:5:12
  |
4 | def fn(frame: FrameT) -> FrameT:
5 |     return frame.with_columns(nw.lit("hello world").alias("foo"))
  |            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Argument type `FrameT@fn` does not satisfy upper bound `LazyFrame[LazyFrameT@LazyFrame]` of type variable `Self`
  |
info: Type variable defined here
    --> .venv/lib/python3.13/site-packages/narwhals/dataframe.py:2354:1
     |
2354 | / class LazyFrame(BaseFrame[LazyFrameT]):

...
```

Here, ty highlights the entire `LazyFrame` class which is like 1.3k lines long.

### Version

ty 0.0.12 (4b74e4ded 2026-01-14)

_Originally reported by @danielgafni in https://github.com/astral-sh/ty/issues/1503#issuecomment-3753866351_

---

_Label `diagnostics` added by @dhruvmanila on 2026-01-20 04:18_

---

_Label `bug` added by @dhruvmanila on 2026-01-20 04:19_

---

_Comment by @dhruvmanila on 2026-01-20 05:06_

Oops, this was fixed in https://github.com/astral-sh/ruff/pull/22646

---

_Closed by @dhruvmanila on 2026-01-20 05:06_

---
