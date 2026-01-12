```yaml
number: 11733
title: "False-positive `TCH003`: `lint.typing-modules` setting is not honored in the typing module itself"
type: issue
state: closed
author: bersbersbers
labels:
  - bug
assignees: []
created_at: 2024-06-04T07:25:10Z
updated_at: 2025-02-07T09:07:14Z
url: https://github.com/astral-sh/ruff/issues/11733
synced_at: 2026-01-12T15:54:51Z
```

# False-positive `TCH003`: `lint.typing-modules` setting is not honored in the typing module itself

---

_@bersbersbers_

`lint.typing-modules` allows defining compatibility modules whose exports will be treated the same as the exports from the `typing` module. However, this does not seem to work in the module itself - `ruff` does not seem to understand that `TYPE_CHECKING` defines a type-checking block in this example:

`bug.py`
```python
from __future__ import annotations

import typing

def is_type_checking() -> bool:
    # This will be more complicated, obviously
    return 4 < 10

# This should be considered a typing module's TYPE_CHECKING variable:
TYPE_CHECKING = typing.TYPE_CHECKING or is_type_checking()

if TYPE_CHECKING:
    # So I consider this a false-negative TCH003
    from collections.abc import Iterable

def fun(_var: Iterable[str]) -> None:
    return None
```

`pyproject.toml`
```toml
[tool.ruff]
lint.select = ["TCH003"]
lint.typing-modules = ["bug"]
```

`ruff check bug.py`
```
bug.py:14:33: TCH003 Move standard library import `collections.abc.Iterable` into a type-checking block
Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

`ruff --version`
```
ruff 0.4.7
```


---

_Label `bug` added by @charliermarsh on 2024-06-04 13:22_

---

_Comment by @bersbersbers on 2025-02-07 08:55_

I think this can be closed as a subset/duplicate of the now-closed https://github.com/astral-sh/ruff/pull/15719.

---

_Closed by @MichaReiser on 2025-02-07 09:07_

---
