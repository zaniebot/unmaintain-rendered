---
number: 14883
title: "(📚) docs for `PLW1641` `known problems` doesn't make sense"
type: issue
state: closed
author: KotlinIsland
labels:
  - documentation
assignees: []
created_at: 2024-12-09T23:34:48Z
updated_at: 2024-12-20T08:33:17Z
url: https://github.com/astral-sh/ruff/issues/14883
synced_at: 2026-01-07T13:12:16-06:00
---

# (📚) docs for `PLW1641` `known problems` doesn't make sense

---

_Issue opened by @KotlinIsland on 2024-12-09 23:34_

```
# Known problems
Does not check for `__hash__` implementations in superclasses.
```

`__hash__` defined in a superclass doesn't affect the semantics:
```py
class A:
    def __hash__(self):
        return 1

class B(A):
   def __eq__(self, other):
       return False
B.__hash__ # None
```

---

_Comment by @zanieb on 2024-12-09 23:38_

The rule is

> Checks for classes that implement `__eq__` but not `__hash__`.


So.. if you have this:

```python
class A:
    def __hash__(self):
        return 1

class B(A):
   def __eq__(self, other):
       return False
```

B implements `__hash__` via A but is not detected by Ruff.

What doesn't make sense about this?


---

_Comment by @KotlinIsland on 2024-12-10 00:18_

> What doesn't make sense about this?

as in my OP:
```py
print(B.__hash__) # None
```

---

_Referenced in [astral-sh/ruff#6932](../../astral-sh/ruff/issues/6932.md) on 2024-12-10 00:26_

---

_Comment by @KotlinIsland on 2024-12-10 00:27_

> A class that overrides [**eq**()](https://docs.python.org/3/reference/datamodel.html#object.__eq__) and does not define [**hash**()](https://docs.python.org/3/reference/datamodel.html#object.__hash__) will have its [**hash**()](https://docs.python.org/3/reference/datamodel.html#object.__hash__) implicitly set to None.

source: [https://docs.python.org/3/reference/datamodel.html#object.\_\_hash__](https://docs.python.org/3/reference/datamodel.html#object.__hash__)

---

_Comment by @charliermarsh on 2024-12-10 00:28_

Wow, so it's _just_ when `__eq__` is defined?

---

_Comment by @zanieb on 2024-12-10 00:41_

@KotlinIsland sorry your OP doesn't have inheritance

Interesting find though!

---

_Label `documentation` added by @zanieb on 2024-12-10 00:41_

---

_Comment by @KotlinIsland on 2024-12-10 01:01_

> sorry your OP doesn't have inheritance

fail...

i've updated it

---

_Referenced in [astral-sh/ruff#14885](../../astral-sh/ruff/pulls/14885.md) on 2024-12-10 01:14_

---

_Closed by @dhruvmanila on 2024-12-20 08:33_

---

_Closed by @dhruvmanila on 2024-12-20 08:33_

---
