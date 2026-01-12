```yaml
number: 5249
title: F821 false positive in else when except block locals matches try block locals.
type: issue
state: closed
author: jaraco
labels:
  - bug
assignees: []
created_at: 2023-06-21T12:46:58Z
updated_at: 2023-06-21T16:54:00Z
url: https://github.com/astral-sh/ruff/issues/5249
synced_at: 2026-01-12T15:54:45Z
```

# F821 false positive in else when except block locals matches try block locals.

---

_@jaraco_

Sometime between 0.0.265 and 0.0.274, ruff started failing on this case:

```
try:
    v = 3
except ImportError as v:
    print(v)
else:
    print(v)
```

with 

```
e.py:6:11: F821 Undefined name `v`
```

This issue was encountered in pypa/setuptools#3961.

---

_Renamed from "F821 false positive" to "F821 false positive in else when except block locals matches try block locals." by @jaraco on 2023-06-21 12:50_

---

_Comment by @charliermarsh on 2023-06-21 12:52_

Thanks, I’ll fix this today (and will add setuptools to our ecosystem CI check).

(FWIW I do recommend using a different variable name since v will be unbound after the else if you hit the except block, but we should still avoid this false positive.) 

---

_Label `bug` added by @charliermarsh on 2023-06-21 12:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-21 14:30_

---

_Closed by @charliermarsh on 2023-06-21 16:54_

---
