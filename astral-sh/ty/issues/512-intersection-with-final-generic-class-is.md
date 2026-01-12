```yaml
number: 512
title: "Intersection with `@final` generic class is incorrectly simplified to Never"
type: issue
state: open
author: JelleZijlstra
labels:
  - bug
  - generics
  - set-theoretic types
assignees: []
created_at: 2025-05-26T01:12:52Z
updated_at: 2025-10-06T16:56:27Z
url: https://github.com/astral-sh/ty/issues/512
synced_at: 2026-01-12T15:54:23Z
```

# Intersection with `@final` generic class is incorrectly simplified to Never

---

_@JelleZijlstra_

### Summary

This intersection is incorrectly simplified to Never.

```python
from typing import TypeVar, Generic, final, reveal_type
from ty_extensions import Intersection

T_co = TypeVar("T_co", covariant=True)

class Base(Generic[T_co]):
    def f(self) -> T_co:
        raise NotImplementedError

@final
class Child(Base[T_co]):
    pass

def f(x: Intersection[Base[int], Child[object]]):
    reveal_type(x)  # Never, expect Child[int]
```

https://play.ty.dev/06b37356-775c-4128-ab4a-7a65e159bc87

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-05-26 06:25_

---

_Label `generics` added by @AlexWaygood on 2025-05-26 06:25_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-05-26 06:25_

---

_Comment by @AlexWaygood on 2025-05-26 06:28_

I think this is possibly related to #492

---

_Comment by @carljm on 2025-05-30 00:07_

I don't think it's #492 exactly -- there's no attempt here to have different specializations of a single class in the same MRO. Rather the problem is that in `is_disjoint_from` we don't account for the possibility that a final generic type can overlap with a base class that it isn't a subtype of (due to variance). `Child[object]` is not a subtype of `Base[int]` (wrong variance), but `Child[int]` is a subtype of both, so they should not be considered disjoint (and we should simplify the intersection to the common subtype, which is kind of a separate but related issue).

---

_Comment by @AlexWaygood on 2025-05-30 22:20_

It's related to #492 in that the cause of #492 is that our MRO now consists of a sequence of generic aliases rather than a sequence of class literals following our implementation of generics, but some parts of our code (such as the check for duplicate bases) have not yet been adjusted to account for that change. Similarly here, our implementation of disjointness for `@final` instance types calls `Class::is_subclass_of()`, and `Base[int].is_subclass_of(Child[object])` does not return `true` in our current model. This is because `Child[object]` does not appear in the MRO of `Base[int]`.

https://github.com/astral-sh/ruff/blob/aa1fad61e0f9fe0c7faac876f2ef55cd3817fc6c/crates/ty_python_semantic/src/types/instance.rs#L94-L101

---

_Added to milestone `Beta` by @carljm on 2025-07-23 23:17_

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 15:11_

---

_Added to milestone `GA` by @carljm on 2025-08-15 15:11_

---

_Comment by @JelleZijlstra on 2025-09-05 00:26_

Another example that @carljm tells me probably has the same root cause:

```python
from ty_extensions import Top, Intersection
from typing import Any

class Base[T]:
    def get(self) -> T: raise NotImplementedError
    def push(self, obj: T) -> None: ...

class Child[T](Base[T]):
    pass

def _(x: Intersection[Base[Any], Child[Any]]):
    reveal_type(x.get)  # ty: Never; expect something like `() -> Any`
```


---

_Renamed from "Intersection with @final generic class is incorrectly simplified to Never" to "Intersection with `@final` generic class is incorrectly simplified to Never" by @AlexWaygood on 2025-10-06 16:56_

---
