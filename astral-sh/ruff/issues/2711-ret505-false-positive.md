---
number: 2711
title: RET505 false positive
type: issue
state: closed
author: trag1c
labels:
  - bug
assignees: []
created_at: 2023-02-10T05:01:56Z
updated_at: 2023-02-15T20:51:01Z
url: https://github.com/astral-sh/ruff/issues/2711
synced_at: 2026-01-10T01:22:41Z
---

# RET505 false positive

---

_Issue opened by @trag1c on 2023-02-10 05:01_

It seems like it's not looking behind enough:
```py
def __sub__(self, other: Any) -> Array[T]:
    new_array = self.val.copy()
    if isinstance(other, Array):
        ...  # no returns here
    elif isinstance(other, Number):
        ...  # no returns here
    elif other is NULL:
        return self.val.pop()
    else:  # RET505 Unnecessary `else` after `return` statement
        raise NotDefinedError(f"Array - {get_type_name(other)}")
    return Array(new_array)
```

---

_Comment by @charliermarsh on 2023-02-10 13:27_

The dreaded `RET505`...

---

_Label `bug` added by @charliermarsh on 2023-02-10 13:37_

---

_Comment by @charliermarsh on 2023-02-10 13:38_

FWIW, I think it wants you to do this (which is less clear):

```py
def __sub__(self, other: Any) -> Array[T]:
    new_array = self.val.copy()
    if isinstance(other, Array):
        ...  # no returns here
    elif isinstance(other, Number):
        ...  # no returns here
    else:
        if other is NULL:
            return self.val.pop()
        raise NotDefinedError(f"Array - {get_type_name(other)}")
    return Array(new_array)
```

---

_Comment by @charliermarsh on 2023-02-15 20:51_

I believe this was fixed in #2881.

---

_Closed by @charliermarsh on 2023-02-15 20:51_

---
