```yaml
number: 13069
title: Red Knot panics when function return type and function name are the same
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - ty
assignees: []
created_at: 2024-08-23T06:42:19Z
updated_at: 2024-08-24T02:31:37Z
url: https://github.com/astral-sh/ruff/issues/13069
synced_at: 2026-01-10T11:09:55Z
```

# Red Knot panics when function return type and function name are the same

---

_Issue opened by @MichaReiser on 2024-08-23 06:42_

Minimal Repro

```python
def x(self) -> x:
    ...
```

The original code from the pandas project

```python
    # https://github.com/python/mypy/issues/4125
    # error: Signature of "type" incompatible with supertype "BaseMaskedDtype"
    # @property
    def type(self) -> type:  # type: ignore[override]
        return np.bool_
```

https://github.com/pandas-dev/pandas/blob/0c24b20bd9cd743bd5f5afceb1a6fef54a2dbdc1/pandas/core/arrays/boolean.py#L76-L78

---

_Label `bug` added by @MichaReiser on 2024-08-23 06:42_

---

_Label `red-knot` added by @MichaReiser on 2024-08-23 06:42_

---

_Closed by @carljm on 2024-08-24 02:31_

---

_Closed by @carljm on 2024-08-24 02:31_

---
