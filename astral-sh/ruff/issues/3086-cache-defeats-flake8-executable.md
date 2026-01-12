```yaml
number: 3086
title: cache defeats flake8-executable
type: issue
state: closed
author: dimbleby
labels:
  - bug
assignees: []
created_at: 2023-02-21T17:35:20Z
updated_at: 2023-02-21T23:20:08Z
url: https://github.com/astral-sh/ruff/issues/3086
synced_at: 2026-01-12T15:54:43Z
```

# cache defeats flake8-executable

---

_@dimbleby_

```
$ ruff .
foo.py::1:3: EXE001 Shebang is present but file is not executable
Found 1 error.

$ chmod +x foo.py

$ ruff .
foo.py::1:3: EXE001 Shebang is present but file is not executable
Found 1 error.
```
Need to clear the ruff cache to get it to notice that this is fixed.

---

_Label `bug` added by @charliermarsh on 2023-02-21 17:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-21 19:27_

---

_Comment by @charliermarsh on 2023-02-21 19:35_

We need to use `ctime` on Unix platforms.

---

_Closed by @charliermarsh on 2023-02-21 23:20_

---
