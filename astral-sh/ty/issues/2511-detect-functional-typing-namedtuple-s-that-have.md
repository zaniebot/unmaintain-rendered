```yaml
number: 2511
title: "Detect functional `typing.NamedTuple`s that have fields beginning with underscores"
type: issue
state: open
author: AlexWaygood
labels:
  - runtime semantics
  - namedtuples
assignees: []
created_at: 2026-01-15T12:11:24Z
updated_at: 2026-01-15T12:23:21Z
url: https://github.com/astral-sh/ty/issues/2511
synced_at: 2026-01-15T12:52:53Z
```

# Detect functional `typing.NamedTuple`s that have fields beginning with underscores

---

_@AlexWaygood_

We currently detect that the first two of these classes will lead to an error being raised at runtime, but we do not currently detect that the third of these classes will lead to an error being raised at runtime:

```py
import collections
from typing import NamedTuple

class X(NamedTuple):
    _foo: int

Y = collections.namedtuple("Y", "_x")

Foo = NamedTuple("Foo", [("_x", int), ("_y", str)])
```



---

_Assigned to @charliermarsh by @AlexWaygood on 2026-01-15 12:11_

---

_Label `runtime semantics` added by @AlexWaygood on 2026-01-15 12:11_

---

_Label `namedtuples` added by @AlexWaygood on 2026-01-15 12:23_

---
