```yaml
number: 7479
title: "False-positive `N802` with `overload` and `override`"
type: issue
state: closed
author: bersbersbers
labels:
  - good first issue
  - rule
  - accepted
assignees: []
created_at: 2023-09-18T08:22:39Z
updated_at: 2023-09-18T18:32:41Z
url: https://github.com/astral-sh/ruff/issues/7479
synced_at: 2026-01-10T11:09:49Z
```

# False-positive `N802` with `overload` and `override`

---

_Issue opened by @bersbersbers on 2023-09-18 08:22_

* A minimal code snippet that reproduces the bug.

```python
from typing import overload

from typing_extensions import override


class CannotModify:
    @overload
    def BadName(self, var: int) -> int:  # noqa: N802
        ...

    @overload
    def BadName(self, var: str) -> str:  # noqa: N802
        ...

    def BadName(self, var: int | str) -> int | str:  # noqa: N802
        return var


class MyClass(CannotModify):
    @overload
    def BadName(self, var: int) -> int:
        ...

    @overload
    def BadName(self, var: str) -> str:
        ...

    @override
    def BadName(self, var: int | str) -> int | str:
        return var * 2

```

* The command you invoked
```
ruff --isolated --select N802 bug.py
```

gives 

```
bug.py:21:9: N802 Function name `BadName` should be lowercase
bug.py:25:9: N802 Function name `BadName` should be lowercase
Found 2 errors.
```

I feel the `@override` before the implementation of `MyClass.BadName` should extend to the overloads. Or should I add one `@override` per `@overload`?

* The current Ruff version (`ruff --version`).

0.0.290

---

_Comment by @charliermarsh on 2023-09-18 13:18_

I suppose we could just omit any methods tagged with `@overload` since presumedly we will end up flagging the implementation itself?

---

_Label `rule` added by @charliermarsh on 2023-09-18 16:32_

---

_Label `needs-decision` added by @charliermarsh on 2023-09-18 16:32_

---

_Comment by @charliermarsh on 2023-09-18 17:48_

Yeah, let's just omit `@overload` methods here I think.

---

_Label `needs-decision` removed by @charliermarsh on 2023-09-18 17:48_

---

_Label `good first issue` added by @charliermarsh on 2023-09-18 17:48_

---

_Label `accepted` added by @charliermarsh on 2023-09-18 17:48_

---

_Closed by @charliermarsh on 2023-09-18 18:32_

---
