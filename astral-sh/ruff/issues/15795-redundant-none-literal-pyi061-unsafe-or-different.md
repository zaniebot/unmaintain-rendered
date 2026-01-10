---
number: 15795
title: "`redundant-none-literal (PYI061)`: Unsafe or different autofix for Python 3.9"
type: issue
state: closed
author: Avasam
labels:
  - fixes
  - preview
assignees: []
created_at: 2025-01-28T21:26:01Z
updated_at: 2025-02-09T15:58:54Z
url: https://github.com/astral-sh/ruff/issues/15795
synced_at: 2026-01-10T01:22:56Z
---

# `redundant-none-literal (PYI061)`: Unsafe or different autofix for Python 3.9

---

_Issue opened by @Avasam on 2025-01-28 21:26_

### Description

In https://github.com/astral-sh/ruff/pull/14872, more autofixes were added for `redundant-none-literal (PYI061)`, but it only applies to Python 3.10+ as the `|` may not be usable in annotations in <=3.9 in all cases, even with `from __future__ import annotations`.

I am requesting to either allow the fix on <=3.9 as an "unsafe" autofix.
OR to autofix using `typing.Union`. Leaving the `Union` --> `|` transformation logic to other (`UP`?) rules.

I also think this fix should always be available using `|` in type stubs (`.pyi` files)

## Current vs expected

The following code:
```py
def get_rotation(rotation: float | Literal[None, "horizontal", "vertical"]) -> float: ...
```
With the following command:
`ruff check --fix --unsafe-fixes --preview --select=PYI061 --isolated --target-version=py39`

### In Ruff 0.9.3:
```
stubs\matplotlib\text.pyi:16:44: PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
   |
14 | from .transforms import Bbox, Transform
15 |
16 | def get_rotation(rotation: float | Literal[None, "horizontal", "vertical"]) -> float: ...
   |                                            ^^^^ PYI061
17 |
18 | class Text(Artist):
   |
   = help: Replace with `Literal[...] | None`

Found 1 error.
```

### I expect:
```
Found 1 error (1 fixed, 0 remaining).
```

Unsafely autofixed to:
```
def get_rotation(rotation: float | Literal["horizontal", "vertical"] | None) -> float: ...
```
Or safely autofixed to:
```
def get_rotation(rotation: float | Union[Literal["horizontal", "vertical"], None]) -> float: ...
```



---

_Referenced in [astral-sh/ruff#14537](../../astral-sh/ruff/issues/14537.md) on 2025-01-28 21:26_

---

_Label `bug` added by @AlexWaygood on 2025-01-28 23:06_

---

_Label `fixes` added by @AlexWaygood on 2025-01-28 23:06_

---

_Referenced in [microsoft/python-type-stubs#340](../../microsoft/python-type-stubs/pulls/340.md) on 2025-01-29 00:15_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-02-03 16:50_

---

_Referenced in [astral-sh/ruff#16044](../../astral-sh/ruff/pulls/16044.md) on 2025-02-08 21:20_

---

_Label `bug` removed by @dylwil3 on 2025-02-08 21:21_

---

_Label `preview` added by @dylwil3 on 2025-02-08 21:21_

---

_Closed by @dylwil3 on 2025-02-09 15:58_

---
