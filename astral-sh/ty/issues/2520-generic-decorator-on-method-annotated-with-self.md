```yaml
number: 2520
title: generic decorator on method annotated with Self solves to Unknown
type: issue
state: open
author: carljm
labels:
  - generics
assignees: []
created_at: 2026-01-15T15:43:40Z
updated_at: 2026-01-15T15:43:57Z
url: https://github.com/astral-sh/ty/issues/2520
synced_at: 2026-01-15T15:50:04Z
```

# generic decorator on method annotated with Self solves to Unknown

---

_@carljm_

> ```py
> from typing import Self, TypeVar, Callable, Generic
> 
> _T = TypeVar("_T")
> 
> class Box(Generic[_T]):
>     def __init__(self, value: _T) -> None:
>         self.value = value
> 
> def boxify(func: Callable[..., _T]) -> Callable[..., Box[_T]]:
>     def wrapper(*args, **kwargs) -> Box[_T]:
>         return Box(func(*args, **kwargs))
>     return wrapper
> 
> class Foo:
>     def no_decorator(self) -> Self:
>         return self
> 
>     @boxify
>     def with_decorator(self) -> Self:
>         return self
> 
> obj = Foo()
> reveal_type(obj.no_decorator())    # Foo
> reveal_type(obj.with_decorator())  # ty: Box[Unknown], pyright: Box[Foo]
> ``` 

 _Originally posted by @carljm in [#2344](https://github.com/astral-sh/ty/issues/2344#issuecomment-3712554298)_

---

_Label `generics` added by @carljm on 2026-01-15 15:43_

---

_Added to milestone `Stable` by @carljm on 2026-01-15 15:43_

---
