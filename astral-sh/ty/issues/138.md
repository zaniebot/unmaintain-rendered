```yaml
number: 138
title: Typevar should specialize to a single type across different statements
type: issue
state: open
author: dcreager
labels:
  - generics
assignees: []
created_at: 2025-04-03T20:09:46Z
updated_at: 2025-11-18T16:10:20Z
url: https://github.com/astral-sh/ty/issues/138
synced_at: 2026-01-10T02:06:24Z
```

# Typevar should specialize to a single type across different statements

---

_Issue opened by @dcreager on 2025-04-03 20:09_

A typevar should specialize to a single type each time its generic class or function is specialized. We're already considering that, and have a (currently failing) mdtest for this example:

```py
def f[T: (int, str)](t: T, u: T) -> T:
  return t + u
```

This is valid since `int` has an `__add__` method that takes in another `int`, and `str` has one that takes in another `str`.  It's not an error that `int`'s method doesn't take in a `str`, since if `t` is an `int`, `u` must be one too. This is an important way that constrained typevars are different from a union.

@carljm and I were trying to brainstorm whether this kind of propagation could occur across multiple statements, and he came up with this example:

```py
def f[T](a: T, b: T):
    if isinstance(a, int):
        reveal_type(a)  # revealed: int
        reveal_type(b)  # hopefully: int
```

The narrowing constraint machinery seems like it provides what we need to make this work — the `isinstance` call would (waving hands wildly) not just add a narrowing constraint on `a`, but also on any typevars that appear in the type of `a`.

---

_Renamed from "[red-knot] Typevar should specialize to a single type across different statements" to "Typevar should specialize to a single type across different statements" by @MichaReiser on 2025-05-07 15:25_

---

_Label `generics` added by @AlexWaygood on 2025-05-11 07:32_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 16:12_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
