---
number: 4939
title: "[bug] F401 false positive for imports inside functions"
type: issue
state: closed
author: smackesey
labels:
  - bug
assignees: []
created_at: 2023-06-07T20:45:36Z
updated_at: 2023-06-07T21:56:05Z
url: https://github.com/astral-sh/ruff/issues/4939
synced_at: 2026-01-07T13:12:15-06:00
---

# [bug] F401 false positive for imports inside functions

---

_Issue opened by @smackesey on 2023-06-07 20:45_

Contrived example, but ruff 0.0.271 flags the below import inside of `fn` with F401 (unused import) and auto-deletes it, but this results in a runtime error when you execute the code, because the first argument to `cast` doesn't get a deferred annotation like a function parameter:

```
from __future__ import annotations

from typing import TYPE_CHECKING, cast

if TYPE_CHECKING:
    from threading import Thread


def fn(thread: Thread):
    from threading import Thread  # Flagged as F401

    casted_thread = cast(Thread, thread)
    return casted_thread

print(fn(1))
```

You can work around it by quoting the cast argument, but still a bug.

---

_Renamed from "F401 false positive for imports inside functions" to "[bug] F401 false positive for imports inside functions" by @smackesey on 2023-06-07 20:46_

---

_Comment by @charliermarsh on 2023-06-07 20:57_

Oops, thanks. I’ll fix this tonight. I need to cut a new release anyway.

---

_Label `bug` added by @charliermarsh on 2023-06-07 20:57_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-07 21:20_

---

_Referenced in [astral-sh/ruff#4942](../../astral-sh/ruff/pulls/4942.md) on 2023-06-07 21:48_

---

_Closed by @charliermarsh on 2023-06-07 21:56_

---
