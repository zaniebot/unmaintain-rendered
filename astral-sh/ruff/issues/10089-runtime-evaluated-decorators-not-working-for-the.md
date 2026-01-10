---
number: 10089
title: "`runtime-evaluated-decorators` not working for the annotations of decorated functions."
type: issue
state: closed
author: DetachHead
labels: []
assignees: []
created_at: 2024-02-23T01:55:24Z
updated_at: 2024-02-23T03:33:10Z
url: https://github.com/astral-sh/ruff/issues/10089
synced_at: 2026-01-10T01:22:49Z
---

# `runtime-evaluated-decorators` not working for the annotations of decorated functions.

---

_Issue opened by @DetachHead on 2024-02-23 01:55_

```py
from __future__ import annotations

from pydantic import validate_call
from asdf import Foo # error: TCH002: Move third-party import `asdf.Foo` into a type-checking block

@validate_call
def a(value: Foo): ...
```
config:
```json
{
  "lint": {
    "select": [
      "TCH"
    ],
    "flake8-type-checking": {"runtime-evaluated-decorators": ["pydantic.validate_call"]}
  }
}
```
[playground](https://play.ruff.rs/d484b3c4-94b4-4efa-ad9e-267382c77c73)

---

_Referenced in [astral-sh/ruff#9312](../../astral-sh/ruff/issues/9312.md) on 2024-02-23 01:55_

---

_Comment by @charliermarsh on 2024-02-23 02:41_

I'll take a look now.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-23 02:51_

---

_Comment by @charliermarsh on 2024-02-23 03:05_

It looks like these are applied for assignments _within_ function bodies, but not annotations on function _signatures_ (parameters and return types).

---

_Referenced in [astral-sh/ruff#10091](../../astral-sh/ruff/pulls/10091.md) on 2024-02-23 03:24_

---

_Closed by @charliermarsh on 2024-02-23 03:33_

---
