---
number: 699
title: Deletions need to avoid creating unused blocks
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2022-11-12T16:29:08Z
updated_at: 2022-11-12T16:39:10Z
url: https://github.com/astral-sh/ruff/issues/699
synced_at: 2026-01-07T13:12:14-06:00
---

# Deletions need to avoid creating unused blocks

---

_Issue opened by @charliermarsh on 2022-11-12 16:29_

We used to handle this, but I think it got lost in a refactor.

In short:

```py
if True:
    import os
    import sys
```

When we remove `import os`, it's fine; but if we then remove `import sys`, we have to leave a `pass` to retain valid syntax.


---

_Label `bug` added by @charliermarsh on 2022-11-12 16:29_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-12 16:29_

---

_Referenced in [astral-sh/ruff#700](../../astral-sh/ruff/pulls/700.md) on 2022-11-12 16:39_

---

_Closed by @charliermarsh on 2022-11-12 16:39_

---
