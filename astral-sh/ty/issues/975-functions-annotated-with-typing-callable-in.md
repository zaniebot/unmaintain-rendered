```yaml
number: 975
title: "Functions annotated with `typing.Callable` in parameters have no `__name__`"
type: issue
state: closed
author: lmmx
labels: []
assignees: []
created_at: 2025-08-12T17:08:49Z
updated_at: 2025-08-12T18:02:34Z
url: https://github.com/astral-sh/ty/issues/975
synced_at: 2026-01-12T15:54:24Z
```

# Functions annotated with `typing.Callable` in parameters have no `__name__`

---

_@lmmx_

### Summary

I've tried this with `typing.Callable`, `collections.abc.Callable`, and `typing_extensions.Callable` and all give the same error.

```python
error[unresolved-attribute]: Type `(...) -> R` has no attribute `__name__`
   --> path/to/file.py:104:49
    |
102 |             def wrapper(*args, **kwargs):
103 |                 # Generate cache key
104 |                 cache_key = self._get_cache_key(func.__name__, args, kwargs)
    |                                                 ^^^^^^^^^^^^^
105 |
106 |                 # Check if result is cached
    |
info: rule `unresolved-attribute` is enabled by default
```

I made a repro ([playground link](https://play.ty.dev/e330fea2-2856-47b4-9fb6-21f36768abb6)) and this appears to be only in function params?

```python
from typing import Callable

def foo(arg1: Callable[..., str]) -> str:
    name_of_foo_arg: str = arg1.__name__
    return name_of_foo_arg

foo_itself: Callable[Callable[..., str], str] = foo
name_of_foo_itself: str = foo_itself.__name__
```
⇣
```
Type `(...) -> str` has no attribute `__name__` (unresolved-attribute) [Ln 4, Col 28]
```

- `foo_itself` which has been assigned to the funcdef `foo` is allowed to have a `__name__`
- `arg1` which is a function passed into `foo` as a parameter is not allowed to have a `__name__`

This program would work:

```python
>>> def foo(arg1: Callable[..., str]) -> str:
...     name_of_arg: str = arg1.__name__
...     return name_of_arg
... 
>>> foo(bar)
'bar'
```

Note that a lambda still has a `__name__`

```python
>>> lam = lambda x: "x"
>>> lam.__name__
'<lambda>'
```

### Version

ty 0.0.1-alpha.12

---

_Renamed from "Functions annotated with `typing.Callable`" to "Functions annotated with `typing.Callable` in parameters have no `__name__`" by @lmmx on 2025-08-12 17:09_

---

_Comment by @AlexWaygood on 2025-08-12 17:17_

Yes, this is an area where ty currently does something differently to other type checkers such as mypy and pyright. It's easy to demonstrate that the behaviour of mypy and pyright is unsound, however -- for example:

```py
from typing import Callable

def foo(arg1: Callable[..., str]) -> str:
    name_of_foo_arg: str = arg1.__name__
    return name_of_foo_arg
    
class CustomCallable:
    def __call__(self) -> str:
        return "foo"

# mypy and pyright both accept this, but it raises an exception at runtime!
foo(CustomCallable())
```

The question is: what is a `Callable` type meant to represent?
1. Is it meant to represent "a type that is both callable with a certain signature, _and_ has all the attributes that you usually find on functions"? The fact that mypy and pyright allow you to access a `__name__` attribute on `Callable` types would imply that this is their interpretation -- but they're very inconsistent about this, allowing you to pass in types such as `CustomCallable` above that do not have `__name__` attributes available.
2. Or is it meant to represent "any callable type with a certain signature, but that does not necessarily have any other attributes""? This is how ty currently interprets `Callable` types.

---

_Comment by @AlexWaygood on 2025-08-12 17:18_

There's related discussion in https://github.com/astral-sh/ty/issues/599, so I'll fold this issue into that one

---

_Closed by @AlexWaygood on 2025-08-12 17:18_

---

_Comment by @lmmx on 2025-08-12 17:56_

Thanks Alex, sorry for the duplicate! Oof that's a mouthful, but fair enough!

In my case I am using "Callable" because there is no `typing.Function`, but since this "typing-extensions" library seems to have a `FunctionType` and you can bundle it with a callable that works for me!

It does indeed work this way

```python
from types import FunctionType
from typing import Callable

from ty_extensions import Intersection

CallableFn = Intersection[FunctionType, Callable[[], None]]

        def decorator(
            func: CallableFn[..., R],
        ) -> CallableFn[..., R]:
            @functools.wraps(func)
            def wrapper(*args, **kwargs):
                cache_key = self._get_cache_key(func.__name__, args, kwargs)
```

---

_Comment by @AlexWaygood on 2025-08-12 18:02_

> In my case I am using "Callable" because there is no `typing.Function`

to be clear: `types.FunctionType` is in the CPython standard library! And it is the type that all functions are instances of at runtime:

```py
>>> from types import FunctionType
>>> def f(): ...
... 
>>> type(f) is FunctionType
True
>>> type((lambda: 42)) is FunctionType
True
```

It's just `ty_extensions.Intersection` that is currently not yet standardised. If you want to use it in your own code, for now you'll need to import it in an `if TYPE_CHECKING` block, since it doesn't exist at runtime.

---
