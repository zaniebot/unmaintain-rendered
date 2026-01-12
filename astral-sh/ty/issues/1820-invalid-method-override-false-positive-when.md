```yaml
number: 1820
title: "`invalid-method-override` false positive when overriding method that uses `Callable` with a paramspec"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - generics
  - type properties
assignees: []
created_at: 2025-12-09T06:36:10Z
updated_at: 2025-12-15T05:34:30Z
url: https://github.com/astral-sh/ty/issues/1820
synced_at: 2026-01-12T15:54:25Z
```

# `invalid-method-override` false positive when overriding method that uses `Callable` with a paramspec

---

_@DetachHead_

### Summary

```py
from typing import override, Callable
class Foo:
    def foo[**P](self, fn: Callable[P, None]): ...


class Bar(Foo):
    @override
    def foo[**P](self, fn: Callable[P, None]): ... # error: invalid-method-override
```

https://play.ty.dev/c17aa583-3631-41b3-be6e-79a86d91ac3b

### Version

0.0.1-alpha.32

---

_Label `generics` added by @sharkdp on 2025-12-09 08:17_

---

_Label `type properties` added by @sharkdp on 2025-12-09 08:17_

---

_Label `bug` added by @AlexWaygood on 2025-12-09 13:25_

---

_Comment by @carljm on 2025-12-09 17:20_

Thanks for the report!

Verified that the issue doesn't repro with e.g.

```py
from typing import Callable

class Foo:
    def foo[T](self, fn: Callable[[T], None]): ...

class Bar(Foo):
    def foo[T](self, fn: Callable[[T], None]): ...
```

So it seems like an issue with callable-type assignability that's specific to ParamSpec. @dhruvmanila could you take a look?

---

_Assigned to @dhruvmanila by @carljm on 2025-12-09 17:20_

---

_Added to milestone `Beta` by @carljm on 2025-12-09 17:20_

---

_Comment by @dalcinl on 2025-12-10 20:48_

I'm getting the same failure with the following reproducer (conde is in `*.pyi` stub). Most likely the same issue.

```python
from concurrent.futures import Executor, Future
from typing import Callable, ParamSpec, TypeVar
from typing import override

_P = ParamSpec("_P")
_T = TypeVar("_T")

class MyExecutor(Executor):
    def submit(
        self,
        fn: Callable[_P, _T],
        /,
        *args: _P.args,
        **kwargs: _P.kwargs,
    ) -> Future[_T]: ...
```

---

_Comment by @dhruvmanila on 2025-12-11 11:28_

I'm following the trail of the assignability check:

| `self` | `target` |
|--------|--------|
| `Bar.foo` | `Foo.foo` |
| `Callable[P@foo, None]` | `Callable[P@foo, None]` |
| `P@foo.args` | `P@foo.args` |
| `tuple[object, ...]` | `P@foo.args` |

The reason for the final row is that `P.args` has an upper bound of `tuple[object, ...]` which is done by [this branch](https://github.com/astral-sh/ruff/blob/24ed28e31434106e3d0bcc8d1a1ede50b645c5da/crates/ty_python_semantic/src/types.rs#L2174-L2205). Pyright also fails at the [final assignability check](https://pyright-play.net/?strict=true&code=CYUwZgBAdg9g5gbQFRIAoF0AUAPAXBAFwFcAHAGxARgCMArEAYwIBoIA6D9VpAQwCc4AZ3yo2-IdyQBrAO7jhEUbPkBKXACgIWiPIgBeCNiA) but it might be special casing the `ParamSpec` somehow.

---

_Comment by @AlexWaygood on 2025-12-11 16:22_

If we can't figure out a principled fix in time for the beta, I'd be fine with a quick short-term hack that skips Liskov checks for methods that have ParamSpecs in their generic context. Our Liskov checks are far from comprehensive currently anyway 

---

_Comment by @carljm on 2025-12-11 17:39_

@dhruvmanila It looks to me like we need special-cased handling for ParamSpec assignability to another ParamSpec, rather than falling back to the generalized upper bound like that?

---

_Closed by @dhruvmanila on 2025-12-15 05:34_

---
