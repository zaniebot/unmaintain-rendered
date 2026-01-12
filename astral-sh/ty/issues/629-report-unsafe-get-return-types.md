```yaml
number: 629
title: "report unsafe `__get__` return types"
type: issue
state: open
author: KotlinIsland
labels:
  - typing semantics
  - attribute access
assignees: []
created_at: 2025-06-11T00:58:26Z
updated_at: 2025-11-18T16:10:30Z
url: https://github.com/astral-sh/ty/issues/629
synced_at: 2026-01-12T15:54:23Z
```

# report unsafe `__get__` return types

---

_@KotlinIsland_

### Summary

descriptors can be unsafe:

```py
class A: 
    pass

class B(A):
    def __get__(self, *_) -> 1:
        return 1

class C:
    a: A = B()

C.a  # static: A, runtime: 1
```

we can avoid this by reporting an error on the signature of `__get__`: "`__get__`s return type must be assignable to the super classes `__get__` (defaulting to the type of the superclass in it's absense)"

the same applies to `__set__`

(from #600)

### Version

_No response_

---

_Label `typing semantics` added by @sharkdp on 2025-06-11 06:13_

---

_Label `attribute access` added by @sharkdp on 2025-06-11 06:13_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 15:14_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
