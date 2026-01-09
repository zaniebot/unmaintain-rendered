---
number: 11245
title: "False positive for `F821` on circular reference in `TYPE_CHECKING` block"
type: issue
state: open
author: ngnpope
labels:
  - bug
assignees: []
created_at: 2024-05-02T10:43:58Z
updated_at: 2024-05-03T10:11:22Z
url: https://github.com/astral-sh/ruff/issues/11245
synced_at: 2026-01-07T13:12:15-06:00
---

# False positive for `F821` on circular reference in `TYPE_CHECKING` block

---

_Issue opened by @ngnpope on 2024-05-02 10:43_

Keywords searched for before creating issue: `F821`, `TYPE_CHECKING`.

Example code that produces the false positive:

```python
from __future__ import annotations

from typing import TYPE_CHECKING, TypeAlias

if TYPE_CHECKING:
    Nested: TypeAlias = dict[str, bool | Nested]
```

Command line and output:

```console
$ ruff --version
ruff 0.4.2
$ ruff check --isolated --select=F821 bug.py 
bug.py:6:42: F821 Undefined name `Nested`
Found 1 error.
$ mypy --strict bug.py 
Success: no issues found in 1 source file
```

Expected that `F821` would not be raised in a `TYPE_CHECKING` block for a circular reference when using postponed evaluation of annotations (PEP 563).

---

_Comment by @trag1c on 2024-05-02 11:25_

nit: indeed, PEP 563 applies to _annotations_, but `dict[str, bool | Nested]` isn't one (in fact the `__future__` import doesn't make a difference).

I think the issue is still valid though :+1:

---

_Label `bug` added by @charliermarsh on 2024-05-02 14:29_

---

_Comment by @charliermarsh on 2024-05-02 14:29_

Not sure... @AlexWaygood would know.

---

_Comment by @AlexWaygood on 2024-05-02 14:40_

Yeah I think `from __future__ import annotations` is irrelevant here, _but_ if it's in an `if TYPE_CHECKING` block we should treat it as though it has "stub file" semantics (whether or not `__future__` annotations are enabled). The rhs of an assignment is evaluated at runtime regardless of whether `__future__` annotations are enabled, but nothing in an `if TYPE_CHECKING` block is ever evaluated at runtime, so it doesn't matter

TL;DR: I agree that this is a false positive

---

_Comment by @AlexWaygood on 2024-05-02 14:42_

If you tried to _use_ `Nested` in a type annotation anywhere without either quoting it or having `__future__` annotations enabled, it would fail, because the alias doesn't exist at runtime. But that's a separate issue

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-05-03 10:11_

---

_Referenced in [astral-sh/ruff#11262](../../astral-sh/ruff/pulls/11262.md) on 2024-05-03 12:45_

---

_Referenced in [astropy/astropy#15920](../../astropy/astropy/pulls/15920.md) on 2024-07-01 21:00_

---
