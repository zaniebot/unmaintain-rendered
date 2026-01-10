---
number: 6928
title: "TCH004: local imports shadowing TYPE_CHECKING imports lead to false positives"
type: issue
state: closed
author: twoertwein
labels:
  - bug
assignees: []
created_at: 2023-08-28T00:43:05Z
updated_at: 2023-08-28T13:04:45Z
url: https://github.com/astral-sh/ruff/issues/6928
synced_at: 2026-01-10T01:22:46Z
---

# TCH004: local imports shadowing TYPE_CHECKING imports lead to false positives

---

_Issue opened by @twoertwein on 2023-08-28 00:43_

A simplified version of what happened in https://github.com/pandas-dev/pandas/pull/54786

```py
from __future__ import annotations

from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from bar import Foo # will be moved out by ruff

def test() -> Foo:  # the Foo from TYPE_CHECKING
    # avoid cyclic imports (yes, this is bad design)
    from bar import Foo

    return Foo() # this is the local Foo, the TYPE_CHECKING Foo is not "used"
```

edit: The issue was something else in https://github.com/pandas-dev/pandas/pull/54786 it was actually used at runtime inside a cast statement.

---

_Label `bug` added by @zanieb on 2023-08-28 04:57_

---

_Comment by @zanieb on 2023-08-28 04:57_

Thanks for the report! I think this is a fair pattern to special case.

Looks very similar to https://github.com/astral-sh/ruff/issues/4941

---

_Comment by @twoertwein on 2023-08-28 13:04_

Ruff actually already supports this (and ruff was moving it out because I actually used it in a cast statement).

Sorry for the noise!

---

_Closed by @twoertwein on 2023-08-28 13:04_

---
