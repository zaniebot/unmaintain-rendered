```yaml
number: 358
title: "`map` class onto iterable rejected"
type: issue
state: closed
author: acushner-rippling
labels:
  - type properties
assignees: []
created_at: 2025-05-13T14:47:33Z
updated_at: 2025-05-28T20:43:08Z
url: https://github.com/astral-sh/ty/issues/358
synced_at: 2026-01-12T15:54:23Z
```

# `map` class onto iterable rejected

---

_@acushner-rippling_

### Summary

the below `map` gives ```No overload of function `__new__` matches arguments (no-matching-overload) [Ln 6, Col 7]```

```python
class Jeb:
    def __init__(self, blah):
        self.blah = blah

nums = [1, 2]
res = map(Jeb, nums)
```

https://play.ty.dev/8bb3121d-3da3-4449-a1a4-0095409fa3f1

### Version

_No response_

---

_Label `generics` added by @AlexWaygood on 2025-05-13 15:40_

---

_Comment by @carljm on 2025-05-13 16:12_

Thanks for the report! This will be fixed by https://github.com/astral-sh/ruff/pull/17638

---

_Label `generics` removed by @carljm on 2025-05-13 16:15_

---

_Label `type properties` added by @AlexWaygood on 2025-05-13 18:05_

---

_Closed by @carljm on 2025-05-28 20:43_

---

_Closed by @carljm on 2025-05-28 20:43_

---
