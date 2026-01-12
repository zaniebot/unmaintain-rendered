```yaml
number: 14172
title: "[red-knot] understand more special forms in annotation expressions"
type: issue
state: closed
author: carljm
labels:
  - help wanted
  - ty
assignees: []
created_at: 2024-11-07T16:50:08Z
updated_at: 2024-12-13T17:54:22Z
url: https://github.com/astral-sh/ruff/issues/14172
synced_at: 2026-01-12T15:54:53Z
```

# [red-knot] understand more special forms in annotation expressions

---

_@carljm_

Feel free to create and link a sub-task to claim one of these to work on:

- [x] Unions via `|`
- [x] #14498
- [x] #14354
- [x] #14544
- [x] https://github.com/astral-sh/ruff/issues/14558
- [x] #14545
- [x] #14703
- [x] #14834
- [x] #14546
- [x] #14922
- [x] https://github.com/astral-sh/ruff/issues/14891
- [x] https://github.com/astral-sh/ruff/issues/14916



---

_Label `help wanted` added by @carljm on 2024-11-07 16:50_

---

_Label `red-knot` added by @carljm on 2024-11-07 16:50_

---

_Added to milestone `Red Knot 2024` by @carljm on 2024-11-07 16:50_

---

_Comment by @Glyphack on 2024-11-17 11:32_

To understand better: Do we want to follow the same approach for `typing.Any` and infer it as a known instance?
[Any](https://github.com/python/typeshed/blob/ca65e087f1f9dfc28e89192b60ce7cfc2e92c674/stdlib/typing.pyi#L134) is assigned an object. If we leave it as is then we just need to infer the `KnownClass::Object` in the annotations.

---

_Comment by @AlexWaygood on 2024-11-17 11:46_

> To understand better: Do we want to follow the same approach for `typing.Any` and infer it as a known instance?

I think so. It's important that we distinguish internally between "members of the type `Any`" and "the singleton symbol that actually represents the `Any` type itself". Inferring the symbol `typing.Any` as a known instance allows us to do that.

---

_Closed by @carljm on 2024-12-13 17:54_

---
