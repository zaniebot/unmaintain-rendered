```yaml
number: 1513
title: "`issubclass()` narrowing leads to incorrect results for a generic `@final` class"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - narrowing
assignees: []
created_at: 2025-11-10T11:13:41Z
updated_at: 2025-12-10T16:18:11Z
url: https://github.com/astral-sh/ty/issues/1513
synced_at: 2026-01-12T15:54:25Z
```

# `issubclass()` narrowing leads to incorrect results for a generic `@final` class

---

_@AlexWaygood_

### Summary

Here's our behaviour on `main`:

```py
from typing import final

@final
class Foo[T]:
    x: T

def f(x: type[Foo]):
    if issubclass(x, Foo):
        reveal_type(x)  # `Never` (should be `<class 'Foo[Unknown]'>`)
    else:
        reveal_type(x)  # `<class 'Foo[Unknown]'>` (should be `Never`)

# `memoryview` is `@final` and generic in typeshed
def g(x: type[memoryview]):
    if issubclass(x, memoryview):
        reveal_type(x)  # `Never` (should be `<class 'memoryview[Unknown]'>`)
    else:
        reveal_type(x)  # `<class 'memoryview[Unknown]'>` (should be `Never`)
```

https://play.ty.dev/bc10f742-75d1-4d82-87dd-7fa07c89e151

All inhabitants of `type[memoryview]` are, by definition, subclasses of `memoryview`, so our behaviour here is clearly incorrect. I'm not totally sure what the fix should be, however.

There are several things at play here. For any class `C`:
1. We eagerly simplify `type[C]` ("any subclass of `C`") to `<class 'C'>` (a singleton type consisting only of the class object `C`) if `C` is `@final`
2. `issubclass()` leads us to narrow the type by intersecting the type with `Top[type[C]]`, which for a `@final` class therefore simplifes to `Top[<class 'C'>]`, and for a `@final` generic class therefore simplifies to `<class 'Top[C[Unknown]]'>`
3. `<class 'C[Unknown]'>` is currently thought by ty to be disjoint from `<class 'Top[C[Unknown]]'>`

---

_Added to milestone `GA` by @AlexWaygood on 2025-11-10 11:13_

---

_Label `bug` added by @AlexWaygood on 2025-11-10 11:13_

---

_Label `narrowing` added by @AlexWaygood on 2025-11-10 11:13_

---

_Comment by @carljm on 2025-11-10 17:30_

There is clearly an error in (3). There might also be some other issues (e.g. in `is_singleton`) in how we treat a `Type::GenericAlias` whose specialization is a top materialization. The top materialization of `<class 'C[Unknown]'>` is the union of every possible generic alias of `C` (that is, every possible materialization of `<class 'C[Unknown]'>`). So it's not a singleton type at all, it's an (infinitely sized) union of generic aliases. Thus it definitely should not be disjoint with `<class 'C[Unknown]'>`. Every materialization of `<class 'C[Unknown]'>` is a subtype of the top materialization of `<class 'C[Unknown]'>` -- that's the definition of a top materialization.

---

_Comment by @oconnor663 on 2025-11-24 19:18_

A new case of this has come up in https://github.com/astral-sh/ruff/pull/21552#issuecomment-3572330789, for example:

```py
from typing import Any, final

@final
class Foo[T]:
    ...

# error [invalid-assignment]: Object of type `<class 'Foo[str]'>` is not assignable to `<class 'Foo[Any]'>`
_: type[Foo[Any]] = Foo[str]
```

---

_Closed by @sharkdp on 2025-12-10 16:18_

---
