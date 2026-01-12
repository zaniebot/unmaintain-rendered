```yaml
number: 720
title: "Nontransitive subtyping involving protocols and `object`"
type: issue
state: closed
author: JelleZijlstra
labels:
  - bug
  - type properties
  - Protocols
assignees: []
created_at: 2025-06-28T15:42:35Z
updated_at: 2025-09-10T11:13:19Z
url: https://github.com/astral-sh/ty/issues/720
synced_at: 2026-01-12T15:54:23Z
```

# Nontransitive subtyping involving protocols and `object`

---

_@JelleZijlstra_

### Summary

ty currently fails the last assert in this file:

```python
from ty_extensions import static_assert, is_subtype_of
from typing import Protocol

class P(Protocol):
    def __str__(self) -> str: ...

class P2(Protocol):
    x: int

static_assert(is_subtype_of(object, P))
static_assert(is_subtype_of(P2, object))
static_assert(is_subtype_of(P2, P))
```
https://play.ty.dev/23be40f7-3ebf-4785-991b-29c7508b5e22

That is, it accepts that `P2 <: object` and `object <: P`, but not that `P2 <: P`.

ty has [a property test](https://github.com/astral-sh/ruff/blob/c5995c40d3714b3d532864b3d45adc5c88ce7b24/crates/ty_python_semantic/src/types/property_tests.rs#L93) asserting that subtyping is transitive, but it apparently didn't catch this case.

Transitivity of subtyping is important for making union simplification deterministic, but it seems that is not an issue in practice here, because `P` is also a subtype of `object` (in the other direction) and ty prefers simplifying to `object`.

I think this is more of a curiosity than a real bug that can break soundness, but reporting it anyway in case it's more serious than I thought.

### Version

_No response_

---

_Comment by @AlexWaygood on 2025-06-28 15:53_

> ty has [a property test](https://github.com/astral-sh/ruff/blob/c5995c40d3714b3d532864b3d45adc5c88ce7b24/crates/ty_python_semantic/src/types/property_tests.rs#L93) asserting that subtyping is transitive, but it apparently didn't catch this case.

yes, we don't generate any protocol types to throw at our property tests yet, because our subtyping/assignability implementation for protocols is very much unfinished. I expect there are probably quite a few more issues like this you'd be able to find if you put your mind to it ;-) My plan had been to add protocols to our property tests once we were a bit further along on our protocols implementation.

Progress on protocols has been a bit slower than expected because it turns out that as soon as you start naively poking at the types of a protocol's members to do a subtype check, you end up with stack overflows if that protocol's members contain (directly or indirectly) a recursive reference back to the original protocol... and protocols like that are surprisingly common!

---

_Label `bug` added by @AlexWaygood on 2025-06-28 16:03_

---

_Label `type properties` added by @AlexWaygood on 2025-06-28 16:03_

---

_Label `Protocols` added by @AlexWaygood on 2025-06-28 16:03_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-06-29 12:43_

---

_Added to milestone `Beta` by @carljm on 2025-08-05 14:54_

---

_Comment by @AlexWaygood on 2025-09-10 11:13_

This was fixed in https://github.com/astral-sh/ruff/pull/20284

---

_Closed by @AlexWaygood on 2025-09-10 11:13_

---
