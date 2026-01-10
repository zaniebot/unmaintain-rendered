---
number: 1978
title: Intersection of fixed-length tuple with another type should be unpackable like the fixed-length tuple 
type: issue
state: closed
author: SuperKenVery
labels:
  - bug
  - set-theoretic types
assignees: []
created_at: 2025-12-17T06:30:19Z
updated_at: 2025-12-22T22:49:49Z
url: https://github.com/astral-sh/ty/issues/1978
synced_at: 2026-01-10T01:52:52Z
---

# Intersection of fixed-length tuple with another type should be unpackable like the fixed-length tuple 

---

_Issue opened by @SuperKenVery on 2025-12-17 06:30_

### Summary

Here's an example:
https://play.ty.dev/948638ab-b0a1-45bf-a945-88d9b60a6dc9

Note the error message:
```
Return type does not match returned value: expected `int`, found `str | int`
```

This is not expected. There should be no error.

I initially came up with a more complex reproduction. Thanks to @rijuyuezhu for providing an even simpler reproduction.

### Version

ty 0.0.2

---

_Renamed from "Wrong type when unpacking a tuple after narrowing down a union" to "Intersection of fixed-length tuple with another type should be unpackable like the fixed-length tuple " by @AlexWaygood on 2025-12-17 07:33_

---

_Label `bug` added by @AlexWaygood on 2025-12-17 07:33_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-12-17 07:33_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-17 07:33_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-12-17 13:14_

---

_Comment by @carljm on 2025-12-22 22:49_

This example seems to work now; I suspect it was fixed by https://github.com/astral-sh/ruff/pull/21965

---

_Closed by @carljm on 2025-12-22 22:49_

---
