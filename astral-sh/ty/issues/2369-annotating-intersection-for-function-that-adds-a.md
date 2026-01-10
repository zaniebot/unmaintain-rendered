```yaml
number: 2369
title: Annotating Intersection for function that adds a base class does not work
type: issue
state: closed
author: classabbyamp
labels:
  - wish
  - dataclasses
assignees: []
created_at: 2026-01-06T18:13:20Z
updated_at: 2026-01-07T12:23:26Z
url: https://github.com/astral-sh/ty/issues/2369
synced_at: 2026-01-10T01:56:41Z
```

# Annotating Intersection for function that adds a base class does not work

---

_Issue opened by @classabbyamp on 2026-01-06 18:13_

### Summary

playground: https://play.ty.dev/7e5a436d-3147-4173-9ed2-d119d3f81a7c

I'm attempting to write a decorator function that takes a class, adds a base class, and makes it a dataclass.

currently, I'm annotating it like so:

```py
from dataclasses import dataclass
from typing import Callable, dataclass_transform, TYPE_CHECKING, TypeVar

if TYPE_CHECKING:
    from _typeshed import DataclassInstance
    from ty_extensions import Intersection

class MyBase:
    @classmethod
    def lorem(cls, *args, **kwargs):
        return cls(*args, **kwargs)

    def ipsum(self):
            return dict(self.__dict__)

class Foo:
    @dataclass_transform()
    def __call__[T](self, cls: type[T], **kwargs) -> Intersection[T, MyBase, DataclassInstance] | Callable[..., Intersection[T, MyBase, DataclassInstance]]:
        def wrap(cls):
            if MyBase not in cls.__bases__:
                cls = type(cls.__name__, (MyBase,) + cls.__bases__, dict(cls.__dict__))
            return dataclass(cls, **kwargs)

        if cls is None:
            return wrap # @foo(...)
        return wrap(cls) # @foo

foo = Foo()

@foo
class Struct:
    bar: str
    baz: int

s = Struct(bar="yo", baz="something")
print(s)

s = Struct("yo", "something")
print(s)

s = Struct.lorem(bar="yo", baz="something")
print(s)

print(s.ipsum())
```
which gives several errors when trying to use it:
```
error[unknown-argument]: Argument `bar` does not match any known parameter of bound method `__init__`
  --> test.py:37:12
   |
35 |     baz: int
36 |
37 | s = Struct(bar="yo", baz="something")
   |            ^^^^^^^^
38 | print(s)
   |
info: Method signature here
   --> stdlib/builtins.pyi:136:9
    |
134 |     @__class__.setter
135 |     def __class__(self, type: type[Self], /) -> None: ...
136 |     def __init__(self) -> None: ...
    |         ^^^^^^^^^^^^^^^^^^^^^^
137 |     def __new__(cls) -> Self: ...
138 |     # N.B. `object.__setattr__` and `object.__delattr__` are heavily special-cased by type checkers.
    |
info: rule `unknown-argument` is enabled by default

error[unknown-argument]: Argument `baz` does not match any known parameter of bound method `__init__`
  --> test.py:37:22
   |
35 |     baz: int
36 |
37 | s = Struct(bar="yo", baz="something")
   |                      ^^^^^^^^^^^^^^^
38 | print(s)
   |
info: Method signature here
   --> stdlib/builtins.pyi:136:9
    |
134 |     @__class__.setter
135 |     def __class__(self, type: type[Self], /) -> None: ...
136 |     def __init__(self) -> None: ...
    |         ^^^^^^^^^^^^^^^^^^^^^^
137 |     def __new__(cls) -> Self: ...
138 |     # N.B. `object.__setattr__` and `object.__delattr__` are heavily special-cased by type checkers.
    |
info: rule `unknown-argument` is enabled by default

error[too-many-positional-arguments]: Too many positional arguments to bound method `__init__`: expected 1, got 3
  --> test.py:40:12
   |
38 | print(s)
39 |
40 | s = Struct("yo", "something")
   |            ^^^^
41 | print(s)
   |
info: Method signature here
   --> stdlib/builtins.pyi:136:9
    |
134 |     @__class__.setter
135 |     def __class__(self, type: type[Self], /) -> None: ...
136 |     def __init__(self) -> None: ...
    |         ^^^^^^^^^^^^^^^^^^^^^^
137 |     def __new__(cls) -> Self: ...
138 |     # N.B. `object.__setattr__` and `object.__delattr__` are heavily special-cased by type checkers.
    |
info: rule `too-many-positional-arguments` is enabled by default

error[unresolved-attribute]: Class `Struct` has no attribute `lorem`
  --> test.py:43:5
   |
41 | print(s)
42 |
43 | s = Struct.lorem(bar="yo", baz="something")
   |     ^^^^^^^^^^^^
44 | print(s)
   |
info: rule `unresolved-attribute` is enabled by default

Found 4 diagnostics
```
(and of course the code doesn't run with this type hint due to #2084, but that's beside the point)

### Version

ty 0.0.9

---

_Comment by @AlexWaygood on 2026-01-06 18:22_

Hi, thanks for the report!

We don't currently respect the return type for class decorators at all, I'm afraid -- this is tracked in https://github.com/astral-sh/ty/issues/143. But even if we did, I think it's pretty unlikely that we'd support the specific pattern you're trying to do here. `dataclass_transform` is very heavily special-cased by type checkers; asking a type-checker to recognise an implicit call to a dataclass-transform-decorated `__call__` method is quite a tall order. The specific `dataclass_transform` patterns that a type checker is expected to understand are enumerated in [the spec](https://typing.python.org/en/latest/spec/dataclasses.html#the-dataclass-transform-decorator).

---

_Label `dataclasses` added by @carljm on 2026-01-07 00:28_

---

_Label `wish` added by @carljm on 2026-01-07 00:28_

---

_Comment by @AlexWaygood on 2026-01-07 12:23_

I think it's very unlikely that we'll ever add support for this pattern unless it's added to the typing spec -- it just doesn't come up enough, I don't think. I'm very happy to reopen if you're able to show that I'm mistaken there, but for now I'll close this so that we can keep our issue tracker actionable :-)

I see you also opened a [discuss.python.org post](https://discuss.python.org/t/type-hinting-a-decorator-that-adds-a-base-class/105557), which is a great forum to ask for help on alternative patterns to use here

---

_Closed by @AlexWaygood on 2026-01-07 12:23_

---
