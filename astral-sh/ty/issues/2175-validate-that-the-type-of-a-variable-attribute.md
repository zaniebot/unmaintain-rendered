```yaml
number: 2175
title: Validate that the type of a variable/attribute/subscript remains consistent with its declared type following an augmented assignment operation
type: issue
state: open
author: MeGaGiGaGon
labels: []
assignees: []
created_at: 2025-12-23T00:47:29Z
updated_at: 2025-12-23T11:05:18Z
url: https://github.com/astral-sh/ty/issues/2175
synced_at: 2026-01-12T15:54:26Z
```

# Validate that the type of a variable/attribute/subscript remains consistent with its declared type following an augmented assignment operation

---

_@MeGaGiGaGon_

### Summary

Hopefully this is a new issue, couldn't find anything on it searching for `iadd` or `inplace`
https://play.ty.dev/b3f4aadb-fd45-4ba2-99d2-5ad1475ef3ef
```py
from typing import reveal_type

class A:
    def __add__(self, other: object) -> object:
        return other

class B:
    def __init__(self, x: A):
        self.x: A = x

c = B(A())
reveal_type(c.x)  # Revealed type: `A`
c.x += 1  # Should be an error
reveal_type(c.x)  # Revealed type: `object` :(
c.x = object()  # But a normal assignment correctly still fails
```

### Version

ty 0.0.5 (d37b7dbd9 2025-12-20) playground 664686bdb

---

_Comment by @carljm on 2025-12-23 01:11_

Nice catch! Definitely a missing validation there.

---

_Added to milestone `Stable` by @carljm on 2025-12-23 01:12_

---

_Renamed from "Generated inplace binary operations allow for incorrect assignments" to "Validate that the type of a variable/attribute/subscript remains consistent with its declared type following an augmented assignment operation" by @AlexWaygood on 2025-12-23 11:05_

---
