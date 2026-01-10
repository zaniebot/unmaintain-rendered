```yaml
number: 1223
title: TypeGuard and isinstance narrowing inconcistencies
type: issue
state: closed
author: tpgillam
labels: []
assignees: []
created_at: 2025-09-20T16:31:04Z
updated_at: 2025-09-20T17:06:54Z
url: https://github.com/astral-sh/ty/issues/1223
synced_at: 2026-01-10T02:06:25Z
```

# TypeGuard and isinstance narrowing inconcistencies

---

_Issue opened by @tpgillam on 2025-09-20 16:31_

### Summary

Run with ty 0.0.1-alpha21.

Consider the following. The comments indicate the diagnostics from running `uvx ty check moo.py`:

```python
import typing


# Case 1
def f1(x: int | str) -> typing.TypeGuard[int | bool]:
    return isinstance(x, (int, bool))

x = typing.cast("int | str", 42)
assert f1(x)
typing.reveal_type(x)  # Revealed type: `int | str`


# Case 2
def f2(x: int | str) -> typing.TypeIs[int | bool]:
    return isinstance(x, (int, bool))

x = typing.cast("int | str", 42)
assert f2(x)
typing.reveal_type(x)  # Revealed type: `int`

# Case 3
x = typing.cast("int | str", 42)
assert isinstance(x, int | bool)
typing.reveal_type(x)  # Revealed type: `int | str`

# Case 4
x = typing.cast("int | str", 42)
assert isinstance(x, (int, bool))
typing.reveal_type(x)  # Revealed type: `int`
```

- Case 1: from the documentation of TypeGuard I would have expected `int | bool` here
- Case 2: this seems correct
- Case 3: I would have _expected_ `int` here, since I think this should be equivalent to case 4
- Case 4: this seems correct

(I've done a brief search of existing issues since I feel sure this must already be reported, but failed to find something relevant. Apologies if I missed them!)

### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Renamed from "TypeGuard narrowing inconcistencies" to "TypeGuard and isinstance narrowing inconcistencies" by @tpgillam on 2025-09-20 16:31_

---

_Comment by @AlexWaygood on 2025-09-20 17:06_

Thanks for the report!

Case 1 here is #117 — we don't yet support `TypeGuard` at all yet unfortunately, only `TypeIs`.

Case 3 is #122 — again, we just don't support this at all yet I'm afraid

---

_Closed by @AlexWaygood on 2025-09-20 17:06_

---
