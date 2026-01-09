---
number: 7073
title: Rule UP007 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T09:25:48Z
updated_at: 2023-09-03T13:23:37Z
url: https://github.com/astral-sh/ruff/issues/7073
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule UP007 cause autofix error

---

_Issue opened by @qarmin on 2023-09-03 09:25_

Ruff 0.0.287 (latest changes from main branch)

```
ruff --fix *.py  --no-cache --select UP007
```

file content:
```
from __future__ import annotations
from typing import Optional, Union
def jsc(
):
    def _recouple_step(
    ) -> Optional[TensorOperator, TensorOperatorComposite]:
        out_tensor = cls._recouple_substructure(tensor_op)  # First recouple innermost layers
```

error:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `6332010.py`, the rule codes UP007, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

6332010.py:6:10: UP007 Use `X | Y` for type annotations
Found 1 error.

```

[6332010.py.zip](https://github.com/astral-sh/ruff/files/12505618/6332010.py.zip)


---

_Label `bug` added by @charliermarsh on 2023-09-03 12:02_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-03 12:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 13:11_

---

_Referenced in [astral-sh/ruff#7079](../../astral-sh/ruff/pulls/7079.md) on 2023-09-03 13:15_

---

_Closed by @charliermarsh on 2023-09-03 13:23_

---
