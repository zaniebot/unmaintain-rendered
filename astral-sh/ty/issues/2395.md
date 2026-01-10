```yaml
number: 2395
title: "Incorrect `index-out-of-bounds` error"
type: issue
state: closed
author: alechouse97
labels: []
assignees: []
created_at: 2026-01-08T13:35:32Z
updated_at: 2026-01-08T16:09:57Z
url: https://github.com/astral-sh/ty/issues/2395
synced_at: 2026-01-10T01:56:41Z
```

# Incorrect `index-out-of-bounds` error

---

_Issue opened by @alechouse97 on 2026-01-08 13:35_

### Summary

Ty is currently raising an `index-out-of-bounds` error when facing a block of code, for example in this `tmp.py`:

```python
def func(x: tuple[float, float] | tuple[float, float, float]) -> None:
    if len(x) == 2:
        y = x[2]
    elif len(x) == 3:
        y = x[3]
```

It correctly catches that `x[2]` is out of bounds in the first branch, but also states `x[2]` is out of bounds in the second branch:

```
error[index-out-of-bounds]: Index 2 is out of bounds for tuple `tuple[int | float, int | float]` with length 2
 --> tmp.py:3:13
  |
1 | def func(x: tuple[float, float] | tuple[float, float, float]):
2 |     if len(x) == 2:
3 |         y = x[2]
  |             ^
4 |     elif len(x) == 3:
5 |         y = x[2]
  |
info: rule `index-out-of-bounds` is enabled by default

error[index-out-of-bounds]: Index 2 is out of bounds for tuple `tuple[int | float, int | float]` with length 2
 --> tmp.py:5:13
  |
3 |         y = x[2]
4 |     elif len(x) == 3:
5 |         y = x[2]
  |             ^
  |
info: rule `index-out-of-bounds` is enabled by default
```

Expected behavior is that ty narrows the type to `tuple[float, float, float]` based on the current branch that required

This can reproduced when running `ty check tmp.py` on the above.

As an aside, it also looks to be changing the type to `tuple[int | float, int | float]`, but I've not been using ty long enough to know if that is expected.

### Version

ty 0.0.10

---

_Renamed from "Incorrect tuple index out of range error" to "Incorrect `index-out-of-bounds` error" by @alechouse97 on 2026-01-08 13:35_

---

_Comment by @AlexWaygood on 2026-01-08 13:37_

thanks!

---

_Closed by @AlexWaygood on 2026-01-08 13:37_

---

_Comment by @carljm on 2026-01-08 16:09_

> As an aside, it also looks to be changing the type to `tuple[int | float, int | float]`, but I've not been using ty long enough to know if that is expected.

See https://docs.astral.sh/ty/reference/typing-faq/#why-does-ty-show-int-float-when-i-annotate-something-as-float

---
