```yaml
number: 720
title: Import tracking should take aliases into account
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-11-13T15:53:33Z
updated_at: 2022-11-15T02:29:31Z
url: https://github.com/astral-sh/ruff/issues/720
synced_at: 2026-01-12T15:54:40Z
```

# Import tracking should take aliases into account

---

_@charliermarsh_

E.g., if we do `from typing import List as CustomList`, we should detect that `CustomList[...]` is a subscript annotation.


---

_Label `enhancement` added by @charliermarsh on 2022-11-13 15:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-14 16:01_

---

_Closed by @charliermarsh on 2022-11-15 02:29_

---
