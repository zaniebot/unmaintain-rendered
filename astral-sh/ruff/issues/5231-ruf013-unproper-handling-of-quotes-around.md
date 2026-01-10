```yaml
number: 5231
title: "RUF013: unproper handling of quotes around annotations"
type: issue
state: closed
author: Alexerson
labels:
  - bug
assignees: []
created_at: 2023-06-20T23:47:21Z
updated_at: 2023-06-21T14:23:39Z
url: https://github.com/astral-sh/ruff/issues/5231
synced_at: 2026-01-10T11:09:47Z
```

# RUF013: unproper handling of quotes around annotations

---

_Issue opened by @Alexerson on 2023-06-20 23:47_

The new rule RUF013 doesn’t properly handle string annotations.

I would expect the following to be fine:
```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from datetime import datetime

def last_value(timestamp: 'datetime | None'=None) -> float ...
```

but with the latest version, I get: ``RUF013 [*] PEP 484 prohibits implicit `Optional` `` and the `--fix` version will replace it by:
```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from datetime import datetime

def last_value(timestamp: 'datetime | None' |None=None) -> float ...
```
which is not valid and will give me:
`TypeError: unsupported operand type(s) for |: 'str' and 'NoneType'`


---

_Label `bug` added by @charliermarsh on 2023-06-20 23:52_

---

_Comment by @charliermarsh on 2023-06-20 23:52_

Thank you.

---

_Comment by @charliermarsh on 2023-06-20 23:55_

I’ll fix this and include it in tonight’s release.

---

_Comment by @Alexerson on 2023-06-20 23:59_

Thanks, it’s very appreciated!

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-06-21 05:03_

---

_Closed by @charliermarsh on 2023-06-21 14:23_

---
