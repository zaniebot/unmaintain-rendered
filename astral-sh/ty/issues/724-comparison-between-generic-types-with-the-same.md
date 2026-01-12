```yaml
number: 724
title: "Comparison between generic types with the same bound fails with `unsupported-operator`"
type: issue
state: closed
author: cam-matsui
labels:
  - bug
  - generics
assignees: []
created_at: 2025-06-28T21:15:48Z
updated_at: 2026-01-08T18:27:35Z
url: https://github.com/astral-sh/ty/issues/724
synced_at: 2026-01-12T15:54:23Z
```

# Comparison between generic types with the same bound fails with `unsupported-operator`

---

_@cam-matsui_

### Summary

E.g.

```py
def compare[T: (int, float)](v1: T, v2: T) -> bool:
    return v1 > v2  # E: Operator `>` is not supported for types `T` and `T`
```

In this case, `v1` and `v2` should be recognized as having the same type which is either `int` or `flaot`, both of which are comparable.

Playground link: https://play.ty.dev/d028ac68-bbe6-4e9e-a7ff-0ae1bf77ff44

### Version

_No response_

---

_Label `generics` added by @carljm on 2025-06-30 19:53_

---

_Label `bug` added by @AlexWaygood on 2025-10-06 16:17_

---

_Added to milestone `Stable` by @carljm on 2025-11-14 15:27_

---

_Closed by @carljm on 2026-01-08 18:27_

---
