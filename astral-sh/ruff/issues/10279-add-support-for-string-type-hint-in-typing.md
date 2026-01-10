---
number: 10279
title: Add support for string type hint in typing.Annotated
type: issue
state: closed
author: minmax
labels:
  - bug
assignees: []
created_at: 2024-03-07T18:24:19Z
updated_at: 2024-03-08T00:51:55Z
url: https://github.com/astral-sh/ruff/issues/10279
synced_at: 2026-01-10T01:22:49Z
---

# Add support for string type hint in typing.Annotated

---

_Issue opened by @minmax on 2024-03-07 18:24_

String type hints used in Annotated should be imported in TYPE_CHECKING section, but ruff remove imports for now.
I think ruff should treat the first argument of Annotated similar to the current behavior for ClassVar.

```python
from types import FunctionType  # `types.FunctionType` imported but unused F401
from typing import Annotated

Func = Annotated["FunctionType", int]
```

ruff 0.3.0

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-07 22:01_

---

_Label `bug` added by @charliermarsh on 2024-03-07 22:01_

---

_Comment by @charliermarsh on 2024-03-07 22:11_

This looks like a bug, probably just an oversight...

---

_Referenced in [astral-sh/ruff#10285](../../astral-sh/ruff/pulls/10285.md) on 2024-03-08 00:36_

---

_Closed by @charliermarsh on 2024-03-08 00:51_

---
