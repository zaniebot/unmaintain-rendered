```yaml
number: 2512
title: Support generic functional NamedTuples/TypedDicts
type: issue
state: open
author: AlexWaygood
labels:
  - typing semantics
  - typeddict
  - namedtuples
assignees: []
created_at: 2026-01-15T12:14:38Z
updated_at: 2026-01-15T15:33:53Z
url: https://github.com/astral-sh/ty/issues/2512
synced_at: 2026-01-15T15:50:04Z
```

# Support generic functional NamedTuples/TypedDicts

---

_@AlexWaygood_

Mypy supports defining generic functional `NamedTuple`s and `TypedDict`s. Ideally we would too:

```py
from typing import NamedTuple, TypeVar, TypedDict

T = TypeVar("T")

GenericPoint = NamedTuple("GenericPoint", [("x", T), ("y", T)])
GenericMovie = TypedDict("GenericMovie", {"director": str, "assistant producer": T})

def _(p: GenericPoint[int], m: GenericMovie[str]):
    reveal_type(p[0])  # int
    reveal_type(p[1])  # int
    reveal_type(p.x)  # int
    reveal_type(p.y)  # int
    
    reveal_type(m["assistant producer"])  # str
```

This is not _very_ important for `NamedTuple`s, because there's not really any reason to define `GenericPoint` using the functional syntax rather than using a `class` statement. It's more important for `TypedDict`s, however, because it's impossible to rewrite `GenericMovie` using a `class` statement: one of its keys is not a valid identifier.

Failing tests for the `NamedTuple` part of this were added in https://github.com/astral-sh/ruff/commit/3e0299488e72fa80f59473565cf724041d0f3f3c.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2026-01-15 12:14_

---

_Assigned to @charliermarsh by @AlexWaygood on 2026-01-15 12:14_

---

_Label `typing semantics` added by @AlexWaygood on 2026-01-15 12:14_

---

_Label `typeddict` added by @AlexWaygood on 2026-01-15 12:14_

---

_Label `namedtuples` added by @AlexWaygood on 2026-01-15 12:14_

---

_Comment by @AlexWaygood on 2026-01-15 12:18_

(We don't support functional TypedDicts at all yet, but I believe we will do soon!)

---

_Added to milestone `Stable` by @carljm on 2026-01-15 15:33_

---
