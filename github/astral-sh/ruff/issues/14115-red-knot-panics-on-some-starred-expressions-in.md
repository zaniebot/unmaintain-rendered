---
number: 14115
title: "[red-knot] Panics on (some) starred expressions in annotations"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - ty
assignees: []
created_at: 2024-11-05T18:58:51Z
updated_at: 2024-11-05T19:25:47Z
url: https://github.com/astral-sh/ruff/issues/14115
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Panics on (some) starred expressions in annotations

---

_Issue opened by @sharkdp on 2024-11-05 18:58_

Red knot currently panics on some instances of starred expressions (at the outermost layer) of type annotations. An actual example where something like this would occur is in variadic generics (the `*Ts`):
```py
Ts = TypeVarTuple("Ts")

def append_int(*args: *Ts) -> tuple[*Ts, int]: …
```
In this particular case, we currently *do not* panic since we never run deferred inference on that parameter type. But *we would* panic if we were to implement function parameter inference.

A case which *does* lead to a panic is the following (invalid syntax) example:
```py
def f() -> *int: ...
f()
```
(it's important that this occurs as a return type and that the function is actually called; it doesn't work otherwise)

A fix for this is already ready at #14106 

---

_Label `bug` added by @sharkdp on 2024-11-05 18:58_

---

_Label `red-knot` added by @sharkdp on 2024-11-05 18:58_

---

_Assigned to @sharkdp by @sharkdp on 2024-11-05 18:58_

---

_Referenced in [astral-sh/ruff#14106](../../astral-sh/ruff/pulls/14106.md) on 2024-11-05 19:00_

---

_Closed by @sharkdp on 2024-11-05 19:25_

---
