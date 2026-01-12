```yaml
number: 2324
title: "[panic] Interaction between the `__call__` method, decoraters, callable annotations, and self referential return types on 3.14"
type: issue
state: open
author: eliasbenb
labels:
  - great-writeup
  - fatal
assignees: []
created_at: 2026-01-05T00:29:10Z
updated_at: 2026-01-09T13:44:47Z
url: https://github.com/astral-sh/ty/issues/2324
synced_at: 2026-01-12T15:54:26Z
```

# [panic] Interaction between the `__call__` method, decoraters, callable annotations, and self referential return types on 3.14

---

_@eliasbenb_

A panic occurs when a class's `__call__` method is annotated to return a union containing multiple `Callable` types and the class's own type, and that method is used as a decorator. The issue appears to be specific to `__call__`; equivalent annotations on other methods do not reproduce the problem.

Although this may be related to #256, the interaction here seems pretty unique from my (inexperienced) perspective.

**Context:**

I encountered this issue while using the [`limiter`](https://github.com/alexdelorenzo/limiter) package, specifically when instantiating and calling the [`Limiter` class](https://github.com/alexdelorenzo/limiter/blob/bb46cd45341907b63779b1d33188f96c92b57769/limiter/limiter.py#L94-L101) as a decorator.

After reducing the problem, I was able to reproduce the panic with a minimal example that does not depend on `limiter` itself.

**Minimal Reproduction:**

Playground link: [https://play.ty.dev/a96aa4cb-45b9-469c-b846-0e113939d774](https://play.ty.dev/a96aa4cb-45b9-469c-b846-0e113939d774)

```py
from collections.abc import Callable


class SomeWrapper:
    # Must return the union of at least two callables
    # Must return self type in the union
    # Must be a class method or instance method, but not a static method (of course)
    # Must be the __call__ method (I tried `some_func`, `__some_func__`, `__enter__`, etc., no other method triggers the issue)
    def __call__(self, fn) -> Callable | Callable[[int], int] | SomeWrapper:
        raise NotImplementedError


@SomeWrapper().__call__  # Instantiate and call
def some_fn():
    raise NotImplementedError
```

From testing and reduction, all of the following appear to be required:

1. The method must be named `__call__`: other method names I tested (`some_func`, `__enter__`, `__some_func__`, etc.) do not trigger the issue.
2. The method must be an instance or class method: static methods do not reproduce the behavior.
3. The return annotation must be a `Union` that includes: at least two `Callable` types, and the enclosing class type (self).
4. The method must be used as a decorator (`@SomeWrapper().__call__`)

Removing any one of these conditions prevented the panic during my testing.

**Observations:**

Even though I'm no expert (or ameteur :p) on the internals of ty, the `__call__` method being a requirement seems interesting/unexpected here - I'd look into that first (in conjunction with the decorator usage requirement).

**Logs:**

<details>
  <summary>Panic trace (ran against above snippet)</summary>

  ```
  $ RUST_BACKTRACE=1 ty check ./test.py 
  error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ce80691/src/function/execute.rs:321:21 when checking `/workspaces/anibridge/test.py`: `BoundMethodType < 'db >::into_callable_type_(Id(a801)): execute: too many cycle iterations`
  info: This indicates a bug in ty.
  info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
  info: Platform: linux x86_64
  info: Version: 0.0.8
  info: Args: ["ty", "check", "./test.py"]
  info: Backtrace:
     0: <unknown>
     1: <unknown>
     2: <unknown>
     3: <unknown>
     4: <unknown>
     5: <unknown>
     6: <unknown>
     7: <unknown>
     8: <unknown>
     9: <unknown>
    10: <unknown>
    11: <unknown>
    12: <unknown>
    13: <unknown>
    14: <unknown>
    15: <unknown>
    16: <unknown>
    17: <unknown>
    18: <unknown>
    19: <unknown>
    20: <unknown>
    21: <unknown>
    22: <unknown>
    23: <unknown>
    24: <unknown>
    25: <unknown>
    26: <unknown>
    27: <unknown>
    28: <unknown>
    29: <unknown>
    30: <unknown>
    31: <unknown>
    32: <unknown>
    33: <unknown>
    34: <unknown>
    35: <unknown>
    36: <unknown>
    37: <unknown>
    38: <unknown>
    39: <unknown>
    40: <unknown>
    41: <unknown>
    42: <unknown>
    43: <unknown>
    44: <unknown>
    45: <unknown>
    46: <unknown>
    47: <unknown>
    48: <unknown>
    49: <unknown>
    50: <unknown>
    51: <unknown>
    52: clone
  
  info: query stacktrace:
     0: is_redundant_with_impl(Id(a004))
               at crates/ty_python_semantic/src/types.rs:2035
     1: infer_deferred_types(Id(1403))
               at crates/ty_python_semantic/src/types/infer.rs:145
     2: FunctionType < 'db >::signature_(Id(4404))
               at crates/ty_python_semantic/src/types/function.rs:839
     3: infer_definition_types(Id(1405))
               at crates/ty_python_semantic/src/types/infer.rs:103
     4: infer_scope_types(Id(1000))
               at crates/ty_python_semantic/src/types/infer.rs:69
     5: check_file_impl(Id(c00))
               at crates/ty_project/src/lib.rs:548
  
  
  Found 1 diagnostic
  WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
  ```

</details>

---

_Label `great-writeup` added by @MichaReiser on 2026-01-05 08:55_

---

_Label `fatal` added by @MichaReiser on 2026-01-05 08:56_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2026-01-05 08:56_

---

_Comment by @dhruvmanila on 2026-01-05 08:57_

Thanks for providing all the details!

It's weird that I'm not seeing a panic when I run the latest version of ty nor when I use the latest `main` although opening it in the playground does produce the panic. I tried it on various Python versions as well (3.10 through 3.13), can you tell me which Python version are you running this on? Could it be specific to the platform as I'm on aarch64 (OP is linux x86_64)?

Edit: I tried it with `--python-platform=linux` which didn't produce the panic either.

---

_Comment by @MichaReiser on 2026-01-05 09:00_

The playground uses 3.14

---

_Comment by @dhruvmanila on 2026-01-05 09:01_

> The playground uses 3.14

Ah yes, that does reproduce it locally. So, it seems ~specific to the Python version~ to be related to deferred annotations.

---

_Renamed from "[panic] Interaction between the `__call__` method, decoraters, callable annotations, and self referential return types" to "[panic] Interaction between the `__call__` method, decoraters, callable annotations, and self referential return types on 3.14" by @dhruvmanila on 2026-01-05 09:03_

---

_Comment by @AlexWaygood on 2026-01-05 09:08_

Evaluation of annotations is deferred by default on 3.14+ (even without `from __future__ import annotations`); could that be the reason why the Python version is load-bearing...?

---

_Comment by @AlexWaygood on 2026-01-05 11:36_

I confirmed that both of these snippets panic with `--python-version=3.13`: first with a quoted annotation:

```py
from collections.abc import Callable


class SomeWrapper:
    # Must return the union of at least two callables
    # Must return self type in the union
    # Must be a class method or instance method, but not a static method (of course)
    # Must be the __call__ method (I tried `some_func`, `__some_func__`, `__enter__`, etc., no other method triggers the issue)
    def __call__(self, fn) -> Callable | Callable[[int], int] | "SomeWrapper":
        raise NotImplementedError


@SomeWrapper().__call__  # Instantiate and call
def some_fn():
    raise NotImplementedError
```

and secondly, with `from __future__ import annotations` added to the top:

```py
from __future__ import annotations
from collections.abc import Callable


class SomeWrapper:
    # Must return the union of at least two callables
    # Must return self type in the union
    # Must be a class method or instance method, but not a static method (of course)
    # Must be the __call__ method (I tried `some_func`, `__some_func__`, `__enter__`, etc., no other method triggers the issue)
    def __call__(self, fn) -> Callable | Callable[[int], int] | SomeWrapper:
        raise NotImplementedError


@SomeWrapper().__call__  # Instantiate and call
def some_fn():
    raise NotImplementedError
```

---

_Comment by @eliasbenb on 2026-01-06 02:18_

Catching up on the conversation over the last 24 hours - first, I appreciate the quick engagement. It's really nice to see how much care is going into this project.

Thanks @AlexWaygood for clarifying the annotation deferment requirement - I was able to confirm the same behavior on my end.

I've explored this further and was able to reduce the repro a bit more.

My earlier assessment that the return annotation must include "at least two callable types" still seems correct, but importantly, it does not need to involve the stdlib `collections.abc.Callable`. Any two callable-shaped protocols appear sufficient. So, my updated requirement is:

- The union must contain two Protocol types that define a `__call__` method, and
- Their `__call__` methods must be defined distinctly. To be clear, not merely two different Protocol classes, but specifically the `__call__` signatures must be distinct.

I also tested just defining a real/non-protocol class with a `__call__` method, but that didn't cause a panic. So, this seems tied to Protocols somehow.

The following still triggers the panic:

```python
from typing import Protocol


class CustomCallable1(Protocol):
    def __call__(self, x: int) -> None: ...


# Notice that the signature here is distinct from the last
class CustomCallable2(Protocol):
    def __call__(self, y: int) -> None: ...


class SomeWrapper:
    def __call__(self, fn) -> CustomCallable1 | CustomCallable2 | SomeWrapper:
        raise NotImplementedError


@SomeWrapper().__call__  # instantiate and call
def some_fn():
    raise NotImplementedError
```

---

_Assigned to @dcreager by @dcreager on 2026-01-09 13:44_

---
