```yaml
number: 2759
title: E701 false positive after migrating from 244 tom 245
type: issue
state: closed
author: alexprengere
labels:
  - bug
assignees: []
created_at: 2023-02-11T08:19:16Z
updated_at: 2023-02-11T17:22:55Z
url: https://github.com/astral-sh/ruff/issues/2759
synced_at: 2026-01-12T15:54:43Z
```

# E701 false positive after migrating from 244 tom 245

---

_@alexprengere_

This used to be fine, but now raises `E701 Multiple statements on one line (colon)`:

```python
cond = False
func = lambda x: x** 2 if cond else lambda x:x
```



---

_Comment by @charliermarsh on 2023-02-11 10:07_

Thanks! Will fix.

---

_Label `bug` added by @charliermarsh on 2023-02-11 10:07_

---

_Closed by @charliermarsh on 2023-02-11 17:22_

---
