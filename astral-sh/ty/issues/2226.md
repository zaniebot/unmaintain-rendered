```yaml
number: 2226
title: "Widen JustFloat to `float | int` as part of widening literal types"
type: issue
state: closed
author: Wizzerinus
labels:
  - help wanted
assignees: []
created_at: 2025-12-26T09:39:05Z
updated_at: 2025-12-27T00:19:24Z
url: https://github.com/astral-sh/ty/issues/2226
synced_at: 2026-01-10T01:56:41Z
```

# Widen JustFloat to `float | int` as part of widening literal types

---

_Issue opened by @Wizzerinus on 2025-12-26 09:39_

### Summary

In the following example, the type for `FloatDict` is inferred as `dict[int, float]` rather than `dict[int, float | int]`, which is a bit unexpected to me, because that causes the assignment to fail typechecking.

```py
FloatDict = dict.fromkeys(range(10), 0.5)

def make_float() -> float:
    raise NotImplementedError()

FloatDict[0] = make_float()
```

Diagnostic:
```
error[invalid-assignment]: Invalid subscript assignment with key of type `Literal[0]` and value of type `int | float` on object of type `dict[int, float]`
 --> floatdict.py:6:1
  |
4 |     raise NotImplementedError()
5 |
6 | FloatDict[0] = make_float()
  | ^^^^^^^^^^^^^^^------------
  |                |
  |                Expected value of type `float`, got `int | float`
  |
info: rule `invalid-assignment` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.7

---

_Comment by @carljm on 2025-12-26 17:17_

Thanks! I think we need to always "widen" float inferences to `float | int` in the same scenarios where we widen literal types. So just like we already infer `dict.fromkeys(range(10), 1)` as `dict[int, int]` (not `dict[int, Literal[1]]`), we should likewise infer this case as `dict[int, float | int]` not `dict[int, float]`.

---

_Added to milestone `Stable` by @carljm on 2025-12-26 17:17_

---

_Renamed from "JustFloat being automatically inferred for `dict.fromkeys`" to "Widen JustFloat to `float | int` as part of widening literal types" by @carljm on 2025-12-26 17:17_

---

_Label `help wanted` added by @carljm on 2025-12-26 17:18_

---

_Comment by @carljm on 2025-12-26 17:18_

I think this should be a fairly easy change -- we already have the literal-widening logic, this _should_ be just adding one more case to it.

---

_Closed by @carljm on 2025-12-27 00:19_

---
