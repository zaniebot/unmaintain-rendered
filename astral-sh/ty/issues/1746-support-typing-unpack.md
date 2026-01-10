```yaml
number: 1746
title: "Support `typing.Unpack`"
type: issue
state: open
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-12-03T19:43:52Z
updated_at: 2025-12-11T10:43:40Z
url: https://github.com/astral-sh/ty/issues/1746
synced_at: 2026-01-10T01:56:41Z
```

# Support `typing.Unpack`

---

_Issue opened by @carljm on 2025-12-03 19:43_

_No description provided._

---

_Added to milestone `Stable` by @carljm on 2025-12-03 19:43_

---

_Label `typing semantics` added by @carljm on 2025-12-03 19:43_

---

_Comment by @dhruvmanila on 2025-12-05 07:20_

(Going to assign myself to this as I talked about implementing this earlier.)

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-12-05 07:20_

---

_Comment by @gjcarneiro on 2025-12-11 10:43_

Here's an example that I would love to have Ty support correctly (mypy validates this).

```py
from typing import TypedDict, Unpack, reveal_type, override


class Params(TypedDict):
    user: str
    request: object


class Foo:
    def foo(self, **kwargs: Unpack[Params]) -> None:
        pass


class Bar(Foo):
    @override
    def foo(self, *, user: str, **_: object) -> None:
        reveal_type(user)


class Zbr(Foo):
    @override
    def foo(self, *, request: object, **_: object) -> None:
        reveal_type(request)
```

---
