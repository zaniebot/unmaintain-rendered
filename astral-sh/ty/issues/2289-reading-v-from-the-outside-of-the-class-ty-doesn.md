---
number: 2289
title: "Reading `__v` from the outside of the class, ty doesn't give any error while Python interpreter gives error"
type: issue
state: closed
author: hyperkai
labels: []
assignees: []
created_at: 2025-12-31T07:58:31Z
updated_at: 2025-12-31T10:55:16Z
url: https://github.com/astral-sh/ty/issues/2289
synced_at: 2026-01-10T01:51:14Z
---

# Reading `__v` from the outside of the class, ty doesn't give any error while Python interpreter gives error

---

_Issue opened by @hyperkai on 2025-12-31 07:58_

### Summary

*Memo:
- ty check
- ty 0.0.7
- python test.py
- Python 3.14.0
- Windows 11

Reading `__v` from the outside of the class, ty doesn't give any error as shown below:

*Memo:
- A double leading underscore(__abc) can make attributes private in a class.

```python
class Cls:
    __v = 100

cls: Cls = Cls()

print(cls.__v) # No error
```

Actually, Python interpreter gives the error properly as shown below:

> AttributeError: 'Cls' object has no attribute '__v'

So, ty should give an error like Python interpreter gives as shown below:

> error: 'Cls' object has no attribute '__v'

### Version

ty 0.0.7

---

_Closed by @AlexWaygood on 2025-12-31 10:54_

---

_Comment by @AlexWaygood on 2025-12-31 10:55_

Thanks! Please see #645

---
