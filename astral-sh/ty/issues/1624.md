```yaml
number: 1624
title: use type context to inform default specialization of bare generic class name
type: issue
state: open
author: carljm
labels:
  - generics
  - bidirectional inference
assignees: []
created_at: 2025-11-24T21:02:02Z
updated_at: 2025-11-24T21:02:35Z
url: https://github.com/astral-sh/ty/issues/1624
synced_at: 2026-01-10T01:58:59Z
```

# use type context to inform default specialization of bare generic class name

---

_Issue opened by @carljm on 2025-11-24 21:02_

Example code (derived from [psycopg2](https://github.com/psycopg/psycopg/blob/47b4dec61a8fc0df5559d9037463f945af042796/psycopg/psycopg/connection.py#L78)):

```py
from typing import TypeVar, Generic

Row = TypeVar("Row", default=int)

class Cursor(Generic[Row]):
    x: Row

class Connection(Generic[Row]):
    factory: type[Cursor[Row]]

    def __init__(self):
        reveal_type(Cursor)
        self.factory = Cursor
```

We default-specialize `Cursor` in the last line, leading to treating it as `<class 'Cursor[int]'>`, which is not assignable to `type[Cursor[Row]]` (since `Row` is a typevar in the scope of `class Connection` -- the assignment must be valid for any possible specialization of `Connection`).

Mypy and pyright both allow this assignment, and it seems like they use bidirectional typing (type context) to do so (evidence: making the last line `self.factory = reveal_type(Cursor)` makes it fail in pyright). So perhaps type context should take precedence over typevar defaults when deciding how to implicitly specialize in this case.

---

_Added to milestone `Stable` by @carljm on 2025-11-24 21:02_

---

_Label `generics` added by @carljm on 2025-11-24 21:02_

---

_Label `bidirectional inference` added by @carljm on 2025-11-24 21:02_

---
