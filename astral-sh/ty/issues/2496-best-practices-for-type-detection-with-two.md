```yaml
number: 2496
title: Best practices for type detection with two variable logic
type: issue
state: open
author: nadav7679
labels:
  - question
assignees: []
created_at: 2026-01-14T17:03:09Z
updated_at: 2026-01-14T17:17:15Z
url: https://github.com/astral-sh/ty/issues/2496
synced_at: 2026-01-14T17:37:34Z
```

# Best practices for type detection with two variable logic

---

_@nadav7679_

### Question

Hi ty team, loving your work.

I encountered quite a simple piece of logic that I can't seem to restructure in a way that passes ty check. I was wondering what is the best practice to handle it.  

The use case is something like this (also view it in this [playground](https://play.ty.dev/a94c53c4-a77d-49c7-8743-2b5e33669991)):

```python3
from typing import reveal_type

class A:
    pass

class B:
    pass

def f(
        a: A | None,
        b: B | None,
) -> A | B | tuple[A, B]:

    if a is None and b is None:
        raise

    if a is None:
        reveal_type(b) # B | None
        return b
    
    if b is None:
        return a

    return (a, b)
```

Basically, the function `f` is allowed to be triggered with either `a`, `b`, or both, but is not allowed to be triggered with two `None`s. `ty` can't recognise this logic so it treats `b` as type `B | None` and displays this error when trying to return B:

```
Return type does not match returned value: expected `A | B | tuple[A, B]`, found `B | None` (invalid-return-type) [Ln 18, Col 16]
```

I tried playing around with it and couldn't find a good solution. I was wondering what's the best practice in these kinds of cases.

Thanks!

### Version

_No response_

---

_Label `question` added by @nadav7679 on 2026-01-14 17:03_

---

_Renamed from "Implementing two variable type detection" to "Best practices for type detection with two variable logic" by @nadav7679 on 2026-01-14 17:04_

---

_Comment by @AlexWaygood on 2026-01-14 17:11_

It's maybe a bit weird, but we do support narrowing unions of tuples via subscript checks, and we also support narrowing of individual subscript elements, so you _could_ do something like this:

```py
from typing import reveal_type

class A:
    value: int

class B:
    value: int

def f(ab: tuple[A | None, B] | tuple[A, B | None]) -> A | B | tuple[A, B]:

    if ab[0] is None and ab[1] is None:
        raise

    if ab[0] is None:
        # `ab` is narrowed to `tuple[A | None, B]` here
        reveal_type(ab[1])  # revealed: B
        return ab[1]
    
    if ab[1] is None:
        # `ab` is narrowed to `tuple[A, B | None]` here
        return ab[0]  # revealed: A

    # at this point, `a[0]` has been narrowed to `A`
    # and `a[1]` has been narrowed to `B`,
    # (although we don't *currently* narrow the tuple as a whole
    # to `tuple[A, B]` at this location)
    return ab[0], ab[1]
```

---

_Comment by @AlexWaygood on 2026-01-14 17:11_

(also, thanks for the kind words!)

---

_Comment by @AlexWaygood on 2026-01-14 17:17_

The more usual thing to do is probably not to wrap your variables in a tuple, but just use an `assert` to help the type checker out:

```py
from typing import reveal_type

class A:
    pass

class B:
    pass

def f(
    a: A | None,
    b: B | None,
) -> A | B | tuple[A, B]:

    if a is None and b is None:
        raise

    if a is None:
        assert b is not None
        reveal_type(b) # B
        return b
    
    if b is None:
        return a

    return (a, b)
```

---
