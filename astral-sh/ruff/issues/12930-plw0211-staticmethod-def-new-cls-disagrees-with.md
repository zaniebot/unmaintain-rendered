```yaml
number: 12930
title: "PLW0211: `@staticmethod ; def __new__(cls, ...)` disagrees with the Python docs"
type: issue
state: closed
author: cclauss
labels:
  - bug
  - accepted
assignees: []
created_at: 2024-08-16T13:45:54Z
updated_at: 2024-08-18T15:30:24Z
url: https://github.com/astral-sh/ruff/issues/12930
synced_at: 2026-01-12T15:54:52Z
```

# PLW0211: `@staticmethod ; def __new__(cls, ...)` disagrees with the Python docs

---

_@cclauss_

```
ruff check --select=PLW0211 - <<EOF
class TestNew:
    @staticmethod
    def __new__(cls, title, *args, **kwargs):
        print(cls)
EOF
```
> -:3:17: PLW0211 First argument of a static method should not be named `cls`

This contradicts https://docs.python.org/3/reference/datamodel.html#basic-customization which uses `cls` and states that `__new__()` is a static method.

PEP20 statement `Explicit is better than implicit.` suggests that `@staticmethod` should be allowed in this code.



---

_Comment by @AlexWaygood on 2024-08-16 14:06_

I think that's a somewhat extreme reading of PEP20! But yes, we should make an exception for `__new__` here, which is obviously very special

---

_Label `bug` added by @AlexWaygood on 2024-08-16 14:06_

---

_Label `accepted` added by @AlexWaygood on 2024-08-16 14:06_

---

_Closed by @AlexWaygood on 2024-08-18 15:30_

---
