```yaml
number: 681
title: "\"not_none\" fails to refine type"
type: issue
state: closed
author: adamh-oai
labels:
  - generics
assignees: []
created_at: 2025-06-18T17:42:43Z
updated_at: 2025-06-18T18:40:17Z
url: https://github.com/astral-sh/ty/issues/681
synced_at: 2026-01-10T02:08:20Z
```

# "not_none" fails to refine type

---

_Issue opened by @adamh-oai on 2025-06-18 17:42_

### Summary

Example below.  If make this `not_none(str | None) -> str` it works as expected.

```
$ cat not_none.py
import os
from typing import TypeVar

T = TypeVar("T")

def not_none(v: T | None) -> T:
    if v is None:
        raise RuntimeError("f")
    return v

def test_not_none() -> int:
    v = not_none(os.environ.get("X"))
    return len(v)

$ uvx ty@latest check
Installed 1 package in 14ms
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[invalid-argument-type]: Argument to function `len` is incorrect
  --> not_none.py:13:16
   |
11 | def test_not_none() -> int:
12 |     v = not_none(os.environ.get("X"))
13 |     return len(v)
   |                ^ Expected `Sized`, found `str | None`
   |
info: Function defined here
    --> stdlib/builtins.pyi:1535:5
     |
1533 | def isinstance(obj: object, class_or_tuple: _ClassInfo, /) -> bool: ...
1534 | def issubclass(cls: type, class_or_tuple: _ClassInfo, /) -> bool: ...
1535 | def len(obj: Sized, /) -> int: ...
     |     ^^^ ---------- Parameter declared here
1536 |
1537 | license: _sitebuiltins._Printer
     |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.1-alpha.11

---

_Comment by @AlexWaygood on 2025-06-18 17:46_

I think this is because we're solving the `TypeVar` `T` to `str | None` (which is a _valid_ solution, just not the narrowest type we could solve to).

---

_Label `generics` added by @AlexWaygood on 2025-06-18 17:46_

---

_Comment by @carljm on 2025-06-18 18:40_

Yes, our constraint solver is currently extremely basic; this should be fixed by improving it. Duplicate of #623 

---

_Closed by @carljm on 2025-06-18 18:40_

---
