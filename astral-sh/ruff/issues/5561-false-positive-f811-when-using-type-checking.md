---
number: 5561
title: False positive F811 when using TYPE_CHECKING
type: issue
state: closed
author: phil65
labels:
  - bug
assignees: []
created_at: 2023-07-06T13:46:54Z
updated_at: 2024-01-10T20:09:25Z
url: https://github.com/astral-sh/ruff/issues/5561
synced_at: 2026-01-10T01:22:44Z
---

# False positive F811 when using TYPE_CHECKING

---

_Issue opened by @phil65 on 2023-07-06 13:46_

For following code
```py
from typing import TYPE_CHECKING:
...
if TYPE_CHECKING:
    from x import y

...

if __name__ == "__main__":
    from x import y
    ...
```
Error F811 gets reported, which should not be the case I think.
Thank you for this great tool!

---

_Renamed from "False positive " to "False positive F811 when using TYPE_CHECKING" by @phil65 on 2023-07-06 13:47_

---

_Comment by @zanieb on 2023-07-06 13:52_

Thanks for the issue! This indeed looks incorrect. Since this is just a small example, I'm curious why you're using this pattern in the real world?

Regardless, it seems like `from x import y` should be allowed in a new scope if the previous import was in an if-type-checking block.

You would expect the following to get reported still, right?

```python
from typing import TYPE_CHECKING:

if TYPE_CHECKING:
    from x import y

from x import y
```



---

_Label `bug` added by @zanieb on 2023-07-06 13:52_

---

_Comment by @phil65 on 2023-07-06 13:58_

Yes, I would expect that to get reported.
I am often using the `if __name__ == "__main__":` section for testing purposes, that´s why I am using this pattern.

---

_Referenced in [astral-sh/ruff#9418](../../astral-sh/ruff/pulls/9418.md) on 2024-01-07 02:00_

---

_Comment by @charliermarsh on 2024-01-10 20:09_

This no longer gets triggered (in the next version).

---

_Closed by @charliermarsh on 2024-01-10 20:09_

---
