```yaml
number: 17904
title: "ty: dict should be compatible with Mapping"
type: issue
state: closed
author: JelleZijlstra
labels:
  - bug
  - ty
assignees: []
created_at: 2025-05-07T03:24:23Z
updated_at: 2025-05-07T15:17:58Z
url: https://github.com/astral-sh/ruff/issues/17904
synced_at: 2026-01-12T15:54:56Z
```

# ty: dict should be compatible with Mapping

---

_@JelleZijlstra_

### Summary

ty fails to recognize that a `dict` (i.e., `dict[Any, Any]`) is assignable to `Mapping[str, int]`.

```python
from collections.abc import Mapping

def capybara(x: Mapping[str, int]): ...

def hutia(x: dict):
    capybara(x)
```

```
    Argument to this function is incorrect: Expected `Mapping[str, int]`, found `dict[Unknown, Unknown]` (lint:invalid-argument-type) [Ln 6, Col 14]
```

https://play.ty.dev/b945b5e0-9b04-4e28-93c2-76ba33b24f9f

Not sure what exactly is going on here; I thought it might be something to do with variance but it does accept `list` for `MutableSequence[int]`.

### Version

_No response_

---

_Comment by @carljm on 2025-05-07 03:41_

It works with `dict[str, int]` but fails with `dict[Any, Any]`.

cc @dcreager 

---

_Label `bug` added by @carljm on 2025-05-07 03:43_

---

_Label `ty` added by @carljm on 2025-05-07 03:43_

---

_Assigned to @dcreager by @dcreager on 2025-05-07 14:13_

---

_Comment by @JelleZijlstra on 2025-05-07 14:49_

Noticed another one that looks similar:

```
error: lint:invalid-assignment: Object of type `defaultdict[Unknown, Unknown]` is not assignable to `dict[type[ADT], _TagCount]`
    --> taxonomy/shell.py:3049:5
     |
3047 |         return
3048 |
3049 |     counts: dict[type[ADT], _TagCount] = defaultdict(_TagCount)
     |     ^^^^^^
3050 |     for obj in model.select_valid():
3051 |         seen_for_nam = set()
```

Also seeing this with `Counter`.

---

_Comment by @charliermarsh on 2025-05-07 14:51_

Is this not the same as https://github.com/astral-sh/ty/issues/101?

---

_Comment by @JelleZijlstra on 2025-05-07 14:54_

Yes it is

---

_Closed by @JelleZijlstra on 2025-05-07 14:54_

---

_Closed by @dcreager on 2025-05-07 14:55_

---
