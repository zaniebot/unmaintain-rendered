```yaml
number: 20512
title: "FA102 is missing `collection.abc` types"
type: issue
state: closed
author: Andrej730
labels:
  - rule
  - preview
assignees: []
created_at: 2025-09-22T09:34:56Z
updated_at: 2025-10-03T13:45:34Z
url: https://github.com/astral-sh/ruff/issues/20512
synced_at: 2026-01-10T11:09:59Z
```

# FA102 is missing `collection.abc` types

---

_Issue opened by @Andrej730 on 2025-09-22 09:34_

### Summary

E.g. use of `Sequence[int]` is not flagged. 
See example below (command I've used is `ruff check test.py --target-version py38 --select=FA102`)

Ruff playground - https://play.ruff.rs/b295c8a4-4295-4a5a-8a2d-e99699814822
```python
from collections.abc import Sequence

# Not reported by Ruff, but results in runtime error:
# TypeError: 'ABCMeta' object is not subscriptable
def test() -> Sequence[int]:
    return [1, 2, 3]


# FA102 Missing `from __future__ import annotations`, but uses PEP 585 collection
def test2() -> list[int]:
    return [1, 2, 3]
```

### Version

ruff 0.13.1

---

_Comment by @ntBre on 2025-09-22 14:49_

This makes sense to me, but it will have to be a preview change since it expands the scope of a stable rule.

---

_Label `rule` added by @ntBre on 2025-09-22 14:49_

---

_Label `preview` added by @ntBre on 2025-09-22 14:49_

---

_Closed by @ntBre on 2025-10-03 13:45_

---
