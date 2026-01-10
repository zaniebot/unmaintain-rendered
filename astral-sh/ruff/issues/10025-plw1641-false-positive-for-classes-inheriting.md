```yaml
number: 10025
title: "PLW1641: false positive for classes inheriting from builtins"
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2024-02-18T12:18:56Z
updated_at: 2024-02-18T12:31:45Z
url: https://github.com/astral-sh/ruff/issues/10025
synced_at: 2026-01-10T11:09:52Z
```

# PLW1641: false positive for classes inheriting from builtins

---

_Issue opened by @spaceone on 2024-02-18 12:18_

```python
class Foo(dict):
    def __eq__(self, other):
        return # some logic
```
reports `PLW1641 eq-without-hash` but `__hash__` is inherited from `dict`.

---

_Comment by @trag1c on 2024-02-18 12:22_

`dict` doesn't define a `__hash__` method. Also:
> A class that overrides [__eq__()](https://docs.python.org/3/reference/datamodel.html#object.__eq__) and does not define [__hash__()](https://docs.python.org/3/reference/datamodel.html#object.__hash__) will have its [__hash__()](https://docs.python.org/3/reference/datamodel.html#object.__hash__) implicitly set to None.

Source: <https://docs.python.org/3/reference/datamodel.html#object.__hash__>

---

_Comment by @spaceone on 2024-02-18 12:31_

oh yes, sorry.

---

_Closed by @spaceone on 2024-02-18 12:31_

---
