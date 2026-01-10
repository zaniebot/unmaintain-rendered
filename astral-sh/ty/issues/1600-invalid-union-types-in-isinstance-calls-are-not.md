```yaml
number: 1600
title: "Invalid union types in `isinstance()` calls are not detected when nested in tuples"
type: issue
state: open
author: AlexWaygood
labels:
  - runtime semantics
assignees: []
created_at: 2025-11-20T12:08:40Z
updated_at: 2025-11-21T00:08:24Z
url: https://github.com/astral-sh/ty/issues/1600
synced_at: 2026-01-10T01:58:59Z
```

# Invalid union types in `isinstance()` calls are not detected when nested in tuples

---

_Issue opened by @AlexWaygood on 2025-11-20 12:08_

### Summary

We currently emit a diagnostic when an invalid `types.UnionType` instance is used as the second argument to `isinstance()` or `issubclass()`. But we do not currently detect this if the `types.UnionType` instance is nested inside a tuple.

It's probably not high-priority to fix this because I'm not sure why you'd write code like this, but it's a known edge case that we don't handle currently, so it seemed worth opening an issue about.

```py
from typing import Literal

def f(x):
    if isinstance(x, str | Literal[42]):  # diagnostic (good! this fails at runtime)
        reveal_type(x)

    if isinstance(x, (int, str | Literal[42])):  # no diagnostic
        reveal_type(x)  # revealed: Unknown

    if isinstance(x, (int, (str, (bytes, memoryview | Literal[42])))):  # no diagnostic
        reveal_type(x)  # revealed: Unknown
```

At runtime:

```pycon
Python 3.13.1 (main, Jan  3 2025, 12:04:03) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from typing import Literal
>>> if isinstance(42, (bytes, str | int)):
...     print('got you')
...     
got you
>>> if isinstance(42, (bytes, str | Literal[42])):
...     print('got you')
...     
Traceback (most recent call last):
  File "<python-input-3>", line 1, in <module>
    if isinstance(42, (bytes, str | Literal[42])):
       ~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 1786, in __instancecheck__
    return self.__subclasscheck__(type(obj))
           ~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 1790, in __subclasscheck__
    if issubclass(cls, arg):
       ~~~~~~~~~~^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 1378, in __subclasscheck__
    raise TypeError("Subscripted generics cannot be used with"
                    " class and instance checks")
TypeError: Subscripted generics cannot be used with class and instance checks
```

### Version

_No response_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-11-20 12:08_

---

_Comment by @carljm on 2025-11-20 15:38_

Thanks. Yeah, I had it on my list to reopen https://github.com/astral-sh/ty/issues/620, since TypeAlias support doesn't fix the given example there. But this issue works just as well!

---

_Added to milestone `Stable` by @carljm on 2025-11-20 15:38_

---

_Comment by @AlexWaygood on 2025-11-20 15:46_

> Thanks. Yeah, I had it on my list to reopen [#620](https://github.com/astral-sh/ty/issues/620), since TypeAlias support doesn't fix the given example there. But this issue works just as well!

Hmm, I think that's actually a separate issue relating to us not understanding recursive type aliases properly yet? Currently we infer the type of `isinstance()` as being

```
def isinstance(obj: object, class_or_tuple: type | UnionType | tuple[Divergent, ...], /) -> bool
```

But ideally we would understand [this recursive PEP-613 type alias](https://github.com/astral-sh/ruff/blob/416e2267daaf9699c26ec73c644a03356e3de7f2/crates/ty_vendored/vendor/typeshed/stdlib/builtins.pyi#L3736-L3739), and that would lead us to flag `isinstance(42, (None,))` calls because `tuple[None]` is not assignable to `tuple[_ClassInfo, ...]` (`None` is not an instance of `type`/`UnionType` or a tuple thereof.)

This issue is about something different (and it's lower-priority than the one you're talking about): even once we understand recursive type aliases, we _still_ wouldn't flag the example I gave above, because `str | Literal[42]` _is_ a `types.UnionType` instance (and therefore permitted according to typeshed's recursive type alias), it's just a `types.UnionType` instance with invalid elements in it.

---

_Comment by @carljm on 2025-11-21 00:00_

I see - the diagnostic you're talking about here is not a failing assignability check to the parameter type in typeshed, it's an additional diagnostic we offer when special-casing `isinstance` calls, to mention if the PEP 604 union has invalid elements in it. So it's checking something that isn't expressible in the typeshed annotations, since `UnionType` doesn't express anything about the types of its elements. And that additional check doesn't recursively descend into tuples.

I'll probably just open a separate issue for recursive PEP 613 type aliases, with `isinstance` as an example case.

---

_Removed from milestone `Stable` by @AlexWaygood on 2025-11-21 00:08_

---
