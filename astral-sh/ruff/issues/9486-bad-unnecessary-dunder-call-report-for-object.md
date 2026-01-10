---
number: 9486
title: "(🐞) bad `unnecessary-dunder-call` report for `object.__getattribute__(self` within a `__getattribute__` implementation"
type: issue
state: closed
author: KotlinIsland
labels:
  - bug
assignees: []
created_at: 2024-01-12T04:49:13Z
updated_at: 2024-01-12T19:48:44Z
url: https://github.com/astral-sh/ruff/issues/9486
synced_at: 2026-01-10T01:22:49Z
---

# (🐞) bad `unnecessary-dunder-call` report for `object.__getattribute__(self` within a `__getattribute__` implementation

---

_Issue opened by @KotlinIsland on 2024-01-12 04:49_

```py
class A:
    a = 1

    def __getattribute__(self, item):
        return object.__getattribute__(self, item)  # PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..

A().a
```
Ah yes, better use `getattr` built-in function..

```py
class A:
    a = 1

    def __getattribute__(self, item):
        return getattr(self, item)

A().a
#   File "C:\Users\charlie_marsh(parody)\rust_based_python_typechecker\__init__.py", line 5, in __getattribute__
#    return getattr(self, item)
#           ^^^^^^^^^^^^^^^^^^^
#  [Previous line repeated 745 more times]
#RecursionError: maximum recursion depth exceeded
```

---

_Renamed from "(🐞) bad `unnecessary-dunder-call` report for `object.__getattribute__(self` with a `__getattribute__` implementation" to "(🐞) bad `unnecessary-dunder-call` report for `object.__getattribute__(self` within a `__getattribute__` implementation" by @KotlinIsland on 2024-01-12 04:49_

---

_Label `bug` added by @charliermarsh on 2024-01-12 04:56_

---

_Comment by @charliermarsh on 2024-01-12 05:00_

What is going on with that filename lol

---

_Comment by @randolf-scholz on 2024-01-12 17:43_

Other example of a false-positive:

```python
class A:
    def __repr__(self):
        return "Prints this text"

a = A()
object.__repr__(a)  # PLC2801
```

Generally, for `T.__dunder__(obj, *args, **kwargs)` to be redundant (assuming `isinstance(T, type)`), I think we necessarily need `type(obj).__dunder__ is T.__dunder__` to be true?


---

_Comment by @charliermarsh on 2024-01-12 17:46_

Need to understand how pylint handles this.

---

_Comment by @charliermarsh on 2024-01-12 17:47_

So for one thing, they omit dunder calls _within_ dunder methods.

---

_Comment by @charliermarsh on 2024-01-12 17:49_

It looks like they also skip dunder calls on non-instantiated classes? Which includes `object`?

---

_Comment by @bastimeyer on 2024-01-12 19:10_

> So for one thing, they omit dunder calls _within_ dunder methods.

This is the main cause of `PLC2801` being raised in our project after updating ruff to `0.1.13`.

Another false positive report (IMO) after upgrading to the latest version is the explicit call of dunder methods in test cases. Imagine a `__getitem__` method which gets tested for a specific exception in a `with pytest.raises(...)` block. Ruff now wants me to use the subscript notation (`foo[bar]` instead of `foo.__getitem__(bar)`), which is then reported as a "statement without an effect" by other linters (like the one built into pycharm for example). Calling the `__getitem__` method explicitly should be ok in test cases and shouldn't require an annotation for suppressing `PLC2801`.

---

_Comment by @charliermarsh on 2024-01-12 19:13_

Yeah we can easily fix that part. The second part is harder and will require us to greatly limit the scope of the rule.

---

_Comment by @charliermarsh on 2024-01-12 19:16_

(I'll fix the first part now.)

---

_Referenced in [astral-sh/ruff#9496](../../astral-sh/ruff/pulls/9496.md) on 2024-01-12 19:32_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-12 19:32_

---

_Closed by @charliermarsh on 2024-01-12 19:48_

---
