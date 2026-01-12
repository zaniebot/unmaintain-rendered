```yaml
number: 232
title: cyclic control flow for loops
type: issue
state: open
author: carljm
labels:
  - control flow
  - runtime semantics
assignees: []
created_at: 2024-11-07T15:27:26Z
updated_at: 2025-12-23T15:15:12Z
url: https://github.com/astral-sh/ty/issues/232
synced_at: 2026-01-12T15:54:22Z
```

# cyclic control flow for loops

---

_@carljm_

Currently we don't even model control flow back edges in loops (because they will often lead to inference cycles). Once we have fixpoint iteration support in Salsa, we need to add these back edges and tests for them, including cases where we have to fallback to avoid runaway fixpoint iteration, e.g.:

```py
x = 0
for _ in range(y):
    x += 1
```

---

_Assigned to @carljm by @carljm on 2024-11-07 15:27_

---

_Renamed from "[red-knot] fixpoint iteration and cyclic control flow for loops" to "[red-knot] cyclic control flow for loops" by @carljm on 2025-03-12 12:50_

---

_Unassigned @carljm by @carljm on 2025-03-27 18:47_

---

_Renamed from "[red-knot] cyclic control flow for loops" to "cyclic control flow for loops" by @MichaReiser on 2025-05-07 15:27_

---

_Label `control flow` added by @AlexWaygood on 2025-05-10 21:38_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:48_

---

_Removed from milestone `Beta` by @carljm on 2025-06-11 00:52_

---

_Added to milestone `GA` by @carljm on 2025-06-11 00:52_

---

_Comment by @sharkdp on 2025-09-04 07:52_

I've seen multiple real world examples where missing support for loop control flow leads to an incorrect reachability analysis for large blocks of code within loops, because we observe a type that is too narrow. For example:

```py
first_iteration = True

for x in xs:
    if not first_iteration:
        # this whole block is considered unreachable

    # more code

    first_iteration = False
```

---

_Assigned to @oconnor663 by @oconnor663 on 2025-11-14 15:21_

---

_Comment by @Gankra on 2025-12-07 18:56_

I just encountered the exact same problem as David:

https://play.ty.dev/9214d74c-f4de-46df-80d0-44b3262d1ff4

```py
some_list: list[int] = [0, 1]

first_part = True
for j in range(0, 10):
    if first_part:
        first_part = False
        continue
    # ty infers `val: Never` here!?
    # (many other results now infer to Unknown)
    val = some_list[0]
```

And because we don't respect `first_part: bool` there's no (non-tedious) way to tell ty to chill.

---

_Comment by @sharkdp on 2025-12-07 19:00_

> And because we don't respect `first_part: bool` there's no (non-tedious) way to tell ty to chill.

Obviously not ideal, but a `cast` always helps as a workaround in these situations (`first_part = cast(bool, True)`).

---

_Removed from milestone `Stable` by @carljm on 2025-12-19 23:38_

---

_Added to milestone `M1` by @carljm on 2025-12-19 23:38_

---

_Comment by @anstadnik on 2025-12-23 15:15_

```python
    i = 0
    while i < 1: # Here i is Literal[0]
        i += 1 # Here i is Literal[1]
        
    reveal_type(i) # Never
```

`ty` thinks that it's an endless loop

---
