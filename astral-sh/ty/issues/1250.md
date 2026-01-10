---
number: 1250
title: "widen `Literal` types in invariant/covariant positions of generic function calls"
type: issue
state: closed
author: KotlinIsland
labels:
  - generics
assignees: []
created_at: 2025-09-25T03:27:07Z
updated_at: 2026-01-08T18:39:05Z
url: https://github.com/astral-sh/ty/issues/1250
synced_at: 2026-01-10T01:51:14Z
---

# widen `Literal` types in invariant/covariant positions of generic function calls

---

_Issue opened by @KotlinIsland on 2025-09-25 03:27_

### Summary

```py
def f[A](a: A) -> list[A]:
    return [a]

result = f(1) # list[Literal[1]]
```
i would expect `list[int]` to match the semantics of `[1]`

`Literal` should only be preserved for covariant and "bare" positions

### Version

_No response_

---

_Label `generics` added by @AlexWaygood on 2025-09-25 06:46_

---

_Added to milestone `GA` by @carljm on 2025-09-25 14:29_

---

_Comment by @carljm on 2026-01-08 18:39_

This is now done.

---

_Closed by @carljm on 2026-01-08 18:39_

---
