```yaml
number: 1303
title: "False-positive `F821: Undefined name` with `mypy_extensions.DefaultNamedArg`"
type: issue
state: closed
author: actionless
labels:
  - bug
assignees: []
created_at: 2022-12-20T20:44:54Z
updated_at: 2022-12-20T21:29:34Z
url: https://github.com/astral-sh/ruff/issues/1303
synced_at: 2026-01-12T15:54:41Z
```

# False-positive `F821: Undefined name` with `mypy_extensions.DefaultNamedArg`

---

_@actionless_

```python
from typing import Any, Callable

from mypy_extensions import DefaultNamedArg

SomeCallableT = Callable[[Any, DefaultNamedArg(bool | None, name="some_prop_name")], None]
```

```console
$ ruff test_ruff_F821.py
Found 1 error(s).
test_ruff_F821.py:5:66: F821 Undefined name `some_prop_name`
```

---

_Renamed from "False-positive `F821Undefined name` with `mypy_extensions.DefaultNamedArg`" to "False-positive `F821: Undefined name` with `mypy_extensions.DefaultNamedArg`" by @actionless on 2022-12-20 20:45_

---

_Label `bug` added by @charliermarsh on 2022-12-20 21:07_

---

_Comment by @charliermarsh on 2022-12-20 21:07_

Good stuff, thanks for filing, an easy fix.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-20 21:11_

---

_Closed by @charliermarsh on 2022-12-20 21:29_

---
