```yaml
number: 10205
title: "`futures.exception()` is flagged as \"unnecessary parens on exception\""
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-03-03T00:12:17Z
updated_at: 2024-03-03T00:28:53Z
url: https://github.com/astral-sh/ruff/issues/10205
synced_at: 2026-01-10T11:09:52Z
```

# `futures.exception()` is flagged as "unnecessary parens on exception"

---

_Issue opened by @charliermarsh on 2024-03-03 00:12_

An example from the python standard library: `concurrent.futures.Future.exception()` returns an exception raised by the called function, or `None` if no exception was raised.

```python
from concurrent.futures import ThreadPoolExecutor

with ThreadPoolExecutor() as executor:
    future = executor.submit(float, "a")
    if future.exception():
        raise future.exception()  # RSE102 Unnecessary parentheses on raised exception
```

_Originally posted by @joostmeulenbeld in https://github.com/astral-sh/ruff/issues/5416#issuecomment-1954151338_
            

---

_Label `bug` added by @charliermarsh on 2024-03-03 00:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-03 00:12_

---

_Closed by @charliermarsh on 2024-03-03 00:28_

---
