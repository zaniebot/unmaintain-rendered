```yaml
number: 1310
title: false positive for RET504 in case of globals
type: issue
state: closed
author: tlambert03
labels:
  - bug
assignees: []
created_at: 2022-12-21T17:59:53Z
updated_at: 2022-12-24T17:59:24Z
url: https://github.com/astral-sh/ruff/issues/1310
synced_at: 2026-01-10T12:05:25Z
```

# false positive for RET504 in case of globals

---

_Issue opened by @tlambert03 on 2022-12-21 17:59_

The following code results in `RET504 Unnecessary variable assignment before `return` statement`
 
```python
X: int | None = None

def get_x() -> int:
    global X
    if X is None:
        X = 1
    return X
```

however, as it's modifying a global, it's not unnecessary assignment.  (naturally, whether this is a good pattern is a separate discussion :joy: ... but it's not syntactically unnecessary).

(Haven't checked the behavior of flake8-return)

---

_Comment by @charliermarsh on 2022-12-21 18:45_

Hah! Yeah, globals should probably be omitted from this check, since assignments are themselves side-effects.

---

_Label `bug` added by @charliermarsh on 2022-12-21 18:45_

---

_Closed by @charliermarsh on 2022-12-24 17:02_

---

_Comment by @tlambert03 on 2022-12-24 17:51_

Thank you! 🙏

---

_Comment by @charliermarsh on 2022-12-24 17:59_

All @squiddy :)

---
