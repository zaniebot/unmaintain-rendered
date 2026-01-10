```yaml
number: 5787
title: "Cannot ignore `TCH004` via config file "
type: issue
state: closed
author: deadnews
labels:
  - bug
assignees: []
created_at: 2023-07-15T20:35:19Z
updated_at: 2023-07-15T20:57:31Z
url: https://github.com/astral-sh/ruff/issues/5787
synced_at: 2026-01-10T11:09:48Z
```

# Cannot ignore `TCH004` via config file 

---

_Issue opened by @deadnews on 2023-07-15 20:35_

Same behavior as https://github.com/astral-sh/ruff/issues/3472 with `ruff 0.0.278`.

----
* A minimal code snippet that reproduces the bug.

```py
from __future__ import annotations

import importlib.util
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    import importlib.machinery


def from_spec(spec: importlib.machinery.ModuleSpec):
    return importlib.util.module_from_spec(spec)
```

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

```sh
$ poetry run ruff test.py                                                                                     
test.py:7:12: TCH004 Move import `importlib.machinery` out of type-checking block. Import is used for more than type hinting.
Found 1 error.
```

* The current Ruff settings (any relevant sections from your `pyproject.toml`).

```toml
[tool.ruff]
select = ["TCH"]
ignore = ["TCH004"]
```

* The current Ruff version (`ruff --version`).

```sh
$ poetry run ruff --version                                                                                                     
ruff 0.0.278
```








---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-15 20:38_

---

_Label `bug` added by @charliermarsh on 2023-07-15 20:38_

---

_Comment by @charliermarsh on 2023-07-15 20:38_

Thanks, sorry about that.

---

_Closed by @charliermarsh on 2023-07-15 20:57_

---
