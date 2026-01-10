```yaml
number: 2047
title: Covariant protocols confuse generic type inference
type: issue
state: closed
author: refi64
labels: []
assignees: []
created_at: 2025-12-18T01:51:15Z
updated_at: 2025-12-18T09:48:51Z
url: https://github.com/astral-sh/ty/issues/2047
synced_at: 2026-01-10T01:53:59Z
```

# Covariant protocols confuse generic type inference

---

_Issue opened by @refi64 on 2025-12-18 01:51_

### Summary

I don't *think* this is a duplicate, but honestly this is really strange to search and thus I might not be doing it correctly.

https://play.ty.dev/4c5a6497-d459-4ad9-8b51-11ea091cd1cb

```python
from typing import Protocol

# This *needs* to be a protocol. If you:
# - Remove the (Protocol) base.
# - `...` -> `assert False` (so ty doesn't complain about the missing return value).
# then this all *works*.
class Co[T](Protocol):
    def x(self) -> T:
        ...

# ===== EXAMPLE 1 =====

class X[T]:
    def __init__(self, a: Co[T], b: T) -> None:
        pass

def example_1(a: Co[str], b: int) -> None:
    # This line results in:
    # - Revealed type: `X[Unknown]`
    # - Argument to bound method `__init__` is incorrect: Expected `Co[int]`, found `Co[str]`
    # even though a is Co[str] and thus T could be inferred as `str | int`.
    # Pyright *does* infer it as `X[str | int]`, and mypy infers it as `X[object]`.
    reveal_type(X(a, b))

    # If we explicitly set the type of T, this passes type checking:
    # - Revealed type: `X[int | str]`
    reveal_type(X[int | str](a, b))

# ===== EXAMPLE 2 =====
# This one swaps out the constructor invocation for a regular function.

def f[T](a: Co[T], b: T) -> T:
    assert False

def id1(x: Co[int | str]) -> Co[int | str]: return x
def id2(x: int | str) -> int | str: return x

def example_2(a: Co[str], b: int) -> None:
    # - Revealed type: `int`
    # - Argument to function `f` is incorrect: Expected `Co[int]`, found `Co[str]`
    # Similar to the above, pyright infers `str | int` and mypy `object`.
    reveal_type(f(a, b))

    # We can't explicitly set T for a function, so this passes `a` and `b` into
    # two identity functions to force T to be `int | str`.
    # - Revealed type: `int | str`
    reveal_type(f(id1(a), id2(b)))
```

This is a "reduced" (lol) example that I got from this smaller failure (https://play.ty.dev/33bb9cbc-5d32-4966-b7c5-3645a166140a):

```python
from typing import reveal_type
reveal_type(dict({'a': 'b'}, c=1))
```

which passes under pyright with `dict[str, str | int]` and mypy with `dict[str, object]` but fails under ty.

**EDIT:** specifically, the `dict` constructor takes an `Iterable[tuple[...]]`, and `Iterable` is a covariant protocol.

### Version

ty 0.0.2

---

_Comment by @refi64 on 2025-12-18 01:58_

argh and just a few minutes later I find #1714, this might be the same issue?

---

_Comment by @sharkdp on 2025-12-18 09:48_

Thank you for reporting this. This looks very much related to #1714, yes.

---

_Closed by @sharkdp on 2025-12-18 09:48_

---
