---
number: 3403
title: "Feature request: initialize super class with super call"
type: issue
state: open
author: NeilGirdhar
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-03-08T14:35:12Z
updated_at: 2025-04-10T02:50:31Z
url: https://github.com/astral-sh/ruff/issues/3403
synced_at: 2026-01-10T01:22:42Z
---

# Feature request: initialize super class with super call

---

_Issue opened by @NeilGirdhar on 2023-03-08 14:35_

A common error with Python inheritance is to try to explicitly call base class methods:
```python
from typing import Any

class C:
    def __init__(self, x: int, **kwargs):
        super().__init__(**kwargs)
        self.x = x


class D(C):
    def __init__(self, **kwargs: Any) -> None:
        C.__init__(self, 1, **kwargs)  # Bad!
        # super().__init__(x=1, **kwargs)  # Good.


class E(C):
    def __init__(self, **kwargs: Any) -> None:
        super().__init__(**kwargs)
        self.y = 2


class F(D, E):
    pass


f = F()
print(f.x, f.y)  # AttributeError: 'F' object has no attribute 'y'
```
The problem with `D`'s definition is that `E` and `F` can come along and now `E`'s initializer is not called.

This is a problem with all methods (not just `__init__`) although `__init__` is the most common place this fails.

In general, I think it should be a Ruff error to call a base class method explicitly rather than through `super`.  Here's an example of [this error](https://github.com/pandas-dev/pandas/blob/a07cb65459f24446e82354854ff6658f29414c0a/pandas/core/frame.py#L852).

The same problem would manifest with `__enter__`, `__exit__`, or any user-defined method that exhibits cooperative inheritance.

---

_Label `rule` added by @charliermarsh on 2023-03-08 17:34_

---

_Comment by @charliermarsh on 2023-03-08 17:35_

We have UP008, which handles the _arguments_ portion to `__super__`, but I guess this is different in that we don't catch the `C` in `C.__init__`. Is that right?

---

_Comment by @NeilGirdhar on 2023-03-08 17:47_

Yeah, seems similar.  The problem here is that you should rarely (if ever) call your superclass by its name: `C.__init__(...`.  You should instead call it like this `super().__init__(...`

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:33_

---

_Comment by @Avasam on 2025-04-10 02:09_

I've been upgrading pywin32's legacy code over the past year. I would love an autofix (even if marked unsafe) for the hundreds base class `__init__` calls here ^^

![Image](https://github.com/user-attachments/assets/e0ae8686-06b7-43c3-8f68-a42361dd035f)

---

_Comment by @NeilGirdhar on 2025-04-10 02:13_

@Avasam Wow, that looks painful.  Too bad Ruff hadn't warned that developer before they did that!

---

_Comment by @Avasam on 2025-04-10 02:21_

> Too bad Ruff hadn't warned that developer before they did that!

What do you mean? Ruff hasn't done anything. I'm +1-ing the feature request to catch this (to prevent regressions) with a real-world example and asking for an autofix at the same time.

---

_Comment by @NeilGirdhar on 2025-04-10 02:50_

@Avasam Yes, I understand (Thank you).   I just mean too bad Ruff didn't have this feature before someone made that mess for you to clean up 😄 

---
