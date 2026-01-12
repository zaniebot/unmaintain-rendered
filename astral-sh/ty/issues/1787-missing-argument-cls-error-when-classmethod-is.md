```yaml
number: 1787
title: "Missing argument (`cls`) error when `classmethod` is mixed with other decorators"
type: issue
state: closed
author: AA-Turner
labels:
  - generics
assignees: []
created_at: 2025-12-05T21:57:19Z
updated_at: 2025-12-16T17:16:42Z
url: https://github.com/astral-sh/ty/issues/1787
synced_at: 2026-01-12T15:54:25Z
```

# Missing argument (`cls`) error when `classmethod` is mixed with other decorators

---

_@AA-Turner_

### Summary

I couldn't find an existing issue for this. Error is new in alpha 32, no problems reported in alpha 31.

```
error[missing-argument]: No argument provided for required parameter `cls`
   --> tests\test_intl\test_intl.py:880:10
    |
879 |     # see: https://github.com/pytest-dev/pytest/issues/363
880 |     with pytest.MonkeyPatch.context() as mock:
    |          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
881 |         clock: _MockClock
882 |         if os.name == 'posix':
    |
info: Union variant `(cls) -> _GeneratorContextManager[Unknown, None, None]` is incompatible with this call site
info: Attempted to call union type `((cls) -> _GeneratorContextManager[Unknown, None, None]) | ((cls) -> _GeneratorContextManager[Unknown, None, None])`
info: rule `missing-argument` is enabled by default
```

Minimal reproducer ([playground](https://play.ty.dev/2f60d178-9bc8-497a-99fd-02bf7f581465)):

```python
from collections.abc import Generator
from contextlib import contextmanager

class Spam:
    @contextmanager
    @classmethod
    def context(cls) -> Generator[None]:
        yield None


def func():
    with Spam.context() as ctx:  # No argument provided for required parameter `cls` (missing-argument) [Ln 12, Col 10]
        print("blah")
```

I imagine this is a bad interaction between the classmethod decorator and other decorators?

A

### Version

ty 0.0.1-alpha.32 (84a188116 2025-12-05)

---

_Comment by @carljm on 2025-12-05 22:50_

Looks like we need to bind `cls` on classmethods when solving a paramspec.

---

_Added to milestone `Beta` by @carljm on 2025-12-05 22:50_

---

_Label `generics` added by @carljm on 2025-12-05 22:51_

---

_Assigned to @dhruvmanila by @carljm on 2025-12-05 22:51_

---

_Comment by @dhruvmanila on 2025-12-08 06:22_

I think this requires https://github.com/astral-sh/ruff/pull/21685, I tried the minimal reproducer on that branch and the diagnostics goes away.

---

_Assigned to @ibraheemdev by @carljm on 2025-12-08 17:38_

---

_Unassigned @dhruvmanila by @carljm on 2025-12-08 17:38_

---

_Comment by @aidandj on 2025-12-10 05:29_

I'm guessing [this](https://play.ty.dev/9e2b444d-8c62-43e4-bcba-52c9b02d597a) is the same/similar issue. If not I can open a new issue.

```
from typing import overload, Type, Dict, TypeVar, Iterator, Tuple, Union, Set
from contextlib import contextmanager

class Base: ...


class Child(Base): ...


T = TypeVar("T", bound=Base)

@contextmanager
def cm(
    d: Dict[Type[T], int] | Dict[str, int],
):
    yield ({},)


with cm({Child: 1}) as (): # Argument is incorrect: Expected `dict[type[T@cm], int] | dict[str, int]`, found `dict[Unknown | <class 'Child'>, Unknown | int]` (invalid-argument-type) [Ln 19, Col 9]
    pass
```


---

_Comment by @dhruvmanila on 2025-12-10 06:17_

> I'm guessing [this](https://play.ty.dev/9e2b444d-8c62-43e4-bcba-52c9b02d597a) is the same/similar issue. If not I can open a new issue.

No, this is related to https://github.com/astral-sh/ty/issues/1729

---

_Comment by @carljm on 2025-12-10 18:03_

@dhruvmanila https://github.com/astral-sh/ruff/pull/21685 has landed, but the reproducer here still errors.

---

_Unassigned @ibraheemdev by @carljm on 2025-12-10 18:04_

---

_Comment by @dhruvmanila on 2025-12-11 08:29_

I don't think this is related to `ParamSpec` as the issue exists on `main` as well https://play.ty.dev/939b003c-20bd-4aeb-b607-54d4113d6221:

```py
from collections.abc import Callable
from typing import Any


def decorator(fn: Callable[[Any], None]) -> Callable[[Any], None]:
    return fn


class Foo:
	@decorator
    @classmethod
    def context(cls) -> None:
        return None

# ty: No argument provided for required parameter 1 [missing-argument]
Foo.context()
```

I think I know where the issue is -- in `Type::bindings` the type of `Foo.context` is not `BoundMethod` but `CallableType` which is why there's no "bound" type argument that's being added to the `CallableBinding` [here](https://github.com/astral-sh/ruff/blob/24ed28e31434106e3d0bcc8d1a1ede50b645c5da/crates/ty_python_semantic/src/types/call/bind.rs#L1567-L1570) which is required to get an accurate `CallArguments` [here](https://github.com/astral-sh/ruff/blob/24ed28e31434106e3d0bcc8d1a1ede50b645c5da/crates/ty_python_semantic/src/types/call/arguments.rs#L127-L142).

---

_Renamed from "ty 0.0.1a32: classmethod context manager error with `[missing-argument]`" to "Missing argument error when `classmethod` is mixed with other decorators" by @dhruvmanila on 2025-12-11 08:51_

---

_Renamed from "Missing argument error when `classmethod` is mixed with other decorators" to "Missing argument (`cls`) error when `classmethod` is mixed with other decorators" by @dhruvmanila on 2025-12-11 08:52_

---

_Comment by @carljm on 2025-12-11 18:03_

I suspect that we need to start representing `@classmethod` and `@staticmethod` as a transformation to a staticmethod/classmethod type (which preserves a reference to the underlying function/callable), rather than via flags on function literal types. This would also contribute to fixing https://github.com/astral-sh/ty/issues/1452

---

_Assigned to @carljm by @carljm on 2025-12-12 03:38_

---

_Comment by @carljm on 2025-12-13 05:03_

> I suspect that we need to start representing `@classmethod` and `@staticmethod` as a transformation to a staticmethod/classmethod type

We may still want to do this, but I realized that it's not a solution to this issue. At least, not a complete one -- it might work for the case where `@classmethod` is applied above another decorator, but it wouldn't work for the case where a Callable-returning decorator is applied above `@classmethod`. Such a decorator will not naturally preserve a staticmethod/classmethod type, because it returns a Callable type.

This problem is fundamentally not solvable in a fully correct/robust way, much like the more general question of whether a Callable type is a bound-method descriptor or not. The Callable type does not clarify this distinction, and there's no general way to tell from the signature whether a decorator that takes in a Callable and returns a Callable will (at runtime) actually return a classmethod or not.

And it's not correct to apply the self-binding behavior to the classmethod's signature "early" (e.g. when some other decorator is applied) -- that behavior happens only later, via the descriptor protocol.

So I did what it appears other type checkers also do, and applied a heuristic where we assume that a decorator returning a Callable type "propagates" classmethod-ness, if it is applied using decorator syntax, to a method also decorated with `@classmethod`. This is also the same heuristic we already use for propagating "function-like-ness".

---

_Closed by @carljm on 2025-12-16 17:16_

---
