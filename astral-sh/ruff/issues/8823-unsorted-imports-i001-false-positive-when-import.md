---
number: 8823
title: "`unsorted-imports` (`I001`) false positive when `import __future__` is below `from __future__` import"
type: issue
state: closed
author: DetachHead
labels:
  - bug
assignees: []
created_at: 2023-11-23T07:05:13Z
updated_at: 2023-12-07T03:56:24Z
url: https://github.com/astral-sh/ruff/issues/8823
synced_at: 2026-01-10T01:22:48Z
---

# `unsorted-imports` (`I001`) false positive when `import __future__` is below `from __future__` import

---

_Issue opened by @DetachHead on 2023-11-23 07:05_

```py
from __future__ import annotations  # I001: Import block is un-sorted or un-formatted
import __future__
```
https://play.ruff.rs/064dd932-8bfa-447c-b8a6-458a27ac6ba4

running `ruf --fix` causes `F404` and a runtime syntax error:
```py
import __future__  # F404: `from __future__` imports must occur at the beginning of the file
from __future__ import annotations
```
```
SyntaxError: from __future__ imports must occur at the beginning of the file
```

(i can't think of a reason you'd ever want to import the whole `__future__` module so this is probably low priority)

---

_Comment by @charliermarsh on 2023-11-23 09:53_

Hah great find.

---

_Label `bug` added by @charliermarsh on 2023-11-23 09:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-07 03:34_

---

_Referenced in [astral-sh/ruff#9039](../../astral-sh/ruff/pulls/9039.md) on 2023-12-07 03:34_

---

_Closed by @charliermarsh on 2023-12-07 03:56_

---
