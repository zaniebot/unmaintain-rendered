```yaml
number: 1161
title: "`NamedTuple`-specific methods not part of auto-completions"
type: issue
state: closed
author: sharkdp
labels:
  - good first issue
  - server
  - completions
  - namedtuples
assignees: []
created_at: 2025-09-10T10:01:10Z
updated_at: 2025-11-29T18:37:01Z
url: https://github.com/astral-sh/ty/issues/1161
synced_at: 2026-01-10T01:58:59Z
```

# `NamedTuple`-specific methods not part of auto-completions

---

_Issue opened by @sharkdp on 2025-09-10 10:01_

Start with:

```py
from typing import NamedTuple


class Person(NamedTuple):
    name: str
    age: int | None = None

person = Person("Alice", 25)
```

and then try to auto-complete on `person.<CURSOR>`. This should include methods from [`NamedTupleFallback`](https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/_typeshed/_type_checker_internals.pyi#L55), e.g. `_asdict`, but does not.

I *think* this should be easy to fix by doing something similar to what we do for `TypedDict`s [here](https://github.com/astral-sh/ruff/blob/9cb37db5101771eb08f2e90f5b556b33d73a1308/crates/ty_python_semantic/src/types/ide_support.rs#L102-L112), only with `NamedTuple` and `NamedTupleFallback`.

---

_Label `good first issue` added by @sharkdp on 2025-09-10 10:01_

---

_Label `server` added by @sharkdp on 2025-09-10 10:01_

---

_Closed by @sharkdp on 2025-09-15 09:00_

---

_Label `completions` added by @AlexWaygood on 2025-10-03 12:19_

---

_Label `namedtuples` added by @AlexWaygood on 2025-11-29 18:37_

---
