```yaml
number: 2379
title: Support for decorator that transforms a class into an instance of a wrapper type
type: issue
state: closed
author: mwaskom
labels: []
assignees: []
created_at: 2026-01-07T16:28:59Z
updated_at: 2026-01-07T16:56:12Z
url: https://github.com/astral-sh/ty/issues/2379
synced_at: 2026-01-10T01:56:41Z
```

# Support for decorator that transforms a class into an instance of a wrapper type

---

_Issue opened by @mwaskom on 2026-01-07 16:28_

### Summary

This is perhaps a bit weird, but I'd like to define a decorator that can be applied to a class, producing an instance of a wrapper type with an `.invoke()` method that proxies to the `__call__` method of the underlying class.

```python
@wrap()
class Greeter:
    def __call__(self, name: str) -> str:
        return f"Hello, {name}!"

result: str = Greeter.invoke("World")  # Greeter is an instance of a `Wrapper` type
print(result)  # Should print: Hello, World!
```

I have defined my `wrap` and `Wrapper` as

```python
from typing import Any, Callable, Generic, ParamSpec, Protocol, TypeVar

P = ParamSpec("P")
R = TypeVar("R", covariant=True)


class _HasCall(Protocol[P, R]):
    """Protocol for classes with a __call__ method."""

    def __call__(self, *args: P.args, **kwargs: P.kwargs) -> R: ...


class Wrapper(Generic[P, R]):
    """
    Wrapper class that provides an invoke() method.

    Generic over P (the __call__ parameters) and R (the return type).
    """

    def __init__(self, instance: Any) -> None:
        self._instance = instance

    def invoke(self, *args: P.args, **kwargs: P.kwargs) -> R:
        """Dispatch to the wrapped instance's __call__ method."""
        return self._instance(*args, **kwargs)


def wrap() -> Callable[[type[_HasCall[P, R]]], Wrapper[P, R]]:
    """
    Decorator that wraps a class in a Wrapper instance.

    The decorator:
    1. Takes a class with __call__[P, R]
    2. Creates an instance of that class
    3. Returns a Wrapper[P, R] wrapping that instance

    The return type is explicitly annotated as Wrapper[P, R].
    """

    def decorator(cls: type[_HasCall[P, R]]) -> Wrapper[P, R]:
        instance = cls()
        return Wrapper(instance)

    return decorator
```

This script runs as expected. But it fails type checking with `ty`, which does not perceive the type transformation applied by the decorator:

```
error[unresolved-attribute]: Class `Greeter` has no attribute `invoke`
  --> minimal_reproducer.py:77:15
   |
75 |         return f"Hello, {name}!"
76 |
77 | result: str = Greeter.invoke("World")
   |               ^^^^^^^^^^^^^^
78 | print(result)  # Should print: Hello, World!
   |
info: rule `unresolved-attribute` is enabled by default
```

It is somewhat unclear to me if this is expected to work. `mypy` type checking fails with the same error, but `pyright` checking passes as desired.

I'm curious if this is something I should expect `ty` to be able to support. I searched open issues and found a number relating to decorator support in general, but none that appeared specifically about this (admittedly unusual) pattern.

Thank you!

### Version

ty 0.0.9

---

_Comment by @AlexWaygood on 2026-01-07 16:37_

Thanks! We do plan to support this -- see #143

---

_Closed by @AlexWaygood on 2026-01-07 16:37_

---

_Comment by @mwaskom on 2026-01-07 16:40_

Ah apologies for missing that one. Thanks!

---

_Comment by @mwaskom on 2026-01-07 16:41_

Since that issue is pretty old I wonder if you could comment on current thoughts about prioritization? Is this something you'd see as blocking a "stable" / "GA" release of `ty` or more of a nice to have eventually?

---

_Comment by @AlexWaygood on 2026-01-07 16:41_

np, finding dupes on GitHub is impossible, especially on jargon-y projects like type checkers :-)

---

_Comment by @AlexWaygood on 2026-01-07 16:45_

> Since that issue is pretty old I wonder if you could comment on current thoughts about prioritization? Is this something you'd see as blocking a "stable" / "GA" release of `ty` or more of a nice to have eventually?

It's in the "Stable" milestone, so we certainly hope to fix it before we declare that we're stable.

I would _personally_ view it as pretty low priority relative to many other issues in the "stable" milestone. As you noted, some other type checkers such as mypy that have been stable for a long time have never added support for this pattern, and (I used to contribute to mypy a fair bit) I don't honestly remember it coming up _much_ on the mypy issue tracker. We've just got a _lot_ of issues to fix before we declare ourselves stable 😅

---

_Comment by @mwaskom on 2026-01-07 16:49_

Understandable thanks! FWIW this is potentially quite important to us at [Modal](https://modal.com/), as we rely heavily on fairly unusual wrapping patterns in our Python SDK. And we'd of course love to be compatible with `ty` as I know our users will want to leverage it.

---

_Comment by @AlexWaygood on 2026-01-07 16:53_

Ah that's definitely valuable information, and could definitely change our prioritisation here! Would you mind posting a comment to that effect on the other issue?

---

_Comment by @mwaskom on 2026-01-07 16:56_

Sure thing. Thanks for explaining, appreciate your time!

---
