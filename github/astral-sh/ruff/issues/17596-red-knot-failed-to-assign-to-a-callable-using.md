---
number: 17596
title: "[red-knot] Failed to assign to a callable using `Unpack`"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - help wanted
  - ty
assignees: []
created_at: 2025-04-23T22:22:28Z
updated_at: 2025-04-24T16:12:00Z
url: https://github.com/astral-sh/ruff/issues/17596
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Failed to assign to a callable using `Unpack`

---

_Issue opened by @dhruvmanila on 2025-04-23 22:22_

### Summary

```py
from typing import Callable, TypeVarTuple, Unpack


def some_func() -> None: ...


T = TypeVarTuple("T")


def unpack(c: Callable[[Unpack[T]], object]):
    pass


def _():
    # red-knot: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def some_func() -> None` [lint:invalid-argument-type]
    unpack(some_func)
```

### Version

_No response_

---

_Label `bug` added by @dhruvmanila on 2025-04-23 22:22_

---

_Label `red-knot` added by @dhruvmanila on 2025-04-23 22:22_

---

_Referenced in [astral-sh/ruff#17591](../../astral-sh/ruff/pulls/17591.md) on 2025-04-23 22:24_

---

_Label `help wanted` added by @carljm on 2025-04-24 00:47_

---

_Comment by @carljm on 2025-04-24 00:48_

Tagging "help wanted", would be really useful to get some exploration of why we are not considering the one callable assignable to the other one here.

---

_Comment by @dhruvmanila on 2025-04-24 12:39_

(I'll take a quick look this morning and will reply.)

---

_Comment by @dhruvmanila on 2025-04-24 15:35_

Hmm, actually this diagnostic seems correct, reduced version of the above: https://types.ruff.rs/b8f87ca7-7a08-4bb2-aed8-7dbb1d800242

```py
from typing import Any
from knot_extensions import CallableTypeOf, static_assert, is_assignable_to

def empty() -> None: ...

def any_params(*args: Any, **kwargs: Any) -> object: ...

static_assert(is_assignable_to(CallableTypeOf[empty], CallableTypeOf[any_params]))  # false
```

Because, the `any_params` function can take 0 or more number of parameters but `empty` does not take any parameters which means we cannot assign `empty` to `any_params`.

Another way to look at it using protocols:
```py
class SelfCallable(Protocol):  # Subtype
    def __call__(self) -> None: ...


class OtherCallable(Protocol):  # Supertype
    def __call__(self, *args: Any, **kwargs: Any) -> None: ...


def _(self_callable: SelfCallable, other_callable: OtherCallable):
    a: OtherCallable = self_callable
	# This call is correct because `a` is typed as a callable that takes any parameters
    # but the above assignment will mean that it will raise an error at runtime
    a(1, 2, 3) 
```

And, in the REPL:
```pycon
>>> class SelfCallable(Protocol):  # Subtype
...     def __call__(self) -> None: ...
... 
... 
... class OtherCallable(Protocol):  # Supertype
...     def __call__(self, *args: Any, **kwargs: Any) -> None: ...
... 
... 
... def _(self_callable: SelfCallable, other_callable: OtherCallable):
...     a: OtherCallable = self_callable
...     # This call is correct but the above assignment will mean that it will raise an error
...     # at runtime
...     a(1, 2, 3) 
...     
>>> _(lambda: None, lambda *args, **kwargs: None)
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    _(lambda: None, lambda *args, **kwargs: None)
    ~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<python-input-3>", line 13, in _
    a(1, 2, 3)
    ~^^^^^^^^^
TypeError: <lambda>() takes 0 positional arguments but 3 were given
```

---

_Comment by @dhruvmanila on 2025-04-24 16:02_

I'm not sure why is Pyright and mypy not raising an error here but it does when this happens in an assignment statement:

* [Pyright](https://pyright-play.net/?strict=true&enableExperimentalFeatures=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDCAhgDYlEBGJApgDRQAqi1AakSAwK4I30CqKBEQDGAawBQkgCbVgUAM6RqAfWCcUwgBQBKKAFoAfFAByYFNQBcUAHS3J4hlAC8jZmw7camgEQNv2%2Bxk5dSExTWErYjJKGgBtWIFQ0ViGAF1U%2BjAKACtqYRhU7QtxKFKoIXl5QNkoZR1isqgAYigQaik9URQwGCsAQRA0TghqFHwYMFgACyR5KDUNGCQzTDnUYVw2-KsAUQAPBDyYdqgAA00AKnY0eSsAAQYwKTBNCeeFJDQUIhhONqgroN5Np6BcLqIAO7XW5QB5PF5vSbyT7fX7-MGQ6HaXSGKBZXL5U70YBgdRSM5BBRKVTqLQ4oymcynKCxEioXqoABupCQHWuw1GMD08EOqRKZRCIlEmkUIxpGgC4j2kVI5Co1HiiSlKXSmRyR1SzipcoWwnEQA)
* [mypy](https://mypy-play.net/?mypy=master&python=3.13&gist=5b2bfcc2bf76d82460aec6172658abe3&flags=show-traceback)

---

_Comment by @carljm on 2025-04-24 16:11_

Ah right! When looking at this initially, I failed to consider the difference between a `*args: Any, **kwargs: Any` signature and a `...` signature. The former is dynamic only with respect to argument types, not with respect to which parameters are accepted. The latter is dynamic with respect to both. And we seem to [handle this correctly](https://types.ruff.rs/ce5f8391-0937-40e1-a0ff-ca93aaf766a5).

---

_Closed by @carljm on 2025-04-24 16:11_

---
