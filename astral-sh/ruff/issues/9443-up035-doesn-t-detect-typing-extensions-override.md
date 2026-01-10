```yaml
number: 9443
title: "`UP035` doesn't detect `typing_extensions.override` when targeting python 3.12"
type: issue
state: closed
author: DetachHead
labels:
  - rule
assignees: []
created_at: 2024-01-09T04:47:26Z
updated_at: 2024-01-09T17:52:03Z
url: https://github.com/astral-sh/ruff/issues/9443
synced_at: 2026-01-10T11:09:51Z
```

# `UP035` doesn't detect `typing_extensions.override` when targeting python 3.12

---

_Issue opened by @DetachHead on 2024-01-09 04:47_

```py
from typing_extensions import override  # no error
```
https://play.ruff.rs/f12262a2-a134-4035-8901-6e1affb88555

`override` was added to the `typing` module in 3.12, so ruff should complain here

---

_Renamed from "`UP035` doesn't detact `typing_extensioons.override` when targeting python 3.12" to "`UP035` doesn't detact `typing_extensions.override` when targeting python 3.12" by @DetachHead on 2024-01-09 04:48_

---

_Renamed from "`UP035` doesn't detact `typing_extensions.override` when targeting python 3.12" to "`UP035` doesn't detect `typing_extensions.override` when targeting python 3.12" by @DetachHead on 2024-01-09 04:48_

---

_Label `rule` added by @charliermarsh on 2024-01-09 05:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-09 05:48_

---

_Closed by @charliermarsh on 2024-01-09 17:52_

---

_Closed by @charliermarsh on 2024-01-09 17:52_

---
