```yaml
number: 1871
title: Fix iteration over intersection types
type: issue
state: closed
author: carljm
labels:
  - bug
  - help wanted
  - set-theoretic types
assignees: []
created_at: 2025-12-12T23:13:38Z
updated_at: 2026-01-08T18:57:15Z
url: https://github.com/astral-sh/ty/issues/1871
synced_at: 2026-01-12T15:54:26Z
```

# Fix iteration over intersection types

---

_@carljm_

Something is wrong with our handling of iteration for intersection types:

```py
from typing import Sequence

def _(x: Sequence[int], y: object):
    reveal_type(x)  # `Sequence[int]`
    for item in x:
        reveal_type(item)  # we reveal `int`, which is correct

    if isinstance(y, list):
        reveal_type(y)  # `Top[list[Unknown]]`
        for item in y:
            reveal_type(item)  # we reveal `object`, which is correct

    if isinstance(x, list):
        reveal_type(x)  # `Sequence[int] & Top[list[Unknown]]`
        for item in x:
            reveal_type(item)  # should be `int & object`, which is `int` -- but we say `Never`?
```
https://play.ty.dev/53155022-21ea-4a14-bcb3-df544c07e541

I haven't explored the exact root cause here, but I suspect the proper fix should be simple. We should map over intersections at the top level of iteration handling, so we separately iterate each component type, and intersect the results. That should make the above bug impossible, because it should reduce to doing the first two cases (which we already do correctly) and then intersecting them to get `int & object` -> `int`.

---

_Added to milestone `Stable` by @carljm on 2025-12-12 23:13_

---

_Label `bug` added by @carljm on 2025-12-12 23:13_

---

_Label `set-theoretic types` added by @carljm on 2025-12-12 23:13_

---

_Label `help wanted` added by @carljm on 2025-12-12 23:17_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-13 17:40_

---

_Comment by @mattgiles on 2025-12-13 18:33_

I sometimes see not-iterable complaints like:

```
Object of type `list[Thing | ~str | Unknown]` is not iterable (not-iterable)
```

As in this [example](https://play.ty.dev/f8cf1369-0e71-4034-bfde-8c718941d6eb).

Is my issue related to this issue? If not I'll open a new one.

---

_Unassigned @charliermarsh by @charliermarsh on 2025-12-13 19:19_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-13 19:28_

---

_Comment by @carljm on 2025-12-13 21:31_

@mattgiles that looks like a separate issue from this one -- but definitely looks like a bug 

---

_Comment by @carljm on 2026-01-08 18:57_

This is fixed!

---

_Closed by @carljm on 2026-01-08 18:57_

---
