```yaml
number: 14524
title: "[red-knot] Properly handle gradual types in subtyping/equivalence relations"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - ty
assignees: []
created_at: 2024-11-22T10:51:32Z
updated_at: 2024-12-04T16:11:26Z
url: https://github.com/astral-sh/ruff/issues/14524
synced_at: 2026-01-10T11:09:56Z
```

# [red-knot] Properly handle gradual types in subtyping/equivalence relations

---

_Issue opened by @sharkdp on 2024-11-22 10:51_

From https://typing.readthedocs.io/en/latest/spec/concepts.html#materialization:

> Since [Any](https://typing.readthedocs.io/en/latest/spec/special-types.html#any) represents an unknown static type, it does not represent any known single set of values (it represents an unknown set of values). Thus it is not in the domain of the subtype, supertype, or equivalence relations on static types described above.

But we currently return true for `Type::Any.is_subtype_of(Type::Any)`.

The reason for that is that `is_subtype_of` relies on `is_equivalent_to`.

And `is_equivalent_to` doesn't model type equivalence for fully static types as defined [here](https://typing.readthedocs.io/en/latest/spec/concepts.html#subtype-supertype-and-type-equivalence).

But rather something (but also not exactly) what would be the [is-consistent-with](https://typing.readthedocs.io/en/latest/spec/concepts.html#summary-of-type-relations) definition for gradual types (for example, we currently have `Any.is_equivalent_to(Any)` but not `Any.is_equivalent_to(Int)`, which would be required for is-consistent-with).

* [x] Fix subtyping for gradual types
* [x]  Clarify what kind of equivalence `is_equivalent_to` implements and potentially make it more consistent with that definition
* [x] Make sure that this is the interpretation of `is_equivalent_to` that we want at all call sites.
* [x] Potentially introduce `is_fully_static`
* [x] Make sure we still handle assignability correctly.
* [x] Make sure we still simplify unions of `Any`.
* [x] Add new property test: non-fully-static types should never participate in subtyping

---

_Label `bug` added by @sharkdp on 2024-11-22 10:51_

---

_Label `red-knot` added by @sharkdp on 2024-11-22 10:51_

---

_Assigned to @sharkdp by @sharkdp on 2024-11-22 10:51_

---

_Closed by @sharkdp on 2024-12-04 16:11_

---

_Closed by @sharkdp on 2024-12-04 16:11_

---
