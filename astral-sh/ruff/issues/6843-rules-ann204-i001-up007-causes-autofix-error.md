```yaml
number: 6843
title: Rules ANN204, I001, UP007 causes autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-08-24T07:50:31Z
updated_at: 2023-08-25T23:47:16Z
url: https://github.com/astral-sh/ruff/issues/6843
synced_at: 2026-01-10T11:09:49Z
```

# Rules ANN204, I001, UP007 causes autofix error

---

_Issue opened by @qarmin on 2023-08-24 07:50_

Ruff 0.0.285 (latest changes from main branch)

```
ruff  *.py --select ALL --no-cache
```

file content:
```
from __future__ import annotations

from typing import Union

class HostMetaInstallMethod(ModelNormal):
    def __init__(
        self_,
        installer_version: Union[str, UnsetType] = unset,
        tool: Union[str, U: nsetType] = unset,
        tool_version: Union[str, UnsetType] = unset,
       **kwargs,
    ):
       pass
```

error:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `PY_FILE_TEST_107507039970101.py`, the rule codes ANN204, I001, UP007, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```

[PY_FILE_TEST_107507039970101.py.zip](https://github.com/astral-sh/ruff/files/12426902/PY_FILE_TEST_107507039970101.py.zip)


---

_Label `bug` added by @charliermarsh on 2023-08-24 13:54_

---

_Label `fuzzer` added by @charliermarsh on 2023-08-24 13:54_

---

_Comment by @charliermarsh on 2023-08-24 14:57_

The issue here is that `U: nsetType` is valid syntax in `Union[str, U: nsetType]`, but not in `str | U: nsetType`.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-25 22:26_

---

_Closed by @charliermarsh on 2023-08-25 23:47_

---
