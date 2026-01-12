```yaml
number: 8095
title: "`UP036` (outdated-version-block) does not work with slicing"
type: issue
state: closed
author: tdulcet
labels:
  - bug
assignees: []
created_at: 2023-10-20T15:46:19Z
updated_at: 2023-10-21T23:08:19Z
url: https://github.com/astral-sh/ruff/issues/8095
synced_at: 2026-01-12T15:54:47Z
```

# `UP036` (outdated-version-block) does not work with slicing

---

_@tdulcet_

```py
import sys

if sys.version_info[:2] < (3, 7):
    print('oh no')
```
This if block should be removed with `--fix`, but nothing changes. The [Ruff documentation](https://docs.astral.sh/ruff/rules/sys-version-slice3/) even suggests using this slice syntax here, so this should work as expected. (Maybe a new linter rule could be added to remove this unnecessary slice syntax as well.) 
```
ruff --target-version py37 --select UP036 --isolated test.py
```
```
ruff 0.1.1
```

_Originally posted by @tdulcet in https://github.com/astral-sh/ruff/issues/7902#issuecomment-1757807620_
            

---

_Label `bug` added by @charliermarsh on 2023-10-20 18:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-21 22:57_

---

_Closed by @charliermarsh on 2023-10-21 23:08_

---
