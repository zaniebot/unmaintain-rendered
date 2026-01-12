```yaml
number: 1446
title: "Support `cached_property`"
type: issue
state: open
author: graipher
labels:
  - generics
assignees: []
created_at: 2025-10-27T16:06:02Z
updated_at: 2025-12-19T23:32:09Z
url: https://github.com/astral-sh/ty/issues/1446
synced_at: 2026-01-12T15:54:25Z
```

# Support `cached_property`

---

_@graipher_

### Summary

Currently the type of a `functools.cached_property` is `Unknown`, as the standardlibrary `functools` does not have type annotations.

```python
from functools import cached_property
from typing import reveal_type


class Foo:
    @cached_property
    def foo(self) -> str:
        return "This is expensive"


reveal_type(Foo().foo)
```

**Output:**
```
info[revealed-type]: Revealed type
  --> main.py:11:11
   |
11 | reveal_type(Foo().foo)
   |             ^^^^^^^^^ `Unknown`
   |
```

**Expected:**
Some variant of `bound method Foo.foo() -> str`, which is what `ty` reveals without the decorator.

[Playground link](https://play.ty.dev/15997923-202e-4ca2-a82d-cf8f1a0f5b5a)

Is this an issue with typeshed?

### Version

ty 0.0.1-alpha.24

---

_Label `generics` added by @sharkdp on 2025-10-27 16:08_

---

_Comment by @sharkdp on 2025-10-27 16:11_

> Currently the type of a `functools.cached_property` is `Unknown`, as the standardlibrary `functools` does not have type annotations.

ty vendors a copy of typeshed, which does include type annotations for the standard library: https://github.com/python/typeshed/blob/16f766b754405004471af20eeb9a5cf8dea05b44/stdlib/functools.pyi#L227-L238

I think the problem is that we do not induct into `Callable` types when solving type variables while constructing an instance of `cached_property`:

https://github.com/python/typeshed/blob/16f766b754405004471af20eeb9a5cf8dea05b44/stdlib/functools.pyi#L230

So this is probably just related to #623. 

---

_Comment by @Jeremiah-England on 2025-12-19 22:44_

For anyone else finding this who uses cached_property many places, I have gotten around this by defining a private module (e.g. `my_types.py`) with the following code:

```python
# -----------------------------------------------------------------------------
# cached_property shim for ty type checker compatibility
# -----------------------------------------------------------------------------
# `ty` currently fails to infer types through `functools.cached_property` (see
# https://github.com/astral-sh/ty/issues/1446). The type checker cannot properly
# solve type variables when constructing instances of cached_property, resulting
# in `Unknown` types for decorated methods.
#
# In practice, you almost always want the same static behavior as `@property`
# anyway (attribute access reveals the method's return type). So under
# type-checking we alias `cached_property` to `property`, which allows ty to
# correctly infer the return type of decorated methods.
#
# At runtime, we still use the real `functools.cached_property` for its caching
# behavior. This shim provides the best of both worlds: correct type inference
# during static analysis and proper caching at runtime.
#
# Usage:
#     from my_types import cached_property
#
#     class MyClass:
#         @cached_property
#         def expensive_computation(self) -> str:
#             return "This is expensive"
#
#     # Type checker sees: MyClass().expensive_computation -> str
#     # Runtime behavior: Result is cached after first access
if TYPE_CHECKING:
    cached_property = property
else:
    from functools import cached_property  # noqa: F401
```

---

_Comment by @carljm on 2025-12-19 23:25_

We do now infer generics properly from inside Callable types, but unfortunately that wasn't enough to fix this. The remaining problem is our known tech debt around the split between `Type::try_call_constructor` vs `Type::bindings`. Decorator application just uses `Type::try_call` which uses `Type::bindings` and never uses `Type::try_call_constructor`. So currently we don't get things right when a generic class (like `cached_property`) is applied as a decorator. We infer the typevar just fine if you do `cp = cached_property(some_func)`, but not if you use `cached_property` as a decorator.

---

_Added to milestone `Stable` by @carljm on 2025-12-19 23:32_

---
