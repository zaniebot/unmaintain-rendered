```yaml
number: 2356
title: "Type narrowing failures involving `Never`"
type: issue
state: closed
author: lsorber
labels: []
assignees: []
created_at: 2026-01-06T08:39:41Z
updated_at: 2026-01-06T15:29:57Z
url: https://github.com/astral-sh/ty/issues/2356
synced_at: 2026-01-10T01:56:41Z
```

# Type narrowing failures involving `Never`

---

_Issue opened by @lsorber on 2026-01-06 08:39_

### Summary

I found three cases involving a generic parameter `T` that defaults to `Never` where ty fails to narrow a type. It is possible to get it to narrow the type correctly with a `case Unit() as alias:`, but not without `as alias`.

Minimal reproducible example ([ty playground](https://play.ty.dev/100eb6e4-decd-4482-96a2-6bedff6d066d)):
```python
from typing import Any, Never


class Unit[T: str = Never]:
    mapping: dict[T, Any]

    def __init__(self, key: T | None = None) -> None: ...


def success[T: str = Never](unit: Unit[T] | T) -> Unit[T]:
    match unit:
        case Unit() as unit_key:
            return unit_key  # Narrowing succeeds!
        case key if not isinstance(key, Unit):
            return Unit[T](key)  # Narrowing succeeds!
    raise AssertionError("Unreachable")


def error_match[T: str = Never](unit: Unit[T] | T) -> Unit[T]:
    match unit:
        case Unit():
            return unit  # Case 1: Narrowing fails?
        case key if not isinstance(key, Unit):
            return Unit[T](key)  # Narrowing succeeds!
    raise AssertionError("Unreachable")


def error_isinstance[T: str = Never](unit: Unit[T] | T) -> Unit[T]:
    if isinstance(unit, Unit):
        return unit  # Case 2: Narrowing fails?
    if not isinstance(unit, Unit):
        return Unit[T](unit)  # Case 3: Narrowing fails?
    raise AssertionError("Unreachable")
```

Output:
```
Return type does not match returned value: expected `Unit[T@error_match]`, found `(Unit[T@error_match] & Unit[Never]) | (T@error_match & Unit[Never])` (invalid-return-type) [Ln 22, Col 20]
Return type does not match returned value: expected `Unit[T@error_isinstance]`, found `(Unit[T@error_isinstance] & Unit[Never]) | (T@error_isinstance & Unit[Never])` (invalid-return-type) [Ln 30, Col 16]
Argument to bound method `__init__` is incorrect: Expected `T@error_isinstance | None`, found `(Unit[T@error_isinstance] & ~Unit[Never]) | (T@error_isinstance & ~Unit[Never])` (invalid-argument-type) [Ln 32, Col 24]
```

### Version

0.0.9

---

_Comment by @hoxbro on 2026-01-06 13:32_

I have a similar problem with the following:

``` python
import sys

def func() -> tuple[int, int] | None: ...

output = func()

if output is None:
    sys.exit(1)
else:
    a, _ = output  # works

b, _ = output  # fails
```

https://play.ty.dev/3a7e4154-8ba3-4fb5-a14e-7dc3512f250d

---

_Comment by @AlexWaygood on 2026-01-06 13:40_

@hoxbro your issue is covered by https://github.com/astral-sh/ty/issues/690, which at first glance this PR appears unrelated to.

---

_Comment by @AlexWaygood on 2026-01-06 14:07_

Thanks for the report @lsorber! There's a heady mix of false negatives, false positives and true positives going on here from ty. I'll take your functions one by one...

---

For the `success()` case -- unfortunately this is not a case of "narrowing succeeding" :/ It's actually a TODO from ty:

```py
def success[T: str = Never](unit: Unit[T] | T) -> Unit[T]:
    match unit:
        case Unit() as unit_key:
            reveal_type(unit_key)  # revealed: @Todo
            return unit_key
        case key if not isinstance(key, Unit):
            return Unit[T](key)
    raise AssertionError("Unreachable")
```

We don't yet infer a precise type for `as` bindings in `match` statements, unfortunately; `@Todo` is equivalent to `Any`. Because `Any` and `@Todo` are assignable to anything, this leads to no diagnostic being emitted: `Any` is assignable to the return type of `Unit[T]`.

---

For `error_match`, your `return unit` fails because ty has inferred a type of `(Unit[T] & Unit[Never]) | (T & Unit[Never])`, which it says is not assignable to the return type of `Unit[T]`.

```py
def error_match[T: str = Never](unit: Unit[T] | T) -> Unit[T]:
    match unit:
        case Unit():
            return unit  # Case 1: Narrowing fails?
        case key if not isinstance(key, Unit):
            return Unit[T](key)  # Narrowing succeeds!
    raise AssertionError("Unreachable")
```

There's definitely _a_ bug here: I think the type ty _should_ have inferred here is `Unit[T] | (T & Unit[str])`. What happens is that inside the `case Unit()` branch, ty intersects the pre-existing type of `unit` (`Unit[T] | T`) with the "top materialization" of the `Unit` type. The "top materialization" of a type is a theoretical type which all materializations of that type are subtypes of. For example, the top materialization of `Sequence[Any]` is `Sequence[object]`: whatever fully static type `T` the `Any` there materializes to, `Sequence[T]` will always be a subtype of `Sequence[object]`. In this case, the top materialization of `Unit` is `Unit[str]`, because `Unit` is generic over a type variable that has `str` as its upper bound; it's impossible to conceive of a valid materialization of `Unit` that would not be a subtype of `Unit[str]`. Unfortunately, ty gets this wrong and says that the top materialization of `Unit` is `Unit[Never]` -- it uses the TypeVar's default rather than the TypeVar's upper bound. This is tracked in https://github.com/astral-sh/ty/issues/1164, though your issue makes me think that we should bump the priority of that bug report!

If we had inferred the correct top materialization of `Unit`, then we would have inferred `(Unit[str] & Unit[T]) | (Unit[str] & T)`, which would have simplified to `Unit[T] | (T & Unit[str])`. But even if we'd inferred the correct top materialization of `Unit`, that still would have led us to emit an error on this line -- we would have inferred `Unit[T] | (T & Unit[str])`, which is not assignable to your return type of `Unit[T]`. We infer that `T & Unit[str]` is a possibility here, because `T` and `Unit[T]` are not necessarily disjoint types! It's possible to conceive of cases where, due to multiple inheritance, a type checker believes an instance of `T` is being passed in, but the instance of `T` is _also_ an instance of `Unit`, and so the first branch of your `match` statement ends up being taken:

```py
class Bar(Unit[str], str): ...

def returns_foo() -> str:
    return Bar()

reveal_type(error_match(returns_foo()))
```

If you add `@final` to your `Unit` class, however, ty is able to correctly see that `Unit` and `T` are disjoint, and the error goes away. Our revealed types here are a bit confused because of the top-materialization bug I mentioned above, but I believe/hope we still would not emit an error on this even if that bug was fixed: https://play.ty.dev/4d0657ce-7dd5-43a5-ac89-c965ebe13649

---

I think the issues with your third function are basically the same as discussed with your second function.

---

All that said... I don't think there's anything here that isn't discussed in other existing issues:
- The `@Todo` type for the `match` statement is covered by https://github.com/astral-sh/ty/issues/887, I think
- The top-materialization bug is covered by https://github.com/astral-sh/ty/issues/1164
- https://github.com/astral-sh/ty/issues/1578 discusses whether we should be less "pedantic" about narrowing in cases involving multiple inheritance

Thanks again for the report!!

---

_Closed by @AlexWaygood on 2026-01-06 14:07_

---

_Comment by @lsorber on 2026-01-06 15:29_

Thank you for the thorough analysis, @AlexWaygood!

---
