---
number: 2391
title: "Feat: support `collections.abc.<Class>.register`"
type: issue
state: open
author: NickCrews
labels:
  - wish
assignees: []
created_at: 2026-01-08T04:07:59Z
updated_at: 2026-01-09T01:08:21Z
url: https://github.com/astral-sh/ty/issues/2391
synced_at: 2026-01-10T01:48:23Z
---

# Feat: support `collections.abc.<Class>.register`

---

_Issue opened by @NickCrews on 2026-01-08 04:07_

This is related to https://github.com/astral-sh/ty/issues/2390.

I am working in the ibis project, trying to fix up its typing so that [it's `Schema` class](https://github.com/ibis-project/ibis/blob/e5921c9a851afc8fd3980f6a0bbfd73720b4d5d6/ibis/expr/schema.py#L25) typechecks against a `collections.abc.Mapping[str, ibis.DataType]`. It currently does not.

The root of the issue is that the [Schema](https://github.com/ibis-project/ibis/blob/e5921c9a851afc8fd3980f6a0bbfd73720b4d5d6/ibis/expr/schema.py#L25) class inherits from the [MapSet abstract class](https://github.com/ibis-project/ibis/blob/e5921c9a851afc8fd3980f6a0bbfd73720b4d5d6/ibis/common/collections.py#L157), which itself inherits from ibis's custom [Mapping ABC](https://github.com/ibis-project/ibis/blob/e5921c9a851afc8fd3980f6a0bbfd73720b4d5d6/ibis/common/collections.py#L119), which is effectively a re-implementation of `collections.abc.Mapping`. [The reason](https://github.com/ibis-project/ibis/blob/e5921c9a851afc8fd3980f6a0bbfd73720b4d5d6/ibis/common/collections.py#L20-L23) that we re-implement collections.abc is to avoid metaclass conflicts, and for faster isinstance checks.

See how we have the [`@collections.abc.Mapping.register` decorator on our `Mapping` class](https://github.com/ibis-project/ibis/blob/e5921c9a851afc8fd3980f6a0bbfd73720b4d5d6/ibis/common/collections.py#L118-L119)? This makes it so that at runtime, isinstance checks work. BUT, it appears that ty doesn't support this registration mechanism for any `collections.abc` classes, except for Sized for some reason. See this screenshot of the ibis source code, you can see how ty doesn't seem to recognize the .register() method of any of the other classes:

<img width="513" height="607" alt="Image" src="https://github.com/user-attachments/assets/8aa000ca-4a7d-4841-ba9c-34011cd82b9c" />

For a simpler reproducer, see this file:

```python
from __future__ import annotations

from collections.abc import ItemsView, Iterable, Iterator, KeysView, Mapping, ValuesView


class MyMapping:  # noqa: PLW1641
    def __len__(self) -> int:
        return 2

    def __getitem__(self, key: str, /) -> int:
        if key == "a":
            return 1
        elif key == "b":
            return 2
        else:
            raise KeyError(key)

    def __iter__(self) -> Iterator[str]:
        yield "a"
        yield "b"

    def __contains__(self, key: object, /) -> bool:
        return key in ("a", "b")

    def __eq__(self, other: object, /) -> bool:
        if not isinstance(other, Mapping):
            return False
        return dict(self.items()) == dict(other.items())

    def keys(self) -> KeysView[str]:
        return {"a": 1, "b": 2}.keys()

    def values(self) -> ValuesView[int]:
        return {"a": 1, "b": 2}.values()

    def items(self) -> ItemsView[str, int]:
        return {"a": 1, "b": 2}.items()

    def get(self, key: str, default: int | None = None, /) -> int | None:
        try:
            return self[key]
        except KeyError:
            return default


class MappingWithInheritance(MyMapping, Mapping[str, int]):
    pass


@Mapping.register  # ty:ignore[unresolved-attribute]
class MappingRegistered(MyMapping):
    pass


def takes_iterable(it: Iterable[str]):
    return it


def takes_mapping(m: Mapping[str, int]):
    return m


# MyMapping is not recognized as a Mapping by both ty and isinstance.
# This is expected, as https://docs.python.org/3/library/collections.abc.html states,
# "Some simple interfaces are directly recognizable by the presence of the required methods."
# "Complex interfaces do not support this last technique because an interface is more than just the presence of method names."
m = MyMapping()
takes_iterable(m)
takes_mapping(m)  # ty:ignore[invalid-argument-type]
assert isinstance(m, Mapping) is False

# As expected, MappingWithInheritance is recognized as a Mapping
# by both ty and isinstance.
mi = MappingWithInheritance()
takes_iterable(mi)
takes_mapping(mi)
assert isinstance(mi, Mapping) is True

# This is the weird one.
# The isinstance check works at runtime, but ty does not recognize it as a Mapping.
mr = MappingRegistered()
takes_iterable(mr)
takes_mapping(mr)  # ty:ignore[invalid-argument-type]
assert isinstance(mr, Mapping) is True
```

Can you improve ty to work with the `.register()` mechanism?

---

_Comment by @AlexWaygood on 2026-01-08 13:25_

I think you're combining one feature request with one bug report here 😄

The bug report is that ty currently emits a false positive `unresolved-attribute` error when accessing the `register` attribute on classes such as `collections.abc.Iterable` and `collections.abc.Mapping`. This is indeed a bug, but it's already covered by https://github.com/astral-sh/ty/issues/1204.

The feature request is for us to actually support `@collections.abc.Mapping.register` properly, such that we would understand classes decorated with that as being subtypes of `collections.abc.Mapping`. I think we're unlikely to add support for that, as no other type checker does, it's inherently somewhat unsound, and I believe one of the key reasons why other type checkers do not support this is because it would come with quite a large performance penalty (it would slow down all subtyping checks).

---

_Comment by @NickCrews on 2026-01-08 17:10_

Thank you for the link. I did search but didn't find that.

> I think we're unlikely to add support for that

Yeah that makes sense how it would be hard to do correctly for th. It might not even be well defined, eg depends on import order, or the .register() could be inside an if block. So that makes sense why you just want to avoid the whole thing.

Soooooo I'm thinking practically here, if we can't adjust ibis.Schema to inherit from collections.abc.Mapping (see above), then is there any way to get it to type check as though it does?
My current understanding is there is not. Maybe have a giant if TYPE_CHECKING block where we define our Iterable, Mapping, Sequence, etc that defines the classes as stubs that actually do inherit from collections.abc?? Gross but possible?

---

_Comment by @carljm on 2026-01-09 01:07_

Yeah, I think your "gross but possible" idea might be the best available option, if you want strong type checking. (The alternatives would probably involve a lot of treating things as `Any`.)


---

_Renamed from "Feat: support `collections.abc.<Class>.register` for more classes" to "Feat: support `collections.abc.<Class>.register`" by @carljm on 2026-01-09 01:08_

---

_Label `wish` added by @carljm on 2026-01-09 01:08_

---
