```yaml
number: 3413
title: RUF100 auto-fix remove comments
type: issue
state: closed
author: JonathanPlasse
labels:
  - bug
  - suppression
assignees: []
created_at: 2023-03-09T09:46:35Z
updated_at: 2023-03-10T22:57:16Z
url: https://github.com/astral-sh/ruff/issues/3413
synced_at: 2026-01-10T11:09:46Z
```

# RUF100 auto-fix remove comments

---

_Issue opened by @JonathanPlasse on 2023-03-09 09:46_

```python
# noqa # first comment
# noqa: E501 # second comment
```
The comments after the `noqa`s are removed after the auto-fix.

---

_Label `bug` added by @charliermarsh on 2023-03-09 13:57_

---

_Label `noqa` added by @charliermarsh on 2023-03-09 13:57_

---

_Closed by @charliermarsh on 2023-03-10 22:57_

---
