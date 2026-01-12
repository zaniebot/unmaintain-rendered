```yaml
number: 10306
title: PLE0237 false positive
type: issue
state: closed
author: henryiii
labels:
  - bug
  - linter
assignees: []
created_at: 2024-03-09T02:21:50Z
updated_at: 2024-03-11T22:48:58Z
url: https://github.com/astral-sh/ruff/issues/10306
synced_at: 2026-01-12T15:54:50Z
```

# PLE0237 false positive

---

_@henryiii_

Currently, PLE0237 will report "Attribute `other` is not defined in class's `__slots__`" on a class like this:

```python
class A:
    __slots__ = ("pub",)

    def __init__(self) -> None:
        self.other = "hi"
```

Which is correct. But it also reports the same error on the following correct class:


```python
class B:
    __slots__ = ("pub", "__dict__")

    def __init__(self) -> None:
        self.other = "hi"
```

This is incorrect; since the class has a `__dict__` in it's `__slots__`, it has a dict to put `other` in. This is described in https://docs.python.org/3/reference/datamodel.html#slots (both `__dict__` and `__weakref__` can be declared in `__slots__`), and is an important technique for wrapper classes, where the wrapped object is declared in slots, so that the `__dict__` has everything except the wrapped object.

Incorrect check result seen in https://github.com/scikit-hep/boost-histogram/pull/914.

* Command `ruff check --select=ALL --isolated tmp.py`
* Ruff version: ruff 0.3.1


---

_Label `bug` added by @charliermarsh on 2024-03-09 02:30_

---

_Comment by @charliermarsh on 2024-03-09 02:30_

Thanks!

---

_Comment by @charliermarsh on 2024-03-09 03:23_

So is it correct to effectively turn off this check when `__dict__` is present?

---

_Comment by @henryiii on 2024-03-09 05:40_

Basically. The condition is actually if a class has a `__dict__`, not if it has `__slots__`. Classes without `__slots__` (anywhere in the inheritance chain) automatically get a `__dict__`. Classes with `__slots__` (including in every parent) only get a dict if it's added manually in `__slots__`.

I assume you couldn't handle complex cases, but in the most common simple case of of a `__slots__` having a string `"__dict__"`, I assume you could treat that like a dict class?

---

_Label `linter` added by @zanieb on 2024-03-11 16:54_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-11 22:01_

---

_Closed by @charliermarsh on 2024-03-11 22:48_

---
