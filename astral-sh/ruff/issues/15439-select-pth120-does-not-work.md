```yaml
number: 15439
title: "--select=PTH120 does not work"
type: issue
state: closed
author: skraeven
labels:
  - bug
assignees: []
created_at: 2025-01-12T12:24:01Z
updated_at: 2025-01-13T00:55:48Z
url: https://github.com/astral-sh/ruff/issues/15439
synced_at: 2026-01-10T11:09:56Z
```

# --select=PTH120 does not work

---

_Issue opened by @skraeven on 2025-01-12 12:24_

Selecting [PTH120](https://docs.astral.sh/ruff/rules/os-path-dirname/) rule seems to always return 0 errors. 
The workaround is to drop the 0 (--select=PTH12) to actually get the errors.
I'm not aware of any other rules that has this behavior.

Example:
```
$ ruff check . --select=PTH120 --isolated
All checks passed!
$ ruff check . --select=PTH12 --isolated
test_pth120.py:3:1: PTH120 `os.path.dirname()` should be replaced by `Path.parent`
  |
1 | import os
2 | 
3 | os.path.dirname("foo/bar/baz")
  | ^^^^^^^^^^^^^^^ PTH120
  |

Found 1 error.
```

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-13 00:47_

---

_Label `bug` added by @charliermarsh on 2025-01-13 00:47_

---

_Closed by @charliermarsh on 2025-01-13 00:55_

---

_Closed by @charliermarsh on 2025-01-13 00:55_

---
