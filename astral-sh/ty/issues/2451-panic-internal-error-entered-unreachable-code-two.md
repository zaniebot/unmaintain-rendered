```yaml
number: 2451
title: "[panic] internal error: entered unreachable code: Two equal, normalized intersections should share the same Salsa ID"
type: issue
state: closed
author: yilei
labels:
  - bug
  - fatal
assignees: []
created_at: 2026-01-11T21:00:10Z
updated_at: 2026-01-12T13:37:14Z
url: https://github.com/astral-sh/ty/issues/2451
synced_at: 2026-01-12T15:54:26Z
```

# [panic] internal error: entered unreachable code: Two equal, normalized intersections should share the same Salsa ID

---

_@yilei_

### Summary

Given following Python code:

```python
from typing import Any, TypedDict, cast


class A(TypedDict):
    x: str


class B(TypedDict):
    y: str


T = int | A | B


def test(a: Any, items: list[T]) -> None:
    combined = a or items
    v = combined[0]
    if isinstance(v, dict):
        cast(T, v)
```

`ty check` panicked:

```
❯ ty check /tmp/t.py
error[panic]: Panicked at crates/ty_python_semantic/src/types/type_ordering.rs:270:13 when checking `/tmp/t.py`: `internal error: entered unreachable code: Two equal, normalized intersections should share the same Salsa ID`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.11+1 (22e97ecdd 2026-01-09)
info: Args: ["ty", "check", "/tmp/t.py"]
info: Backtrace:
   0: <unknown>
   1: <unknown>
   2: <unknown>
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: <unknown>
   7: <unknown>
   8: <unknown>
   9: <unknown>
  10: <unknown>
  11: <unknown>
  12: <unknown>
  13: <unknown>
  14: <unknown>
  15: <unknown>
  16: <unknown>
  17: <unknown>
  18: <unknown>
  19: <unknown>
  20: <unknown>
  21: <unknown>
  22: <unknown>
  23: <unknown>
  24: <unknown>
  25: <unknown>
  26: <unknown>
  27: <unknown>
  28: <unknown>
  29: <unknown>
  30: <unknown>
  31: <unknown>
  32: <unknown>
  33: <unknown>
  34: <unknown>
  35: <unknown>
  36: <unknown>
  37: <unknown>
  38: <unknown>

info: query stacktrace:
   0: infer_expression_types_impl(Id(1805))
             at crates/ty_python_semantic/src/types/infer.rs:196
   1: infer_scope_types(Id(1003))
             at crates/ty_python_semantic/src/types/infer.rs:69
   2: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:554
```

### Version

ty 0.0.11+1 (22e97ecdd 2026-01-09)

---

_Label `bug` added by @AlexWaygood on 2026-01-11 21:05_

---

_Label `fatal` added by @AlexWaygood on 2026-01-11 21:05_

---

_Added to milestone `Pre-stable 1` by @AlexWaygood on 2026-01-11 21:05_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-12 00:38_

---

_Closed by @charliermarsh on 2026-01-12 13:35_

---

_Comment by @charliermarsh on 2026-01-12 13:37_

Thank you! Fixed in the next release.

---
