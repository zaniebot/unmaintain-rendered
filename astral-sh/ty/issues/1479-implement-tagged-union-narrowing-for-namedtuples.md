```yaml
number: 1479
title: "Implement \"tagged union\" narrowing for namedtuples and arbitrary nominal types"
type: issue
state: open
author: carljm
labels:
  - narrowing
assignees: []
created_at: 2025-11-04T14:49:06Z
updated_at: 2026-01-09T15:53:04Z
url: https://github.com/astral-sh/ty/issues/1479
synced_at: 2026-01-12T15:54:25Z
```

# Implement "tagged union" narrowing for namedtuples and arbitrary nominal types

---

_@carljm_

```py
class A:
    tag: Literal["a"]

class B:
    tag: Literal["b"]

def _(x: A | B):
    if x.tag == "a":
        reveal_type(x)  # revealed: A
    else:
        reveal_type(x)  # revealed: B
```
            

---

_Label `narrowing` added by @carljm on 2025-11-04 14:49_

---

_Label `typeddict` added by @carljm on 2025-11-06 13:57_

---

_Comment by @AlexWaygood on 2025-11-09 17:08_

Mypy refers to this feature as narrowing using "tagged unions"; it's documented [here](https://mypy.readthedocs.io/en/stable/literal_types.html#tagged-unions). The feature is not typeddict-specific; mypy also supports tagged-union narrowing for unions of normal classes:

```py
from dataclasses import dataclass
from typing import Literal

@dataclass
class A:
    tag: Literal["foo"]
    data: list[str]

@dataclass
class B:
    tag: Literal["bar"]
    data: list[str]

def f(obj: A | B):
    if obj.tag == "foo":
        reveal_type(obj)  # revealed: __main__.A
    else:
        reveal_type(obj)  # revealed: __main__.B
```

and even unions of `NamedTuple`s:

```py
from typing import NamedTuple, Literal

class A(NamedTuple):
    tag: Literal["foo"]
    data: list[str]

class B(NamedTuple):
    tag: Literal["bar"]
    data: list[str]

def f(obj: A | B):
    if obj[0] == "foo":
        reveal_type(obj)  # revealed: __main__.A
    else:
        reveal_type(obj)  # revealed: __main__.B
    
    if obj.tag == "foo":
        reveal_type(obj)
    else:
        reveal_type(obj)
```

https://mypy-play.net/?mypy=latest&python=3.12&gist=9cd1b8ea80140517218e175e50bdda70
https://mypy-play.net/?mypy=latest&python=3.12&gist=b90fb9dbe1b184306163181f7b8af867

---

_Label `typeddict` removed by @AlexWaygood on 2025-11-09 17:08_

---

_Renamed from "narrow TypedDicts via tests of an element" to "Implement "tagged union" narrowing for typeddicts and nominal types" by @AlexWaygood on 2025-11-09 17:11_

---

_Renamed from "Implement "tagged union" narrowing for typeddicts and nominal types" to "Implement "tagged union" narrowing for typeddicts, namedtuples and arbitrary nominal types" by @AlexWaygood on 2025-11-09 21:06_

---

_Comment by @sharkdp on 2025-11-10 20:53_

On https://github.com/astral-sh/ruff/pull/21363, we see 2,160 new diagnostics because we now start to understand some large unions in the pydantic code base, such as [this one](https://github.com/pydantic/pydantic/blob/27c95fd56e217f6cfc6ee96e07e1fb0646ef0d49/pydantic-core/python/pydantic_core/core_schema.py#L4106-L4159) (also see the "fun" comment above). And pydantic makes use of this feature a lot (checking for `schema['type']`). It seems slightly more dramatic than it actually is, because we make no attempt at reducing the number of diagnostics on a single line, and we currently emit >50 diagnostics for each "invalid" key access on large unions of `TypedDict`s such as `CoreSchema`.

---

_Comment by @carljm on 2025-11-10 20:58_

It seems like this might not be a big lift to add -- if we synthesize a protocol type to intersect with, the support for testing against attributes should just fall out? And we could synthesize an implicit TypedDict type to intersect with; once our disjointness etc implementation for TypedDict is correct, that should also make this "just work"?

EDIT: One tricky thing might be deciding how correct to be in our handling of equality tests here, since for e.g. `obj.attr = "foo"` technically the protocol type to synthesize would not be a protocol having attribute `attr` of type `Literal["foo"]`, but rather some representation of 'can be equal to `Literal["foo"]`'.

---

_Added to milestone `GA` by @carljm on 2025-11-10 21:01_

---

_Comment by @sharkdp on 2025-11-10 21:04_

> if we synthesize a protocol type to intersect with

Is it possible to formulate this as a (normal) protocol? Both freqtrade and pydantic narrow based on a key, not based on an attribute. Is it enough to add a `def __getitem__(self, key: Literal["special_key"]) -> Any` for these cases (since we already synthesize `__getitem__` methods with literal parameter types for `TypedDict`s)?

---

_Comment by @carljm on 2025-11-10 21:07_

My suggestion above was that we would use `Protocol` for attribute-based tagged narrowing, and implicit TypedDict types for dictionary-style (`__getitem__` based) structural typing, rather than trying to also do the latter through `Protocol`. But it's possible we could make it work via protocol types.

---

_Comment by @AlexWaygood on 2025-11-10 21:12_

there's also some interesting questions around the intersections of `Callable` types... if we have two `TypedDict`s like so:

```py
class TD1(TypedDict):
    x: Literal["foo"]

class TD2(TypedDict):
    x: Literal["bar"]

def f(td: TD1 | TD2):
    if td["x"] == "foo":
        reveal_type(td)
```

inside the `if` branch, it seems appealing to synthesize a protocol that looks like this and intersect with it:

```py
class P(Protocol):
    def __getitem__(self, item: Literal["x"]) -> Literal["foo"]: ...
```

And we'd hope for that intersection to simplify as

```
(TD1 | TD2) & P
=> (TD1 & P) | (TD2 & P)
=> TD1 | Never
=> TD1
```

I think the left-hand side of the union should work out fine: `TD1 & P` should simplify to `TD1`. But I'm not sure `TD2 & P` will simplify to `Never` with our current implementations of intersections for `Callable` types and `Protocol` types. In order for it to simplify to `Never` naturally, you'd have to have `<Callable` type of TD2.__getitem__> & <Callable type of P.__getitem__>` simplify to `Never`. But I think in fact, it works like this:

```
CallableTypeOf[TD2.__getitem__] & CallableTypeOf[P.__getitem__]
=> ((self, value: Literal["x"], /) -> Literal["bar"]) & ((self, value: Literal["x"], /) -> Literal["foo"])
=> (self, value: Literal["x"], /) -> Never
```

---

_Comment by @carljm on 2025-11-10 21:30_

Yes, this kind of thing is why I think it might be simpler to use synthesized TypedDict types for key-based narrowing, and rely on dedicated special-cased handling of TypedDict types? Fully implementing all structural typing (including `TypedDict` itself??) via `Protocol` feels like it ought to be possible, but there are likely many dragons.

---

_Comment by @AlexWaygood on 2025-11-10 21:31_

> Fully implementing all structural typing (including `TypedDict` itself??) via `Protocol` feels like it ought to be possible, but there are likely many dragons.

100%

---

_Comment by @mtshiba on 2025-11-15 05:54_

I've tackled a similar problem before (https://github.com/astral-sh/ruff/pull/19064), and the conclusion was that we needed to check the `Protocol` property members. A `Protocol` that simply has an attribute of a certain type cannot have covariance.

---

_Assigned to @oconnor663 by @oconnor663 on 2025-12-04 22:35_

---

_Comment by @oconnor663 on 2025-12-23 19:37_

Tagged `TypedDict` narrowing is now supported. For example:

```py
from typing import TypedDict, Literal, reveal_type

class Foo(TypedDict):
    tag: Literal["foo"]

class Bar(TypedDict):
    tag: Literal[42]

class Baz(TypedDict):
    tag: Literal[b"baz"]  # `BytesLiteral`

class Bing(TypedDict):
    tag: Literal["bing"]

def _(u: Foo | Bar | Baz | Bing):
    if u["tag"] == "foo":
        reveal_type(u)  # revealed: Foo
    elif u["tag"] == 42:
        reveal_type(u)  # revealed: Bar
    elif u["tag"] == b"baz":
        reveal_type(u)  # revealed: Baz
    else:
        reveal_type(u)  # revealed: Bing
```

---

_Comment by @carljm on 2025-12-23 19:43_

Filed https://github.com/astral-sh/ty/issues/2192 as follow-up to support enum-literal discriminators.

Will keep this issue open for namedtuples and nominal instances. (Could split those out except I think it's possible the implementation might be largely shared?)

---

_Renamed from "Implement "tagged union" narrowing for typeddicts, namedtuples and arbitrary nominal types" to "Implement "tagged union" narrowing for namedtuples and arbitrary nominal types" by @carljm on 2025-12-23 19:43_

---

_Unassigned @oconnor663 by @oconnor663 on 2026-01-09 15:53_

---
