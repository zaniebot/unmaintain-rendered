---
number: 2684
title: TCH001 Re export x annotation
type: issue
state: closed
author: g-as
labels:
  - bug
assignees: []
created_at: 2023-02-09T09:19:25Z
updated_at: 2023-02-09T16:22:17Z
url: https://github.com/astral-sh/ruff/issues/2684
synced_at: 2026-01-07T13:12:14-06:00
---

# TCH001 Re export x annotation

---

_Issue opened by @g-as on 2023-02-09 09:19_

When an import is used in an annotation but also is re exported in an `__all__` statement, **TCH001** rings.

```python
"""Re export x annotation."""
from .stuff import Stuff

__all__ = ("Stuff", "get_stuff")


def get_stuff(x: Stuff) -> None:
    """Get some stuff."""
    ...
```
```bash
__init__.py:2:20: TCH001 Move application import `.stuff.Stuff` into a type-checking block
```


---

_Label `bug` added by @charliermarsh on 2023-02-09 12:13_

---

_Referenced in [astral-sh/ruff#2689](../../astral-sh/ruff/pulls/2689.md) on 2023-02-09 16:12_

---

_Closed by @charliermarsh on 2023-02-09 16:22_

---
