```yaml
number: 105
title: Support gradual equivalence for an overloaded callable
type: issue
state: closed
author: dhruvmanila
labels:
  - overloads
  - type properties
assignees: []
created_at: 2025-05-01T19:32:32Z
updated_at: 2025-08-08T13:54:39Z
url: https://github.com/astral-sh/ty/issues/105
synced_at: 2026-01-10T02:06:24Z
```

# Support gradual equivalence for an overloaded callable

---

_Issue opened by @dhruvmanila on 2025-05-01 19:32_

The task here is to resolve the following TODO and add support for `is_gradual_equivalent_to` for an overloaded callable. Note that this relation is supported when both types are non-overloaded callable i.e., a callable type with one signature.

https://github.com/astral-sh/ruff/blob/75effb8ed7430288648eb616b1499939700edff6/crates/red_knot_python_semantic/src/types.rs#L7216-L7229

### Notes

Currently, the equivalence relation between two, possible overloaded, callable types is implemented based on the definition of [equivalence in the typing spec](https://typing.python.org/en/latest/spec/concepts.html#subtype-supertype-and-type-equivalence) and delegates it to the subtyping check:

> We also define an **equivalence** relation on fully static types: the types `A` and `B` are equivalent (or “the same type”) if and only if `A` is a subtype of `B` and `B` is a subtype of `A`.

Implementation of equivalence check between two callable type:

https://github.com/astral-sh/ruff/blob/75effb8ed7430288648eb616b1499939700edff6/crates/red_knot_python_semantic/src/types.rs#L7190-L7214

I had a discussion with Carl internally and we think that the gradual equivalence check could potentially be implemented using a similar approach instead of implementing it from scratch as it might be complicated to implement the check from scratch.

The way this could work is by using some kind of "strategy" parameter that the `CallableType::is_subtype_of` accepts which would have two variants to indicate that the subtype check between two parameter type is being done to check either (1) gradual equivalence between two callable types or (2) subtyping between two callable types.

In terms of implementation, this could look like:
* Update `CallableType::is_gradual_equivalent_to` to delegate it to `is_subtype_of` when either of both types are overloaded
* Update `CallableType::is_subtype_of` to accept an enum parameter with two variants
* Pass this parameter to `Signature::is_subtype_of`
* The inner close in `Signature::is_subtype_of` that checks the relation between two types will take either of the two approach:
  1. If the main check is `is_subtype_of`, the logic that exists currently will remain as is 
  2. If the main check is `is_gradual_equivalent_to`, if both types are fully static then call into `Type::is_subtype_of` otherwise use `Type::is_gradual_equivalent_to`

Both (1) and (2) above would be represented using a two variant enum parameter that I talked about earlier.

---

_Renamed from "[red-knot] Support gradual equivalence for an overloaded callable" to "Support gradual equivalence for an overloaded callable" by @MichaReiser on 2025-05-07 15:24_

---

_Label `overloads` added by @AlexWaygood on 2025-05-10 18:02_

---

_Label `type properties` added by @AlexWaygood on 2025-05-11 07:40_

---

_Removed from milestone `Beta` by @carljm on 2025-06-11 01:03_

---

_Added to milestone `GA` by @carljm on 2025-06-11 01:03_

---

_Comment by @dhruvmanila on 2025-08-08 13:54_

This isn't required after astral-sh/ruff#18799 as the `is_gradual_equivalent_to` check has been removed.

---

_Closed by @dhruvmanila on 2025-08-08 13:54_

---
