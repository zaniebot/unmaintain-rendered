---
number: 14762
title: "False F401 with annotations and `no_type_check`."
type: issue
state: closed
author: coady
labels: []
assignees: []
created_at: 2024-12-04T01:17:18Z
updated_at: 2024-12-04T01:19:43Z
url: https://github.com/astral-sh/ruff/issues/14762
synced_at: 2026-01-07T13:12:16-06:00
---

# False F401 with annotations and `no_type_check`.

---

_Issue opened by @coady on 2024-12-04 01:17_

Using v0.8.1.
```python
from collections.abc import Iterable
from typing import no_type_check

@no_type_check
def func(it: Iterable):
    ...
```
outputs
```
.py:1:29: F401 [*] `collections.abc.Iterable` imported but unused
  |
1 | from collections.abc import Iterable
```


---

_Comment by @charliermarsh on 2024-12-04 01:19_

This was reverted in #14726.

---

_Closed by @charliermarsh on 2024-12-04 01:19_

---
