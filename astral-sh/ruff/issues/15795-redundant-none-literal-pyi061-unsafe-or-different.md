```yaml
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
synced_at: 2026-01-12T15:54:54Z
```

# `redundant-none-literal (PYI061)`: Unsafe or different autofix for Python 3.9

---

_@Avasam_

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

_Label `bug` added by @AlexWaygood on 2025-01-28 23:06_

---

_Label `fixes` added by @AlexWaygood on 2025-01-28 23:06_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-02-03 16:50_

---

_Label `bug` removed by @dylwil3 on 2025-02-08 21:21_

---

_Label `preview` added by @dylwil3 on 2025-02-08 21:21_

---

_Closed by @dylwil3 on 2025-02-09 15:58_

---
