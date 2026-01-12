```yaml
number: 8310
title: "`S604` shows warning for any type, not just `True`"
type: issue
state: closed
author: adamhl8
labels:
  - documentation
assignees: []
created_at: 2023-10-28T16:54:07Z
updated_at: 2023-10-30T15:44:39Z
url: https://github.com/astral-sh/ruff/issues/8310
synced_at: 2026-01-12T15:54:48Z
```

# `S604` shows warning for any type, not just `True`

---

_@adamhl8_

Say you have something like this:
```python
def do_stuff(shell):
    pass
```

Calling this function with any argument for `shell` will show `S604`, not just `True` as the warning/documentation implies.

```python
do_stuff(shell="bash")
# S604 Function call with `shell=True` parameter identified, security issue
```

```python
do_stuff(shell=123)
# S604 Function call with `shell=True` parameter identified, security issue
```

---

_Comment by @charliermarsh on 2023-10-28 22:36_

I believe this is as intended and matches Bandit's behavior -- I think the documentation is the problem.

---

_Label `documentation` added by @charliermarsh on 2023-10-28 22:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-30 13:48_

---

_Closed by @charliermarsh on 2023-10-30 15:44_

---
