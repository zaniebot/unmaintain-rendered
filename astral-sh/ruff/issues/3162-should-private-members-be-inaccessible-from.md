---
number: 3162
title: Should private members be inaccessible from objects of the same type?
type: issue
state: closed
author: NeilGirdhar
labels: []
assignees: []
created_at: 2023-02-23T05:20:41Z
updated_at: 2023-02-23T16:53:01Z
url: https://github.com/astral-sh/ruff/issues/3162
synced_at: 2026-01-10T01:22:41Z
---

# Should private members be inaccessible from objects of the same type?

---

_Issue opened by @NeilGirdhar on 2023-02-23 05:20_

Should a class factory not be able to access the object it creates's private methods?  Should a comparison function not be able to access an object of the same type's private members?
```python
from typing import Self, Any

class X:
    _x: int

    def _f(self) -> None:
        ...

    @classmethod
    def factory(cls, x: int) -> Self:
        retval = cls()
        retval._f()  # SLF001 Private member accessed
        return retval

    def __eq__(self, other: Any) -> bool:
        if not isinstance(other, X):
            return False
        return other._x == self._x  # SLF001 Private member accessed
```

---

_Comment by @not-my-profile on 2023-02-23 06:58_

Yes these are false positives. The problem with this is that ruff isn't a static type checker. Static type checkers understand which type `cls()` returns and they support type narrowing i.e. they understand that the type of `other` is `X` after the above `isinstance` check ... ruff doesn't.

And while we could easily implement some logic to handle the exact cases of your example that logic would fail if e.g. retval was reassigned or the `isinstance` check was more complicated ... properly handling this requires proper static type checking.

---

_Comment by @NeilGirdhar on 2023-02-23 08:57_

Yes, I considered that it might be difficult.  Shall I close this issue then?

---

_Comment by @charliermarsh on 2023-02-23 16:53_

Yeah probably hard to resolve right now. Eventually, I hope!

---

_Closed by @charliermarsh on 2023-02-23 16:53_

---
