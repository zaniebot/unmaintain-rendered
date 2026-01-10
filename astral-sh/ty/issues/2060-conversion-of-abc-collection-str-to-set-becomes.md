```yaml
number: 2060
title: "Conversion of abc.Collection[str] to set() becomes set[Unknown] instead of set[str]"
type: issue
state: closed
author: sinon
labels:
  - generics
assignees: []
created_at: 2025-12-18T10:42:19Z
updated_at: 2025-12-24T09:24:35Z
url: https://github.com/astral-sh/ty/issues/2060
synced_at: 2026-01-10T01:56:41Z
```

# Conversion of abc.Collection[str] to set() becomes set[Unknown] instead of set[str]

---

_Issue opened by @sinon on 2025-12-18 10:42_

### Summary

Couldn't find an issue that quite matched (maybe this https://github.com/astral-sh/ty/issues/1473 if so please close this and I will link my suppression comment to reference it instead)

Problem:
Conversion of abc.Collection[str] to set() loses type see following code for example

```py
from collections.abc import Collection
from typing import reveal_type


async def some_fn(
    collect_str: Collection[str] | None = None,
) -> None:
    if collect_str is not None:
        reveal_type(collect_str) # Collection[str]
        set_of_collection = set(collect_str)
        reveal_type(set_of_collection)  # set[Unknown]
```

https://play.ty.dev/08c04eb9-d638-4062-a327-3a75648adcf4

Pyrefly and Pyright both resolves as `set[str]`

### Version

ty 0.0.3 (fadfe0966 2025-12-17)

---

_Renamed from "Conversion of abc.Collection[str] to set() becomes Set[Unknown] instead of Set[str]" to "Conversion of abc.Collection[str] to set() becomes set[Unknown] instead of set[str]" by @sinon on 2025-12-18 10:44_

---

_Label `bug` added by @AlexWaygood on 2025-12-18 11:39_

---

_Comment by @sharkdp on 2025-12-18 12:40_

Thank you for reporting this.

I minified this further to:

```py
from collections.abc import Collection
from typing import reveal_type

def _(collect_str: Collection[str]):
    reveal_type(set(collect_str))  # set[Unknown]
```

Since the `set` constructor is generic (`def __init__(self, iterable: Iterable[_T], /) -> None: ...`), I think this is just an instance of #1714.

---

_Closed by @sharkdp on 2025-12-18 12:40_

---

_Label `bug` removed by @sharkdp on 2025-12-18 12:41_

---

_Label `generics` added by @sharkdp on 2025-12-18 12:41_

---

_Comment by @AlexWaygood on 2025-12-18 12:44_

> Since the `set` constructor is generic (`def __init__(self, iterable: Iterable[_T], /) -> None: ...`), I think this is just an instance of [#1714](https://github.com/astral-sh/ty/issues/1714).

but our existing generics solver _should_ be able to _already_ handle cases where classes _explicitly_ inherit from generic protocols. It's only cases where types are _implicit_ subtypes of generic protocols where there are still TODOs. And `Collection` [_explicitly_ inherits from `Iterable`](https://github.com/python/typeshed/blob/8d96801533918957fb194e101cb321bfe1f836f8/stdlib/typing.pyi#L650-L653). So I think there may be something else going on here.

---

_Reopened by @sharkdp on 2025-12-18 12:51_

---

_Added to milestone `Stable` by @carljm on 2025-12-23 01:13_

---

_Closed by @MichaReiser on 2025-12-23 08:24_

---

_Comment by @sinon on 2025-12-24 09:21_

Not sure if better to open a new issue but the MRE I supplied here turned out not to be quite representative of the `Collection[str]` losing it's type when converted to `set()` in the real-life code I was trying to apply `ty` too. There is a `~AlwaysFalsy` in the mix which seems to disrupt the fix that @ibraheemdev made for the more general case 🤔 

```py
from collections.abc import Collection
from typing import Mapping, reveal_type

def something(mapping: Collection[str] | None = None):
    if not mapping:
        raise ValueError
    reveal_type(mapping)  # Collection[str] & ~AlwaysFalsy
    mapping = set(mapping)
    reveal_type(mapping) # set[Unknown]
```

https://play.ty.dev/f980f01d-288a-48cc-a04a-759f80d74b9d

---

_Comment by @AlexWaygood on 2025-12-24 09:24_

@sinon if you could open a new issue for that, that would be great — thank you!!

---
