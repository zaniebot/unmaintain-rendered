```yaml
number: 1239
title: infer type of parameter from super type
type: issue
state: closed
author: KotlinIsland
labels: []
assignees: []
created_at: 2025-09-23T00:39:47Z
updated_at: 2025-09-23T01:51:02Z
url: https://github.com/astral-sh/ty/issues/1239
synced_at: 2026-01-12T15:54:24Z
```

# infer type of parameter from super type

---

_@KotlinIsland_

### Summary

```py
class A:
    def f(self, i: int):
        ...

class B(A):
    def f(self, i):
        reveal_type(i)  # Unknown, expect int
```

here the unannotated `i` can be inferred from the super type as `int`

### Version

_No response_

---

_Closed by @carljm on 2025-09-23 00:42_

---

_Comment by @KotlinIsland on 2025-09-23 01:50_

@carljm oh, sorry. i didn't mean to raise this

i'm not trying to spam you or anything, it was a mistake

---
