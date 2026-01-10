---
number: 2364
title: Typing for return value of type(name, bases, dict) could be improved
type: issue
state: closed
author: HenriBlacksmith
labels: []
assignees: []
created_at: 2026-01-06T12:25:13Z
updated_at: 2026-01-06T12:38:17Z
url: https://github.com/astral-sh/ty/issues/2364
synced_at: 2026-01-10T01:51:14Z
---

# Typing for return value of type(name, bases, dict) could be improved

---

_Issue opened by @HenriBlacksmith on 2026-01-06 12:25_

### Summary

Here is a playground with my MRE: https://play.ty.dev/1de4a48c-86e6-4063-ae8b-17cfa15f7c0a

```python
class A:
    pass

class B(A):
    pass

class C(A):
    pass

def foo(class_name: str) -> type[A]:
    if class_name == "B":
        return type("B", (A,), {})
    if class_name == "C":
        return type("C", (A,), {})
    raise Exception("Unknown class name")
````

I might be wrong but ideally this piece of code should work (at least `mypy` allows it), in that case the type can be narrowed to `type[A]` instead of just `type` given A is among the base classes.

### Version

ty 0.0.9 (f1652f05d 2026-01-05)

---

_Comment by @AlexWaygood on 2026-01-06 12:38_

Thanks!

---

_Closed by @AlexWaygood on 2026-01-06 12:38_

---
