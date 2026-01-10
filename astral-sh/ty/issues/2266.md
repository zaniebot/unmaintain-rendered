---
number: 2266
title: "Built-in operators on `JustFloat`s should return `JustFloat`."
type: issue
state: open
author: Jammf
labels:
  - typing semantics
assignees: []
created_at: 2025-12-29T22:45:48Z
updated_at: 2025-12-30T07:30:51Z
url: https://github.com/astral-sh/ty/issues/2266
synced_at: 2026-01-10T01:51:14Z
---

# Built-in operators on `JustFloat`s should return `JustFloat`.

---

_Issue opened by @Jammf on 2025-12-29 22:45_

### Summary

(Based on https://github.com/astral-sh/ty/issues/2184#issuecomment-3697621323)

Ty reports the sum of two `JustFloat` values is `int | float`. I would have expected it to be another `JustFloat`, given that result is always a `float` at runtime.

```python
a = 0.1
b = 0.2
reveal_type(a)  # float
reveal_type(b)  # float

c = a + b
reveal_type(c)  # int | float
```

https://play.ty.dev/a7945e32-cb7d-4765-bf50-d3b2b711b6ed



### Version

ty 0.0.8 (aa7559db8 2025-12-29)

---

_Comment by @carljm on 2025-12-29 22:57_

Thanks for reporting this! Barring a change to typeshed (or a change in our handling of `float` annotations generally) we would need to special-case methods known to return just-float (or patch our copy of typeshed.)

---

_Label `typing semantics` added by @MichaReiser on 2025-12-30 07:30_

---
