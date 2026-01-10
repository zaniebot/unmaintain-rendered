```yaml
number: 311
title: "Type of `global` variable is not narrowed in function scopes"
type: issue
state: closed
author: ion-elgreco
labels:
  - bug
  - narrowing
assignees: []
created_at: 2025-05-10T16:53:19Z
updated_at: 2025-07-22T23:42:12Z
url: https://github.com/astral-sh/ty/issues/311
synced_at: 2026-01-10T02:06:24Z
```

# Type of `global` variable is not narrowed in function scopes

---

_Issue opened by @ion-elgreco on 2025-05-10 16:53_

### Summary

```python
class Instance: ...

_instance: Instance | None = None

def get_instance() -> Instance:
    """Lazy static to get the instance"""
    global _instance
    if _instance is None:
        _instance = Instance()
    return _instance
```

https://play.ty.dev/f0b802a2-9e21-438d-8946-6386a45b45b5

We already checked that if it's None we assign a value to it, so the return value can't be possibly None anymore

---

_Label `bug` added by @AlexWaygood on 2025-05-10 17:00_

---

_Label `narrowing` added by @AlexWaygood on 2025-05-10 17:00_

---

_Renamed from "Return type incorrectly recognized as None" to "Type of `global` variable is not narrowed in function scopes" by @AlexWaygood on 2025-05-10 17:01_

---

_Comment by @sharkdp on 2025-05-12 10:13_

Thank you for reporting this. The problem here is that narrowing on `global`s is inherently unsafe. Consider this:
```py
x: int | None = 1

def reset_x() -> None:
    global x
    x = None

def f() -> None:
    global x

    if x is not None:
        reset_x()
        reveal_type(x)  # ?
```

Other type checkers such as mypy and pyright report `int` here, but at runtime, `x` is `None`. We might still support this eventually. See #164 for a similar discussion.

---

_Comment by @AlexWaygood on 2025-05-13 01:17_

> The problem here is that narrowing on `global`s is inherently unsafe.

This is true, and it's also possible that code from another thread could mutate the global in a concurrent program.

The unsafety here also applies to narrowing of attributes and subscript expressions, however. You already linked to #164, but just to spell it out:

```py
class Foo:
    x: int | None = 1

def reset_x(t: Foo) -> None:
    t.x = None

def f(t: Foo) -> None:
    if t.x is not None:
        reset_x(t)
        reveal_type(x)  # ?
```

But we intend to support narrowing for attributes and subscripts nonetheless, since a lot of real-world code relies on it, users strongly expect it to work, and the practical experience of existing type checkers has been that the theoretical unsoundness here rarely causes practical issues in Python code.

I think narrowing for `global` variables, as suggested in this issue, has more or less the same set of considerations as narrowing for attributes or subscripts. I can't see a strong justification for doing attribute/subscript narrowing, but not `global`-variable narrowing.

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:23_

---

_Comment by @oconnor663 on 2025-07-14 20:11_

Our behavior today seems to depend on whether the variable is free or marked `global`:

```py
x: int | None = None

def f():
    if x is not None:
        y: int = x  # allowed

def f():
    global x
    if x is not None:
        y: int = x  # error: [invalid-assignment]
```

Probably those two examples should either both pass or both fail?

---

_Comment by @carljm on 2025-07-14 20:15_

Those two examples should both pass; and the fact that the first already passes suggests that it shouldn't be too hard to make the second pass as well?

---

_Closed by @oconnor663 on 2025-07-22 23:42_

---
