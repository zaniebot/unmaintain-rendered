```yaml
number: 3271
title: "[Autofix error] SIM300 Yoda condition (Ruff 0.0.253)"
type: issue
state: closed
author: akx
labels:
  - bug
assignees: []
created_at: 2023-02-28T11:05:24Z
updated_at: 2023-02-28T16:59:04Z
url: https://github.com/astral-sh/ruff/issues/3271
synced_at: 2026-01-12T15:54:43Z
```

# [Autofix error] SIM300 Yoda condition (Ruff 0.0.253)

---

_@akx_

```bash
$ cat sim300bug.py
assert SomeClass().settings.SOME_CONSTANT_VALUE > (60 * 60)
$ ruff --version
ruff 0.0.253
$ ruff --isolated --select=SIM300 --fix sim300bug.py
error: Autofix introduced a syntax error. Reverting all changes.
[...]
sim300bug.py:1:8: SIM300 Yoda conditions are discouraged, use `(60 * 60) < SomeClass().settings.SOME_CONSTANT_VALUE` instead
$
```

---

_Label `bug` added by @charliermarsh on 2023-02-28 15:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-28 16:52_

---

_Closed by @charliermarsh on 2023-02-28 16:59_

---
