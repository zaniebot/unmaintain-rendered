```yaml
number: 813
title: "Type of restriction of float | Sequence[float] not working"
type: issue
state: closed
author: eendebakpt
labels: []
assignees: []
created_at: 2025-07-11T07:43:30Z
updated_at: 2025-07-14T06:40:16Z
url: https://github.com/astral-sh/ty/issues/813
synced_at: 2026-01-10T02:07:36Z
```

# Type of restriction of float | Sequence[float] not working

---

_Issue opened by @eendebakpt on 2025-07-11 07:43_

### Summary

Consider the following minimal example:
```
def ty_test(x: list[float] | float) -> None:
    if isinstance(x, float):
        x = [x] 
    
    if len(x) == 1:
        x[0]
    elif len(x) == 2:
        x[1]
        
    return
```
After the `isinstance` check I would expect the type to be restricted to `list[float]`. Instead the type reported by `ty` is `list[int | float] | int | list[Unknown]`, and multiple errors are generated on the call to `len` and the subscripting on `x`.

### Version

ty 0.0.1-alpha.14 (3ececb07e 2025-07-08)

---

_Comment by @jelle-openai on 2025-07-11 16:10_

This seems mostly correct, except I'd expect the last `list[Unknown]` to be `list[float]`. `float` includes `int` in the type system, and the way ty implements that (which I agree with) is to interpret the type annotation `float` as if it means `float | int`. But `isinstance(x, float)` only checks for actual `float`, not `int`, so after the `if` condition `x` could still be an int.

Implementing it this way is important for soundness; ty will accept a call like `ty_test(1)`, which would fail at runtime in your implementation.

---

_Comment by @eendebakpt on 2025-07-12 22:22_

Yes, with 
```
def ty_test(x: list[float] | float) -> float:
    if isinstance(x, (float, int)):
        x = [x] 
    
    return x[0]
```
all checks are fine. It is a bit strange the `list[Unknown]` is reported (also in the playground), but the return type is accepted.
 
Feel free to close the issue (or leave open for the `Unknown`).

---

_Comment by @sharkdp on 2025-07-14 06:40_

> all checks are fine. It is a bit strange the `list[Unknown]` is reported (also in the playground), but the return type is accepted.

We currently still infer `list[Unknown]` for list literals, so the `[x]` in the `x = [x]` assignment has type `list[Unknown]` at the moment. This is already tracked in https://github.com/astral-sh/ty/issues/543, so I think we can close this.

---

_Closed by @sharkdp on 2025-07-14 06:40_

---
