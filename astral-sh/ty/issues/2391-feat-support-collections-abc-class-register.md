```yaml
number: 2391
title: "Feat: support `collections.abc.<Class>.register`"
type: issue
state: open
author: NickCrews
labels:
  - wish
assignees: []
created_at: 2026-01-08T04:07:59Z
updated_at: 2026-01-10T18:54:43Z
url: https://github.com/astral-sh/ty/issues/2391
synced_at: 2026-01-12T02:26:12Z
```

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

_Comment by @NickCrews on 2026-01-10 17:54_

How difficult would it be for ty to special case the usage of collections.abc.Mapping.register as a decorator, right above the class definition? 
The implementation might be tricky, but at least I think the result would be well defined, eg avoid the import order/conditional pitfalls I describe above.

I'm not sure of the internals of how ty works, but if it uses a lot of the same core code as ruff, I was thinking that perhaps you already had access to the info to be able to answer the question "is this class definition decorated with an abc.ABC.register"?

If you special cased this one usage pattern, it would cover some fraction of use cases, idk I'm totally guessing here but maybe 30-80 percent of usages (I can do a more thorough grep over GitHub to get a more precise answer if this is important to you).

I realize how unappealing it is to add these special cases to your codebase, so no pressure :)

---

_Comment by @NickCrews on 2026-01-10 18:06_

For example, I'm guessing you want to/already support the @dataclass decorator to transform a class, so maybe it wouldn't be too bad to piggy back on that infrastructure?

---

_Comment by @AlexWaygood on 2026-01-10 18:18_

Has ibis ever run any other type checkers in CI before? As I say, to my knowledge this isn't supported by any other type checker (am I wrong?).

It's not impossible that we could support this, but it's still likely to be low-priority if we'd be the first type checker to do so. And we'd need to do some design work to think about how to make it sound. For example, we'd need to emit a diagnostic if `@collections.abc.Mapping.register` was used to decorate a class `C` that did not have all methods and attributes available on `collections.abc.Mapping` (or if any attribute on `C` did not have a compatible type with the attribute on `Mapping`). 

---

_Comment by @NickCrews on 2026-01-10 18:54_

Ibis has never used type checkers before. I'm not even really trying to convert ibis to completely pass a type checker at this point, that is going to be too big of a task. I am running into this type error when using ibis in an app, the app is using ty. So I'm just trying to upstream a few of the best fixes so that my app sees fewer errors that it needs to ignore.

I don't know of any other type checker that supports this (but I'm not familiar with them).

Yes, those concerns also make sense. I won't be expecting anything. Feel free to close if you want to clean up your issues. Thanks!

---
