---
number: 7966
title: TCH false positive with PEP-695 upper bound specification
type: issue
state: closed
author: bryanforbes
labels:
  - bug
assignees: []
created_at: 2023-10-15T21:30:48Z
updated_at: 2023-10-16T00:09:38Z
url: https://github.com/astral-sh/ruff/issues/7966
synced_at: 2026-01-10T01:22:47Z
---

# TCH false positive with PEP-695 upper bound specification

---

_Issue opened by @bryanforbes on 2023-10-15 21:30_

Running the following command with Ruff `0.0.292`:

```
ruff --target-version py312 --select TCH --isolated --no-cache blah.py
```

Given the following code:

```python
from __future__ import annotations

from collections.abc import Callable
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from .foo import Record

type RecordOrThings = Record | int | str
type RecordCallback[R: Record] = Callable[[R], None]


def process_record[R: Record](record: R) -> None:
    ...


class RecordContainer[R: Record]:
    def add_record(self, record: R) -> None:
        ...
```

`TCH004` is reported on line 7, but all uses of `Record` are only for annotations.

---

_Label `bug` added by @charliermarsh on 2023-10-15 22:48_

---

_Comment by @charliermarsh on 2023-10-15 22:48_

Does look like a bug.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-15 23:32_

---

_Referenced in [astral-sh/ruff#7968](../../astral-sh/ruff/pulls/7968.md) on 2023-10-16 00:00_

---

_Closed by @charliermarsh on 2023-10-16 00:09_

---
