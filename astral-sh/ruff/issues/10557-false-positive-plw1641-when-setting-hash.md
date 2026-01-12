```yaml
number: 10557
title: "False-positive `PLW1641` when setting `__hash__ = <ParentClass>.__hash__`"
type: issue
state: closed
author: janosh
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2024-03-25T05:55:11Z
updated_at: 2024-03-29T20:26:15Z
url: https://github.com/astral-sh/ruff/issues/10557
synced_at: 2026-01-12T15:54:50Z
```

# False-positive `PLW1641` when setting `__hash__ = <ParentClass>.__hash__`

---

_@janosh_

with v0.3.4, `ruff check t.py --select PLW1641` on

```python
class A:
    def __eq__(self, other):
        return False

    def __hash__(self):
        return 7

class B(A):
    def __eq__(self, other):
        return True

    __hash__ = A.__hash__
```

issues

```py
PLW1641 Object does not implement `__hash__` method
   |
12 | class B(A):
   |       ^ PLW1641
13 |     def __eq__(self, other):
14 |         return True
```

even though [the docs explicitly recommend this](https://docs.python.org/3/reference/datamodel.html#object.__hash__):

> If a class that overrides [__eq__()](https://docs.python.org/3/reference/datamodel.html#object.__eq__) needs to retain the implementation of [__hash__()](https://docs.python.org/3/reference/datamodel.html#object.__hash__) from a parent class, the interpreter must be told this explicitly by setting `__hash__ = <ParentClass>.__hash__`.



---

_Label `bug` added by @MichaReiser on 2024-03-25 08:26_

---

_Label `rule` added by @MichaReiser on 2024-03-25 08:26_

---

_Label `help wanted` added by @MichaReiser on 2024-03-25 08:26_

---

_Comment by @charliermarsh on 2024-03-29 20:26_

This is now fixed.

---

_Closed by @charliermarsh on 2024-03-29 20:26_

---
