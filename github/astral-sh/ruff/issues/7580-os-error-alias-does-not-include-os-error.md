---
number: 7580
title: "`os-error-alias` does not include `os.error`"
type: issue
state: closed
author: Avasam
labels:
  - bug
assignees: []
created_at: 2023-09-21T19:48:31Z
updated_at: 2023-09-21T21:18:16Z
url: https://github.com/astral-sh/ruff/issues/7580
synced_at: 2026-01-07T13:12:15-06:00
---

# `os-error-alias` does not include `os.error`

---

_Issue opened by @Avasam on 2023-09-21 19:48_

`os.error` is an alias for `OSError`, but is not standardized as part of https://docs.astral.sh/ruff/rules/os-error-alias/

```py
>>> import os; OSError is os.error
True
```

```py
import os

try:
    pass
except (WindowsError, os.error):
    pass
```

becomes:
```py
import os

try:
    pass
except (OSError, os.error):
    pass
```

expected:
```py
import os  # This import is now unused and can be removed

try:
    pass
except OSError:
    pass
```

~~This would be a deviation from pyupgrade, I have opened the same request in parallel: https://github.com/asottile/pyupgrade/issues/892~~
Edit: This was just added in https://github.com/asottile/pyupgrade/pull/893

---

_Renamed from "`os-error-aliases` does not include `os.error`" to "`os-error-alias` does not include `os.error`" by @Avasam on 2023-09-21 19:51_

---

_Label `bug` added by @charliermarsh on 2023-09-21 20:53_

---

_Referenced in [astral-sh/ruff#7582](../../astral-sh/ruff/pulls/7582.md) on 2023-09-21 21:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-21 21:07_

---

_Closed by @charliermarsh on 2023-09-21 21:18_

---
