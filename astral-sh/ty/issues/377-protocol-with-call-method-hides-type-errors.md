```yaml
number: 377
title: Protocol with __call__ method hides type errors
type: issue
state: closed
author: ojii
labels:
  - type properties
  - Protocols
assignees: []
created_at: 2025-05-14T08:40:44Z
updated_at: 2025-09-12T21:20:11Z
url: https://github.com/astral-sh/ty/issues/377
synced_at: 2026-01-12T15:54:23Z
```

# Protocol with __call__ method hides type errors

---

_@ojii_

### Summary

`Callable` is very restricted in what it can express (no kwargs, no optional arguments, etc) and the official Python typing documentation instructs you to use a `Protocol` subclass with a `__call__` method instead. However with `ty` this does not work as expected.

Given the following code:

```python
from typing import Protocol


class MyCallable(Protocol):
    def __call__(self, arg: int) -> int: ...


def func(mc: MyCallable) -> int:
    return mc(1)


# expect to type check
func(lambda n: 1)

# expect to fail check
func(lambda: None)


def x(arg: int) -> int:
    return 1


# expect to type check
func(x)


def y(arg: str) -> str:
    return "a"


# expect to fail check
func(y)


# expect to type check, but fails with call-non-callable on F
def func2[F: MyCallable](f: F) -> int:
    return f(1)
```

Running `ty check file.py` everything passes but the check on `func2`, it is happy with all the calls to `func` even though half the arguments passed in should fail the type check.

### Version

ty 0.0.1-alpha.1 (12f466e46 2025-05-13)

---

_Label `type properties` added by @sharkdp on 2025-05-14 08:53_

---

_Comment by @AlexWaygood on 2025-05-14 10:40_

Thanks for the detailed writeup. We currently have only very basic support for protocols, and the naive approach we currently take leads to lots of false negatives, as you say! We'll be working on improving this over the next few weeks.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-14 10:40_

---

_Label `Protocols` added by @AlexWaygood on 2025-05-14 11:02_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:32_

---

_Comment by @AlexWaygood on 2025-09-12 10:23_

> ```py
> # expect to type check
> func(lambda n: 1)
> ```

I would not expect this to type-check because your protocol states that you must be able to pass a keyword argument `arg` when calling an object for that object to satisfy your protocol. `lambda n: 1` has a positional-or-keyword argument, but it is named `n` rather than `arg`; `(lambda n: 1)(arg=42)` fails at runtime. If you change the `lambda` argument name to `arg` or make the protocol `arg` parameter positional-only, however, this example does now type-check as expected on our `main` branch.

Most of your other examples now type-check as expected following https://github.com/astral-sh/ruff/pull/20165, except for this one -- we still apparently have a false negative here:

```py
from typing import Protocol

class MyCallable(Protocol):
    def __call__(self, arg: int, /) -> int: ...

def func(mc: MyCallable): ...

def y(arg: str) -> str:
    return "a"

# expect to fail check
func(y)
```

---

_Closed by @AlexWaygood on 2025-09-12 21:20_

---
