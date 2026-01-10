---
number: 2254
title: "--add-noqa removes not selected noqa statements"
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-01-27T14:26:53Z
updated_at: 2023-02-02T03:22:32Z
url: https://github.com/astral-sh/ruff/issues/2254
synced_at: 2026-01-10T01:22:40Z
---

# --add-noqa removes not selected noqa statements

---

_Issue opened by @spaceone on 2023-01-27 14:26_

```sh
$ cat foo.py
from foo import bar # noqa: E402,F401
$ ruff --select F401 --add-noqa foo.py
Added 1 noqa directives.
$ cat foo.py
from foo import bar  # noqa: F401
```

→ `--add-noqa` should not remove any non-selected codes.

---

_Label `bug` added by @charliermarsh on 2023-01-27 15:30_

---

_Comment by @charliermarsh on 2023-01-27 15:30_

I think this was an intentional decision but it might be the wrong one.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-02 02:45_

---

_Referenced in [astral-sh/ruff#2465](../../astral-sh/ruff/pulls/2465.md) on 2023-02-02 03:04_

---

_Closed by @charliermarsh on 2023-02-02 03:22_

---
