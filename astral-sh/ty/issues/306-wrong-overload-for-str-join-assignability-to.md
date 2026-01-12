```yaml
number: 306
title: "Wrong overload for `str.join` / assignability to `Iterable[_]`"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - generics
  - type properties
  - Protocols
assignees: []
created_at: 2025-05-10T12:25:07Z
updated_at: 2025-10-08T18:37:32Z
url: https://github.com/astral-sh/ty/issues/306
synced_at: 2026-01-12T15:54:22Z
```

# Wrong overload for `str.join` / assignability to `Iterable[_]`

---

_@sharkdp_

The return type of `str.join` should be `str` here, not `LiteralString`:
```py
from typing import reveal_type

def _(pair: tuple[str, str]):
    reveal_type(", ".join(pair))  # ty: LiteralString
```

Seems related to the fact that we seem to think that `tuple[str, str]` or `Iterable[str]` is assignable to `Iterable[LiteralString]`:
```py
from typing import LiteralString, Iterable

def _(iterable: Iterable[str]):
    x: Iterable[LiteralString] = iterable  # this should be an error
```
https://play.ty.dev/b1a1e84a-6cb9-4b46-89aa-3567a830bad0

---

_Label `bug` added by @sharkdp on 2025-05-10 12:25_

---

_Renamed from "Wrong overload for `str.join`" to "Wrong overload for `str.join` / assignability to `Iterable[_]`" by @sharkdp on 2025-05-10 12:29_

---

_Label `generics` added by @AlexWaygood on 2025-05-10 13:10_

---

_Label `type properties` added by @AlexWaygood on 2025-05-11 07:42_

---

_Comment by @dhruvmanila on 2025-06-17 05:16_

The first example is probably related to https://github.com/astral-sh/ty/issues/665.

---

_Comment by @carljm on 2025-06-17 15:39_

And I suspect the second part is because `Iterable` is a protocol, and currently our protocol support only checks member existence, not member type; @AlexWaygood is working on this.

---

_Comment by @karlicoss on 2025-07-17 00:28_

I have the following example where I'd expect type error, I assume it's the same issue? https://play.ty.dev/5294987b-6776-4cde-9844-bf45e510f59d
```python
from collections.abc import Iterator, Sequence

def gen() -> Iterator[str]:
    yield 123  # expect type error here!

def seq() -> Sequence[str]:
    return [123]  # expect type error here!


a: Iterator[int] = gen()  # expect type error here!

## the types themselves seem to be inferred correctly
reveal_type(gen)
reveal_type(seq())
```

---

_Comment by @jelle-openai on 2025-07-17 00:33_

I'd suspect that's different. The `return [123]` is because ty isn't inferring precise types for list literals; there are open issues about that. The `yield 123` likely means that ty isn't type-checking yields in generators yet. I don't see an open issue about that but [experimenting in the playground](https://play.ty.dev/ffa8537a-9610-4958-91cf-fdf9f1b429ed) gives some TODO types.

---

_Label `Protocols` added by @AlexWaygood on 2025-08-27 11:30_

---

_Closed by @AlexWaygood on 2025-10-08 18:37_

---
