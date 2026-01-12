```yaml
number: 2300
title: Using the walrus operator together with callable causes TypeIs to be ignored.
type: issue
state: open
author: phi-friday
labels:
  - narrowing
  - callables
assignees: []
created_at: 2026-01-02T05:31:13Z
updated_at: 2026-01-06T01:18:04Z
url: https://github.com/astral-sh/ty/issues/2300
synced_at: 2026-01-12T15:54:26Z
```

# Using the walrus operator together with callable causes TypeIs to be ignored.

---

_@phi-friday_

### Summary

https://play.ty.dev/c4b6196b-5a1c-4f2e-a61c-09b50d38fbc0
```python
from typing import Any, reveal_type
from collections.abc import Callable

class Test:
    func: Callable[..., Any]

foo = Test()
first = getattr(foo, "func", None)
if callable(first):
    reveal_type(first)
if callable(second:=getattr(foo, "func", None)):
    reveal_type(second)
```
```
Revealed type: `Any & Top[(...) -> object]` (revealed-type) [Ln 10, Col 17]
Revealed type: `Any | None` (revealed-type) [Ln 12, Col 17]
```

Even if the ty can't fully infer the type of func, it should still be able to determine whether it is a callable object or not.

If this problem is caused by #626, please close it as a duplicate.

### Version

playground 26230b1ed

---

_Label `narrowing` added by @mtshiba on 2026-01-02 07:31_

---

_Label `callables` added by @mtshiba on 2026-01-02 07:31_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-04 18:26_

---

_Added to milestone `Stable` by @carljm on 2026-01-06 01:18_

---
