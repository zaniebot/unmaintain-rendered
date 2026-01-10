```yaml
number: 17931
title: False positive for UP037
type: issue
state: open
author: xu-cheng
labels:
  - bug
  - type-inference
assignees: []
created_at: 2025-05-07T23:19:23Z
updated_at: 2025-05-08T14:56:54Z
url: https://github.com/astral-sh/ruff/issues/17931
synced_at: 2026-01-10T11:09:58Z
```

# False positive for UP037

---

_Issue opened by @xu-cheng on 2025-05-07 23:19_

### Summary

MWE to reproduce the problem:

* `__init__.py`
```py
```

* `prelude.py`
```py
__all__ = [
    "Literal",
]

from typing import Literal
```

* `test.py`
```py
from __future__ import annotations
from .prelude import *

class Foo:
    def func(
        self,
        arg: Literal["bar"],
    ) -> None:
        print(arg)
```

Ruff output:
```
$ ruff check --select=UP .
test.py:7:22: UP037 [*] Remove quotes from type annotation
  |
5 |     def func(
6 |         self,
7 |         arg: Literal["bar"],
  |                      ^^^^^ UP037
8 |     ) -> None:
9 |         print(arg)
  |
  = help: Remove quotes

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

### Version

ruff 0.11.8

---

_Comment by @Avasam on 2025-05-08 04:15_

This would require multi-file analysis to detect that `Literal` here is an alias to `typing.Literal` from a star import: https://github.com/astral-sh/ruff/issues/7447

---

_Comment by @ntBre on 2025-05-08 14:56_

This sounds very closely related to https://github.com/astral-sh/ruff/issues/6014, just with a different rule.

---

_Label `bug` added by @ntBre on 2025-05-08 14:56_

---

_Label `type-inference` added by @ntBre on 2025-05-08 14:56_

---
