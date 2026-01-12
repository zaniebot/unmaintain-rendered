```yaml
number: 6756
title: Rule UP009 causes to show autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-08-22T07:48:30Z
updated_at: 2023-08-23T17:00:08Z
url: https://github.com/astral-sh/ruff/issues/6756
synced_at: 2026-01-12T15:54:46Z
```

# Rule UP009 causes to show autofix error

---

_@qarmin_

Ruff 0.0.285

```
ruff  *.py --select ALL --no-cache
```

file content:
```
 # -*- coding: utf-8 -*-
from __future__ import absolute_import
```

error:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `PY_FILE_TEST_2072608536.py`, the rule codes UP009, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
```

[PY_FILE_TEST_2072608536.py.zip](https://github.com/astral-sh/ruff/files/12406268/PY_FILE_TEST_2072608536.py.zip)


---

_Label `bug` added by @charliermarsh on 2023-08-22 12:51_

---

_Label `fuzzer` added by @charliermarsh on 2023-08-22 12:51_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-22 13:14_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-08-22 14:05_

---

_Assigned to @zanieb by @zanieb on 2023-08-22 15:29_

---

_Closed by @zanieb on 2023-08-23 17:00_

---
