```yaml
number: 14593
title: "[red-knot] Inverse narrowing without explicit `else` branches"
type: issue
state: closed
author: sharkdp
labels:
  - help wanted
  - ty
assignees: []
created_at: 2024-11-25T19:34:21Z
updated_at: 2024-11-28T14:23:56Z
url: https://github.com/astral-sh/ruff/issues/14593
synced_at: 2026-01-12T15:54:54Z
```

# [red-knot] Inverse narrowing without explicit `else` branches

---

_@sharkdp_

Consider the following code:

```py
def optional_int() -> int | None: ...
x = optional_int()

if x is None:
    x = 0
else:
    pass

reveal_type(x)  # revealed: int
```

We correctly infer `int` for `x` in the final line. This works because we union the types of both branches. The `if` branch has an explicit definition of `x` with type `Literal[0]`. And the empty `else` branch has a type of `(int | None) & ~None = int` for `x`, thanks to narrowing. The union of these two, `Literal[0] | int`, then simplifies to `int`.

However, if we remove the empty `else` branch, we get a type of `x: int | None` in the final line. This should be fixed. We should not union `Literal[0]` from the `if` branch with what we had before (`int | None`), but rather apply the inverted narrowing condition to the pre-`if` type of `x`, just as if we had an empty `else` branch.

```py
def optional_int() -> int | None: ...
x = optional_int()

if x is None:
    x = 0

reveal_type(x)  # revealed: int | None
```

---

_Label `red-knot` added by @sharkdp on 2024-11-25 19:34_

---

_Renamed from "[red-knot] Narrowing should " to "[red-knot] Inverse narrowing without explicit `else` branches" by @sharkdp on 2024-11-25 19:34_

---

_Label `help wanted` added by @carljm on 2024-11-26 00:30_

---

_Comment by @sransara on 2024-11-26 22:04_

I would like to take a shot at this tonight if no one has picked this up yet.

BTW I assume you meant to write something like following for the second example?

```
def optional_int() -> int | None: ...
x = optional_int()

if x is None:
    x = 0

reveal_type(x)  # revealed: int | None
```

---

_Comment by @sransara on 2024-11-27 01:52_

My thinking is that:

https://github.com/astral-sh/ruff/blob/82c01aa6623b7d4df1e4f06cb2889059f287336f/crates/red_knot_python_semantic/src/semantic_index/builder.rs#L791-L799

needs to be adjusted to match an `else: pass`.

---

_Closed by @carljm on 2024-11-28 14:23_

---
