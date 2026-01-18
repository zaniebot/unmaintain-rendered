```yaml
number: 2128
title: "`Literal[\"abba\"]` should be a subtype of `Sequence[Literal[\"a\", \"b\"]]`"
type: issue
state: closed
author: carljm
labels:
  - good first issue
  - type properties
assignees: []
created_at: 2025-12-20T02:07:10Z
updated_at: 2026-01-18T17:43:45Z
url: https://github.com/astral-sh/ty/issues/2128
synced_at: 2026-01-18T18:15:23Z
```

# `Literal["abba"]` should be a subtype of `Sequence[Literal["a", "b"]]`

---

_@carljm_

A string literal containing only characters from a given set should be a subtype of `Sequence` of the union of those literal characters.

```py
from collections.abc import Sequence
from typing import Literal

def func(tags: Sequence[Literal["a", "b"]]) -> None:
    pass

func("abba")
```

https://play.ty.dev/381e87b8-2438-4287-a1cd-2e26c04a6dd1

---

_Label `good first issue` added by @carljm on 2025-12-20 02:07_

---

_Label `type properties` added by @carljm on 2025-12-20 02:07_

---

_Comment by @tchan102 on 2025-12-20 03:30_

I’d like to take a stab at this if no one else is working on it.

My current thinking is to handle this as a literal-only case: when we see a string literal,
check whether all of its characters are within the allowed `Literal[...]` set and allow it
as a `Sequence[Literal[...]]` if so. I’ll avoid applying this to non-literal `str` values and
add tests to cover edge cases.

---

_Comment by @carljm on 2025-12-20 16:34_

Sounds great! Yes I think this can be handled as a special match branch in `Type::has_relation_to_impl`.

Probably rather than special-casing the RHS type, it will be simpler and more general to just attempt a fallback from LHS type of `Literal["whatever"]` to `Sequence[Literal["w", "h", "a", "t", "e", "v", "r"]]` and re-try the `has_relation_to_impl` with that as LHS type.

---

_Closed by @AlexWaygood on 2026-01-18 17:43_

---
