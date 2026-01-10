```yaml
number: 1504
title: Length of concatenation of tuples is not inferred
type: issue
state: closed
author: ThorstenBuss
labels: []
assignees: []
created_at: 2025-11-08T13:27:59Z
updated_at: 2025-11-09T16:31:22Z
url: https://github.com/astral-sh/ty/issues/1504
synced_at: 2026-01-10T02:06:25Z
```

# Length of concatenation of tuples is not inferred

---

_Issue opened by @ThorstenBuss on 2025-11-08 13:27_

### Summary

When running `ty check` on the following code, it gives you an invalid-return-type error. This is not the expected behavior since the concatenation of a tuple with one entry and one with two entries always has tree entries.
```python
def concat(a: tuple[int], b: tuple[int, int]) -> tuple[int, int, int]:
    return a + b
```

https://play.ty.dev/cdc8212c-6304-491f-904f-b4cd6468992c

### Version

ty 0.0.1-alpha.25

---

_Comment by @AlexWaygood on 2025-11-09 16:31_

Thanks for the feature request!

It's true that other type checkers accept this code. However, it's also easy to demonstrate that there are ways of breaking the assumptions that other type checkers make about this code. For example, both mypy and pyright report no diagnostics on the following snippet and report that the type of `x` is `tuple[int, int, int]`. But at runtime, `x` has value `(1, 2, 3, 4, 5)`, which pretty obviously isn't consistent with that type!

```py
from typing import overload, TypeVar

T = TypeVar("T")

class Foo(tuple[int, int]):
    @overload
    def __add__(self, other: tuple[int, ...]) -> tuple[int, ...]: ...
    @overload
    def __add__(self, other: tuple[T, ...]) -> tuple[int | T, ...]: ...
    def __add__(self, other: tuple[object, ...]) -> tuple[object, ...]:
        return (1, 2, 3, 4, 5)
    
    __radd__ = __add__

def concat(a: tuple[int], b: tuple[int, int]) -> tuple[int, int, int]:
    return a + b

x = concat((1,), Foo((1, 2)))
reveal_type(x)
```

One way to resolve the unsound behaviour of other type checkers here would be to ban users from overriding `__add__` and `__radd__` on tuple subclasses. But this seems unnecessarily restrictive: users might have a good reason to override these methods on custom tuple subclasses (in particular on `NamedTuple` classes, which are all implicitly tuple subclasses). While we plan to take this approach to "solve" tuple-subclass unsoundness for some other areas where we special-case tuples, it also doesn't _fully_ address the unsoundness issues: the tuple subclass could always be defined in a third-party library that isn't type-checked with ty, and ty wouldn't flag the unsound subclass since all diagnostics are silenced for packages installed in your virtual environment.

We actually used to have the special case you're asking for here, but we ripped it out for the reasons stated above, and also a few other reasons:
- It simplifies our implementation internally
- It turns out that the special case for tuple addition makes it more likely that ty runs into pathological performance issues when inferring types for unannotated instance attributes.
- Empirically, [surprisingly little ecosystem code seemed to rely on the special case](https://github.com/astral-sh/ruff/pull/19636#issuecomment-3136012420)

---

_Closed by @AlexWaygood on 2025-11-09 16:31_

---
