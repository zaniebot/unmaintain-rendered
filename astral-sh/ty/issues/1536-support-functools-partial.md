```yaml
number: 1536
title: Support functools.partial
type: issue
state: open
author: carljm
labels: []
assignees: []
created_at: 2025-11-13T00:24:01Z
updated_at: 2025-11-13T00:24:01Z
url: https://github.com/astral-sh/ty/issues/1536
synced_at: 2026-01-12T15:54:25Z
```

# Support functools.partial

---

_@carljm_

The type stubs for `partial` can't describe its behavior in a useful way, so type checkers special case it. We should too, so that the return type of `partial` tracks the full underlying signature and the partial arguments provided, and we can later combine the partial arguments with newly-supplied arguments and check them against the wrapped signature. For example:

```py
from functools import partial

def f(a: int, b: str) -> bool: ...

g = partial(f, 1)

g(1, "foo") # error: too many arguments
g(1) # error: int not assignable to str
g("foo") # good
```

---

_Added to milestone `Stable` by @carljm on 2025-11-13 00:24_

---
