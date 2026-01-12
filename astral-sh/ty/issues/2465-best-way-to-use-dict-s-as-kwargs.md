```yaml
number: 2465
title: "Best way to use `dict`s as `kwargs`?"
type: issue
state: closed
author: galah92
labels:
  - question
assignees: []
created_at: 2026-01-12T13:27:08Z
updated_at: 2026-01-12T14:13:05Z
url: https://github.com/astral-sh/ty/issues/2465
synced_at: 2026-01-12T15:03:21Z
```

# Best way to use `dict`s as `kwargs`?

---

_Issue opened by @galah92 on 2026-01-12 13:27_

### Question

```python
def foo(a: str, b: int) -> None:
    pass


kw = {"a": "hello", "b": 42}
foo(**kw)
```

This yields:
> Argument to function `foo` is incorrect: Expected `int`, found `Unknown | str | init`

What's the best way to type `kw` to make this work? I'm looking for something simpler than defining a `TypeDict`, specifically.

Motivation: I need to call multiple functions that support the same subset of kwargs with similar values.

### Version

ty 0.0.11 (830cb9cc6 2026-01-09)

---

_Label `question` added by @galah92 on 2026-01-12 13:27_

---

_Comment by @AlexWaygood on 2026-01-12 13:43_

The solution that works today is to do

```py
from typing import Any, cast

def foo(a: str, b: int) -> None:
    pass

kw = cast(dict[str, Any], {"a": "hello", "b": 42})
foo(**kw)
```

We intend to soon change our behaviour so that this will also work:

```py
from typing import Any

def foo(a: str, b: int) -> None:
    pass

kw: dict[str, Any] = {"a": "hello", "b": 42}
foo(**kw)
```

And we have https://github.com/astral-sh/ty/issues/1248 open to track not emitting any error at all on your original code (which I think would be good)

---

_Closed by @AlexWaygood on 2026-01-12 13:43_

---

_Comment by @galah92 on 2026-01-12 14:12_

Thank you. Note that `kw: dict[str, Any] = {"a": "hello", "b": 42}` seems to work for me already, no need for `cast`.

---

_Comment by @AlexWaygood on 2026-01-12 14:13_

Ah right, yeah, I forgot that we implemented that for invariant generics already 😆

---
