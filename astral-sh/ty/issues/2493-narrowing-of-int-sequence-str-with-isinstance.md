```yaml
number: 2493
title: "Narrowing of `int | Sequence[str]` with `isinstance(Sequence)` is not Sequence[str]"
type: issue
state: closed
author: henriquegemignani
labels: []
assignees: []
created_at: 2026-01-14T14:29:25Z
updated_at: 2026-01-14T15:24:03Z
url: https://github.com/astral-sh/ty/issues/2493
synced_at: 2026-01-14T15:39:25Z
```

# Narrowing of `int | Sequence[str]` with `isinstance(Sequence)` is not Sequence[str]

---

_@henriquegemignani_

### Summary

Playground: https://play.ty.dev/6143bcb7-6aa9-405d-ba62-33a4ea7e764d

Code:
```python
from collections.abc import Sequence

type JsonType_RO = int | Sequence[str]


def encode_a(value: JsonType_RO) -> None:
    if isinstance(value, Sequence):
        sequence_value: Sequence[str] = value

def encode_b(value: JsonType_RO) -> None:
    if isinstance(value, Sequence[str]):
        sequence_value: Sequence[str] = value
```
Errors:
```
Object of type `(int & Sequence[object]) | Sequence[str]` is not assignable to `Sequence[str]` (invalid-assignment) [Ln 8, Col 41]
Object of type `JsonType_RO` is not assignable to `Sequence[str]` (invalid-assignment) [Ln 12, Col 41]
```

The main case is `encode_a`. I'd expect `value` to be narrowed to `Sequence[str]`, but that's not the case. As far as I could find, that should be valid typing. I tried to find some related issue, but no luck with search.

`encode_b` is just an attempt to investigate further, but it's interesting how that causes ty to not narrow at all.

### Version

ty 0.0.11 (830cb9cc6 2026-01-09)

---

_Comment by @AlexWaygood on 2026-01-14 14:33_

Thanks! This is basically the same as #1578. ty is warning you here that it's entirely possible for an object to be an instance of both `int` and `Sequence` simultaneously: they're not disjoint types:

```py
>>> from typing import Sequence
>>> class Foo(int, Sequence): ...
... 
>>>
```

You can fix the diagnostic here by using `if not isinstance(value, int)` instead of `if isinstance(value, Sequence)`.

---

_Closed by @AlexWaygood on 2026-01-14 14:33_

---

_Comment by @henriquegemignani on 2026-01-14 15:13_

Is there a way to annotate that such subclass of `int` is not allowed? In the actual use in my code base it does already `if isinstance(value, int)`, but the actual type is `int | Mapping[str, JsonType_RO] | Sequence[JsonType_RO]` and adding a `not isinstance(value, Mapping)` doesn't work. 

---

_Comment by @AlexWaygood on 2026-01-14 15:23_

> Is there a way to annotate that such subclass of `int` is not allowed?

No, Python's type system doesn't currently support a way to annotate "exact types", and we don't have any extensions to the type system for that currently in ty. Such types would unfortunately not be very useful unless you used them _everywhere_, but in most cases it isn't desirable to distinguish between "any instance of `T`", and "any object that is an instance of _exactly_ `T`". So I think the use cases for types like this would actually turn out to be pretty limited, unfortunately.

If you're willing to use the `ty_extensions` module, you could do something like this:

```py
from ty_extensions import Intersection, Not
from collections.abc import Sequence

type JsonType_RO = Intersection[int, Not[Sequence[object]]] | Sequence[str]


def encode_a(value: JsonType_RO) -> None:
    if isinstance(value, Sequence):
        sequence_value: Sequence[str] = value
```

But note that the ty_extensions module doesn't actually exist at runtime right now; you need to import it in an `if TYPE_CHECKING` block if you want to use it in a `.py` file.

I'm curious to know more about this, though:

> adding a `not isinstance(value, Mapping)` doesn't work.

Why doesn't that work in this case for you? Can you give another minimal example? :-)

---
