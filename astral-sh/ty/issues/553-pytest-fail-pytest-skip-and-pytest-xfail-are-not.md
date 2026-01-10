```yaml
number: 553
title: "`pytest.fail()`, `pytest.skip()` and `pytest.xfail()` are not considered callable (`call-not-callable` diagnostic emitted)"
type: issue
state: closed
author: johnniemorrow
labels:
  - generics
  - Protocols
assignees: []
created_at: 2025-05-30T16:54:17Z
updated_at: 2025-07-02T16:03:58Z
url: https://github.com/astral-sh/ty/issues/553
synced_at: 2026-01-10T02:07:36Z
```

# `pytest.fail()`, `pytest.skip()` and `pytest.xfail()` are not considered callable (`call-not-callable` diagnostic emitted)

---

_Issue opened by @johnniemorrow on 2025-05-30 16:54_

### Summary

Hi,

I'm seeing a `call-non-callable` error calling `pytest.fail()`:


```python
import pytest

def test_pytest():
    pytest.fail("because...")

```

```bash
$ ty check tests/test_pytest.py
error[call-non-callable]: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
 --> tests/test_pytest.py:4:5
  |
3 | def test_pytest():
4 |     pytest.fail("because...")
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^
  |
info: rule `call-non-callable` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.1-alpha.7

---

_Label `generics` added by @carljm on 2025-05-30 16:56_

---

_Label `Protocols` added by @carljm on 2025-05-30 16:56_

---

_Comment by @carljm on 2025-05-30 16:58_

Thanks for the report!

https://github.com/astral-sh/ty/issues/480 is at least part of the issue here (see https://github.com/pytest-dev/pytest/blob/main/src/_pytest/outcomes.py#L89 ) -- not sure if there will be anything additional needed on the Protocols side once that's fixed.



---

_Comment by @AlexWaygood on 2025-05-30 17:31_

#480 is insufficient, but it's not because of lack of protocol support, it's because of https://discuss.python.org/t/when-should-we-assume-callable-types-are-method-descriptors/92938 and related issues:
- Pytest's `_WithException` protocol has a `__call__` instance attribute, but we only consider types to be callable if they have a class `__call__` attribute
- `_WithException.__call__` is annotated as a `Callable` type, but it's not clear whether `Callable` types should be treated as having `__get__` methods, and we only view objects as callable if their class `__call__` attributes have `__get__` methods.

---

_Comment by @carljm on 2025-05-30 17:42_

Ok, so this depends on #480, #491, and (a variant case of) #384.

Regarding the latter, I do think there is potentially something Protocol-specific here, in that I think our rationale for assuming an annotated (but not assigned) class attribute is _not_ accessible on the class, doesn't really apply to Protocols. (And I'm increasingly thinking maybe we should just abandon our strictness here and follow mypy/pyright, allowing anything annotated on the class to be potentially present on the class.)

---

_Comment by @AlexWaygood on 2025-05-30 17:56_

> Regarding the latter, I do think there is potentially something Protocol-specific here, in that I think our rationale for assuming an annotated (but not assigned) class attribute is _not_ accessible on the class, doesn't really apply to Protocols.

I can see reasons for accepting the unsoundness for pragmatic reasons and doing what mypy/pyright do here w.r.t. allowing anything annotated on the class to be accessed on the class. Similarly with assuming that `__get__` exists on `Callable` types. But I don't see any reason to treat attribute access on protocols differently to attribute access on other classes. It's pretty easy to demonstrate that mypy/pyright's behaviour with protocols in this regard is unsound -- mypy accepts this, but it fails at runtime because the `__call__` attribute is only accessible on instances:

```py
from typing import Protocol, Callable

class Foo(Protocol):
    __call__: Callable[..., None]

class Bar(Foo):
    def __init__(self) -> None:
        self.__call__ = lambda *args, **kwargs: None

def f(x: Foo):
    x()

f(Bar())
```

---

_Comment by @carljm on 2025-05-30 18:02_

I wouldn't want to adopt that unsoundness; I'd want to instead reject that as a protocol match. But I also agree that interpreting unassigned-but-annotated attributes differently for Protocols would be confusing, because it would mean that a class with an annotated assignment might not satisfy a protocol with an identical-looking annotated assignment. That's not great.

So yeah -- it's increasingly feeling like our two options are a) allow annotated-but-unassigned class attributes to be accessed on the class, or b) convince an unknown number of existing projects to change the way they annotate. And (b) sounds like a real uphill battle.

---

_Comment by @AlexWaygood on 2025-06-01 14:10_

It doesn't really do much to address the general issue, because I'm sure other users are also relying on the mypy/pyright behaviour here. But FWIW, pytest specifically appear open to changing their annotations to remove ambiguity here: https://github.com/pytest-dev/pytest/pull/13445

---

_Renamed from "pytest.fail() reports call-not-callable" to "pytest.fail() and pytest.skip() are not considered callable (`call-not-callable` diagnostic emitted)" by @AlexWaygood on 2025-06-18 14:55_

---

_Renamed from "pytest.fail() and pytest.skip() are not considered callable (`call-not-callable` diagnostic emitted)" to "`pytest.fail()`, `pytest.skip()` and `pytest.xfail()` are not considered callable (`call-not-callable` diagnostic emitted)" by @AlexWaygood on 2025-07-01 13:16_

---

_Comment by @AlexWaygood on 2025-07-01 13:17_

> But FWIW, pytest specifically appear open to changing their annotations to remove ambiguity here: [pytest-dev/pytest#13445](https://github.com/pytest-dev/pytest/pull/13445)

(This PR has now been merged to the pytest `main` branch, but hasn't yet made its way into a pytest release.)

---

_Closed by @sharkdp on 2025-07-02 16:03_

---
