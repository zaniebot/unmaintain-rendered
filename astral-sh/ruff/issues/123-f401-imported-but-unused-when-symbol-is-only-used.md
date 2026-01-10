---
number: 123
title: "`F401 imported but unused` when symbol is only used for type hints"
type: issue
state: closed
author: amotl
labels: []
assignees: []
created_at: 2022-09-07T21:35:05Z
updated_at: 2022-09-08T08:07:08Z
url: https://github.com/astral-sh/ruff/issues/123
synced_at: 2026-01-10T01:22:37Z
---

# `F401 imported but unused` when symbol is only used for type hints

---

_Issue opened by @amotl on 2022-09-07 21:35_

Hi Charlie,

maybe similar to #83, `ruff` also marks spots as "imported but unused", where the imported symbols are only used within type hints.

With kind regards,
Andreas.

#### Example
```python
import dataclasses
from datetime import datetime


@dataclasses.dataclass
class Message:
    message: str
    datetime: datetime
```


---

_Renamed from "F401 `datetime.datetime` imported but unused" to "`F401 imported but unused` when symbol is only used for type hints" by @amotl on 2022-09-07 23:37_

---

_Comment by @charliermarsh on 2022-09-08 02:34_

Wow, this is really tricky, I think it's because the type hint is `datetime: datetime` (as opposed to something like `x: datetime`), so we were parsing the left-hand side first, then assumed `: datetime` was a reference to that left-hand side. Good catch :)

---

_Referenced in [astral-sh/ruff#127](../../astral-sh/ruff/pulls/127.md) on 2022-09-08 02:34_

---

_Closed by @charliermarsh on 2022-09-08 02:35_

---

_Comment by @amotl on 2022-09-08 08:07_

Oh wow, so I've catched that case accidentally ;]. Thank you for fixing it so quickly again!

---

_Referenced in [astral-sh/ruff#139](../../astral-sh/ruff/issues/139.md) on 2022-09-10 12:13_

---
