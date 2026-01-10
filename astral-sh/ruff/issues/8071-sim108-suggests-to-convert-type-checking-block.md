```yaml
number: 8071
title: SIM108 suggests to convert TYPE_CHECKING block
type: issue
state: closed
author: mrcljx
labels:
  - bug
assignees: []
created_at: 2023-10-19T18:08:56Z
updated_at: 2023-10-19T23:15:55Z
url: https://github.com/astral-sh/ruff/issues/8071
synced_at: 2026-01-10T11:09:50Z
```

# SIM108 suggests to convert TYPE_CHECKING block

---

_Issue opened by @mrcljx on 2023-10-19 18:08_

When using type checkers like Mypy, it's sometimes necessary to give them different code to inspect. This is done via `if TYPE_CHECKING` blocks, e.g.

```
from typing import TYPE_CHECKING
import functools

def _retry(callback, count: int) -> None:
  pass

if TYPE_CHECKING:
  retry = _retry
else:
  retry = functools.partial(_retry, count=3)
```

The rule SIM108 will emit (or `--unsafe-fixes`):

```
example.py:7:1: SIM108 Use ternary operator `retry = _retry if TYPE_CHECKING else functools.partial(_retry, count=3)` instead of `if`-`else`-block
```

In that case, type checkers like Mypy would not enable their special handling of `if TYPE_CHECKING` though, inferring the type of `retry` as a union type between the two expressions.

The SIM108 rule should ignore this case.

- `ruff --isolated --select SIM108 example.py`
- Version: 0.1.0


---

_Comment by @charliermarsh on 2023-10-19 18:09_

Yeah this is definitely wrong -- thanks for reporting.

---

_Label `bug` added by @charliermarsh on 2023-10-19 18:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-19 18:47_

---

_Closed by @charliermarsh on 2023-10-19 23:15_

---
