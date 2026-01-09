---
number: 6541
title: Warn on interpreter startup when packages were skipped during sync
type: issue
state: open
author: zanieb
labels:
  - enhancement
  - error messages
assignees: []
created_at: 2024-08-23T19:30:15Z
updated_at: 2024-08-23T19:30:25Z
url: https://github.com/astral-sh/uv/issues/6541
synced_at: 2026-01-07T13:12:17-06:00
---

# Warn on interpreter startup when packages were skipped during sync

---

_Issue opened by @zanieb on 2024-08-23 19:30_

e.g. with something like

```
$ cat .venv/lib/python3.12/site-packages/_root_package.pth
import warnings; warnings.warn(Warning("root_package installation was skipped"))
```

when #6538 #6539 #6540 are used

We'd need to make sure to clean it up.

---

_Label `enhancement` added by @zanieb on 2024-08-23 19:30_

---

_Label `error messages` added by @zanieb on 2024-08-23 19:30_

---
