```yaml
number: 2227
title: "Incorrect type inference for `bisect`"
type: issue
state: open
author: condemil
labels:
  - generics
  - bidirectional inference
assignees: []
created_at: 2025-12-26T14:42:17Z
updated_at: 2025-12-26T17:11:30Z
url: https://github.com/astral-sh/ty/issues/2227
synced_at: 2026-01-10T01:56:41Z
```

# Incorrect type inference for `bisect`

---

_Issue opened by @condemil on 2025-12-26 14:42_

### Summary

I see that ty cannot properly match types for `bisect` functions:

```python
import bisect

def foo(number: int, ranges: list[tuple[int, float]]) -> None:
    # Argument to function `bisect_right` is incorrect: Expected `SupportsLenAndGetItem[tuple[int, float]]`, found `list[tuple[int, int | float]]`
    bisect.bisect_right(ranges, (number, float("inf")))
```

The similar issue happens for `ranges: list[tuple[int, int]]` type.

### Version

_No response_

---

_Comment by @carljm on 2025-12-26 17:10_

Thanks! This looks like a case where we need to better solve a type variable that appears twice in a signature, and one of the occurrences could be widened with type context. The OP example is `float | int` vs `float` (which only occurs because we interpret a `float` annotation as `float | int` but a float literal as simply `float`), but the same thing occurs more simply with `int` vs `Literal[1]`:

```py
import bisect

def foo(number: int, ranges: list[tuple[int, int]]) -> None:
    # Argument to function `bisect_right` is incorrect: Expected `SupportsLenAndGetItem[tuple[int, Literal[1]]]`, found `list[tuple[int, int]]`
    bisect.bisect_right(ranges, (number, 1))
```

Maybe similar to #1970 ?

---

_Added to milestone `Stable` by @carljm on 2025-12-26 17:11_

---

_Label `generics` added by @carljm on 2025-12-26 17:11_

---

_Label `bidirectional inference` added by @carljm on 2025-12-26 17:11_

---
