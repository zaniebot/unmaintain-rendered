```yaml
number: 6030
title: "False positive F821: string Literal in combintation with future import"
type: issue
state: closed
author: twoertwein
labels:
  - bug
assignees: []
created_at: 2023-07-24T13:59:19Z
updated_at: 2023-07-24T15:09:42Z
url: https://github.com/astral-sh/ruff/issues/6030
synced_at: 2026-01-12T15:54:45Z
```

# False positive F821: string Literal in combintation with future import

---

_@twoertwein_

I believe this was not triggered with 0.0.278 but is triggered with 0.0.280
```py
from __future__ import annotations  # without this line ruff does not report F821

from typing import Literal, Final

from typing_extensions import assert_type

CONSTANT: Final = "ns"

assert_type(CONSTANT, Literal["ns"])
```
```sh
ruff --select F821 test.py 
test.py:9:32: F821 Undefined name `ns`
```



---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-24 13:59_

---

_Comment by @charliermarsh on 2023-07-24 14:03_

Thanks, I'll take a look.

---

_Label `bug` added by @charliermarsh on 2023-07-24 14:03_

---

_Closed by @charliermarsh on 2023-07-24 15:09_

---
