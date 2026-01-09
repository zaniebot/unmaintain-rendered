---
number: 5138
title: DJ008 in model type stub is incompatible with PYI029
type: issue
state: closed
author: federicobond
labels:
  - bug
assignees: []
created_at: 2023-06-16T03:39:57Z
updated_at: 2023-06-16T04:03:16Z
url: https://github.com/astral-sh/ruff/issues/5138
synced_at: 2026-01-07T13:12:15-06:00
---

# DJ008 in model type stub is incompatible with PYI029

---

_Issue opened by @federicobond on 2023-06-16 03:39_

When a Django model type stub contains a `__str__` method, ruff complains about PYI029.

```
PYI029 [*] Defining `__str__` in a stub is almost always redundant
```
The autofix though triggers a DJ008:
```
DJ008 Model does not define `__str__` method
```

Given that DJ008 is about providing an override implementation for the default model `__str__` method, it probably does not make sense to enforce DJ008 in .pyi stub files.

---

_Comment by @charliermarsh on 2023-06-16 03:41_

Good call -- I will fix this real quick.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-16 03:41_

---

_Label `bug` added by @charliermarsh on 2023-06-16 03:41_

---

_Referenced in [astral-sh/ruff#5139](../../astral-sh/ruff/pulls/5139.md) on 2023-06-16 03:43_

---

_Comment by @federicobond on 2023-06-16 03:46_

Wow, that's fast! Thank you.

---

_Closed by @charliermarsh on 2023-06-16 03:49_

---

_Comment by @charliermarsh on 2023-06-16 04:03_

No problem, thanks for reporting. I may cut a release tomorrow, but otherwise it'll go out some time early next week.

---
