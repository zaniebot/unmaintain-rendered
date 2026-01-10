```yaml
number: 1096
title: "F841 detect unused variable in `with` statement"
type: issue
state: closed
author: tekumara
labels:
  - bug
assignees: []
created_at: 2022-12-06T01:49:31Z
updated_at: 2022-12-06T03:09:46Z
url: https://github.com/astral-sh/ruff/issues/1096
synced_at: 2026-01-10T12:06:16Z
```

# F841 detect unused variable in `with` statement

---

_Issue opened by @tekumara on 2022-12-06 01:49_

```python
def foo():
    with open("file") as f:
        print("hello")
```

ruff doesn't detect anything but flake8 does:
```
2     F841 local variable 'f' is assigned to but never used
```

ruff 0.0.162

---

_Comment by @charliermarsh on 2022-12-06 02:20_

This was changed intentionally in https://github.com/charliermarsh/ruff/pull/897, but I think that fix probably should've been confined to unpacking assignments.

---

_Label `bug` added by @charliermarsh on 2022-12-06 02:20_

---

_Comment by @charliermarsh on 2022-12-06 02:28_

Will fix.

---

_Closed by @charliermarsh on 2022-12-06 03:09_

---
