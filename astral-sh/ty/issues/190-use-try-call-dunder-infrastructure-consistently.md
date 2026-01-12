```yaml
number: 190
title: "Use `try_call_dunder` infrastructure consistently"
type: issue
state: open
author: sharkdp
labels:
  - bug
assignees: []
created_at: 2025-02-25T13:32:43Z
updated_at: 2025-11-13T16:09:31Z
url: https://github.com/astral-sh/ty/issues/190
synced_at: 2026-01-12T15:54:22Z
```

# Use `try_call_dunder` infrastructure consistently

---

_@sharkdp_

### Summary

`__enter__` and `__exit__` should be treated like all other dunder methods. However, while working on astral-sh/ruff#16368, I noticed that we look them up manually (and potentially incorrectly) here …

https://github.com/astral-sh/ruff/blob/1be0dc6885317340acdd3427aa43acc05e7a43d3/crates/red_knot_python_semantic/src/types/infer.rs#L1621-L1622

… and call them manually (and potentially incorrectly) here:

https://github.com/astral-sh/ruff/blob/1be0dc6885317340acdd3427aa43acc05e7a43d3/crates/red_knot_python_semantic/src/types/infer.rs#L1660-L1661

This only leads to incorrect behavior in very exotic situations like the following, where we incorrectly raise a *"Object of type `Manager` cannot be used with `with` because it does not correctly implement `__enter__`"* diagnostic:

```py
from __future__ import annotations

class Target: ...

class SomeCallable:
    def __call__(self) -> Target:
        return Target()

class Descriptor:
    def __get__(self, instance, owner) -> SomeCallable:
        return SomeCallable()

class Manager:
    __enter__: Descriptor = Descriptor()

    def __exit__(self, exc_type, exc_value, traceback): ...

with Manager() as f:
    reveal_type(f)  # revealed: Target
```

Fixing this would involve using `try_call_dunder` after astral-sh/ruff#16368 is merged, or using `static_member` + a manual descriptor call, if that is not possible.

---

_Label `bug` added by @sharkdp on 2025-02-25 13:32_

---

_Comment by @sharkdp on 2025-02-25 13:35_

The same is probably true for `__class_getitem__`:

https://github.com/astral-sh/ruff/blob/1be0dc6885317340acdd3427aa43acc05e7a43d3/crates/red_knot_python_semantic/src/types/infer.rs#L5220-L5221

https://github.com/astral-sh/ruff/blob/1be0dc6885317340acdd3427aa43acc05e7a43d3/crates/red_knot_python_semantic/src/types/infer.rs#L5238

---

_Comment by @sharkdp on 2025-02-25 13:37_

And augmented assignments:

https://github.com/astral-sh/ruff/blob/1be0dc6885317340acdd3427aa43acc05e7a43d3/crates/red_knot_python_semantic/src/types/infer.rs#L2259-L2267

---

_Comment by @sharkdp on 2025-02-25 13:39_

And `in`/`not in` tests:

https://github.com/astral-sh/ruff/blob/1be0dc6885317340acdd3427aa43acc05e7a43d3/crates/red_knot_python_semantic/src/types/infer.rs#L4902-L4910

---

_Comment by @sharkdp on 2025-02-25 13:40_

… and subscript expressions (this is the last one, I promise):

https://github.com/astral-sh/ruff/blob/1be0dc6885317340acdd3427aa43acc05e7a43d3/crates/red_knot_python_semantic/src/types/infer.rs#L5220-L5238

---

_Renamed from "[red-knot] Context managers should use `try_call_dunder` infrastructure" to "[red-knot] Use `try_call_dunder` infrastructure consistently" by @sharkdp on 2025-02-25 13:41_

---

_Renamed from "[red-knot] Use `try_call_dunder` infrastructure consistently" to "Use `try_call_dunder` infrastructure consistently" by @MichaReiser on 2025-05-07 15:26_

---

_Added to milestone `Stable` by @carljm on 2025-11-13 16:09_

---
