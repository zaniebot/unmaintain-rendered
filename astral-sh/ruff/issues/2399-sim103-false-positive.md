```yaml
number: 2399
title: "SIM103: false positive"
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-01-31T15:58:04Z
updated_at: 2023-01-31T17:45:53Z
url: https://github.com/astral-sh/ruff/issues/2399
synced_at: 2026-01-10T11:09:45Z
```

# SIM103: false positive

---

_Issue opened by @spaceone on 2023-01-31 15:58_

```
def foo(exc):
    if exc.getcode() == 401:
        return False
    else:
        return False
```
causes:
```foo.py:2:5: SIM103 Return the condition `exc.getcode() == 401` directly```

---

_Label `bug` added by @charliermarsh on 2023-01-31 17:26_

---

_Comment by @charliermarsh on 2023-01-31 17:26_

Definitely a bug, but where did you run into this :joy:

---

_Comment by @spaceone on 2023-01-31 17:39_

https://github.com/univention/univention-corporate-server/commit/37e560e5282fab764ff6496131b28bf0d492b900#diff-45af649ef64d42abac0d51f47809588eaf0aff8501ff6fc0dc6a8580134f25c1R141-R144 ;-)

---

_Comment by @ngnpope on 2023-01-31 17:40_

Amazing 😆 

`SIM103` definitely doesn't make sense. It could become:

```python
def foo(exc):
    exc.getcode()  # Still have to call because of potential side effects.
    return False
```

But then there could also be side effects if someone overloaded `__eq__` to do some evil... 🙈 

Maybe a check to identify that all branches have the same statements within?

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-31 17:41_

---

_Closed by @charliermarsh on 2023-01-31 17:45_

---
