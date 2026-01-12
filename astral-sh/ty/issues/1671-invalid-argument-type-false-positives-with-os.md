```yaml
number: 1671
title: invalid-argument-type false positives with os.walk/fnmatch code
type: issue
state: closed
author: correctmost
labels:
  - bug
  - generics
assignees: []
created_at: 2025-11-29T04:04:17Z
updated_at: 2025-12-12T20:43:27Z
url: https://github.com/astral-sh/ty/issues/1671
synced_at: 2026-01-12T15:54:25Z
```

# invalid-argument-type false positives with os.walk/fnmatch code

---

_@correctmost_

### Summary

After https://github.com/astral-sh/ruff/pull/21553 landed, I started seeing `invalid-argument-type` errors with this code:

```python
import fnmatch
import os
from pathlib import Path


for directory, _, files in os.walk(Path('.')):
    for filename in fnmatch.filter(files, '*.json'):
        continue
```

```
walk.py:7:36: error[invalid-argument-type] Argument to function `filter` is incorrect: Argument type `Path` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
walk.py:7:36: error[invalid-argument-type] Argument to function `filter` is incorrect: Expected `Iterable[str]`, found `list[Path]`
```

Pyright, Pyrefly, and mypy do not emit similar warnings.

### Version

ty 0.0.1-alpha.29

---

_Label `bug` added by @sharkdp on 2025-11-29 06:58_

---

_Label `generics` added by @sharkdp on 2025-11-29 06:58_

---

_Assigned to @sharkdp by @sharkdp on 2025-11-29 07:06_

---

_Comment by @sharkdp on 2025-11-29 14:51_

Thank you for reporting this.

https://github.com/astral-sh/ruff/pull/21553 allowed us to understand generic type aliases, so we now understand [typeshed's `GenericPath`](https://github.com/python/typeshed/blob/148b8e631d9a76f7bdcc5478d36692ff79e16227/stdlib/_typeshed/__init__.pyi#L184) which is used in the signature of `os.walk`:
```py
AnyStr = TypeVar("AnyStr", str, bytes)
GenericPath: TypeAlias = AnyStr | PathLike[AnyStr]

def walk(
    top: GenericPath[AnyStr], topdown: bool = True, onerror: _OnError | None = None, followlinks: bool = False
) -> Iterator[tuple[AnyStr, list[AnyStr], list[AnyStr]]]:
	…
```

Since we can't handle generic protocols in the generic solver yet, we solve `AnyStr` as `Path` here (first union element), instead of `str` (second union element), and therefore return `list[Path]` in the third tuple element instead of `list[str]`.

This should be solved by #623.

---

_Unassigned @sharkdp by @sharkdp on 2025-12-01 11:32_

---

_Added to milestone `Beta` by @carljm on 2025-12-01 23:37_

---

_Assigned to @dcreager by @carljm on 2025-12-01 23:37_

---

_Assigned to @AlexWaygood by @carljm on 2025-12-01 23:37_

---

_Comment by @correctmost on 2025-12-12 20:11_

It looks like this issue was fixed by astral-sh/ruff@7bf50e70.

https://play.ty.dev/15dcb62d-3910-42f7-8121-543ebfd3fb89

---

_Comment by @AlexWaygood on 2025-12-12 20:43_

Aha! So it looks like this problem was caused by two bugs existing in ty simultaneously. Fixing either would have fixed this symptom, and one has now been fixed (by https://github.com/astral-sh/ruff/commit/7bf50e70)

---

_Closed by @AlexWaygood on 2025-12-12 20:43_

---

_Comment by @AlexWaygood on 2025-12-12 20:43_

Thanks @correctmost!

---
