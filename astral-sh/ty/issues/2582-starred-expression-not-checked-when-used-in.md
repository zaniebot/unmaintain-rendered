```yaml
number: 2582
title: Starred expression not checked when used in function argument
type: issue
state: open
author: abgros
labels:
  - bug
  - runtime semantics
assignees: []
created_at: 2026-01-21T23:13:05Z
updated_at: 2026-01-21T23:45:31Z
url: https://github.com/astral-sh/ty/issues/2582
synced_at: 2026-01-22T00:08:39Z
```

# Starred expression not checked when used in function argument

---

_@abgros_

### Summary

This incorrect code passes type checking:

```python
def some_fn(a: int):
    pass

some_fn(*None)
```
But fails at runtime:
```
TypeError: some_fn() argument after * must be an iterable, not NoneType
```

On the other hand, this is correctly flagged as an error:
```python
b = *None
```

https://play.ty.dev/952eb26e-1acb-481f-b419-8e15f3c2a84a

### Version

ty 0.0.7

---

_Comment by @AlexWaygood on 2026-01-21 23:21_

Thanks! Definitely a bug.

---

_Label `bug` added by @AlexWaygood on 2026-01-21 23:21_

---

_Label `runtime semantics` added by @AlexWaygood on 2026-01-21 23:21_

---

_Added to milestone `Stable` by @AlexWaygood on 2026-01-21 23:21_

---

_Comment by @denyszhak on 2026-01-21 23:30_

@AlexWaygood Would love to give this one a shot

---

_Comment by @carljm on 2026-01-21 23:45_

@denyszhak sure

---

_Assigned to @denyszhak by @carljm on 2026-01-21 23:45_

---
