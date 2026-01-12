```yaml
number: 20146
title: "[ty] minor TypedDict fixes"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/typeddict-fixes
created_at: 2025-08-29T02:03:55Z
updated_at: 2025-08-29T16:46:51Z
url: https://github.com/astral-sh/ruff/pull/20146
synced_at: 2026-01-12T15:56:55Z
```

# [ty] minor TypedDict fixes

---

_@carljm_

## Summary

In `is_disjoint_from_impl`, we should unpack type aliases before we check `TypedDict`. This change probably doesn't have any visible effect until we have a more discriminating implementation of disjointness for `TypedDict`, but making the change now can avoid some confusion/bugs in future.

In `type_ordering.rs`, we should order `TypedDict` near more similar types, and leave Union/Intersection together at the end of the list. This is not necessary for correctness, but it's more consistent and it could have saved me some confusion trying to figure out why I was only getting an unreachable panic when my code example included a `TypedDict` type.

## Test Plan

None besides existing tests.

---

_Label `ty` added by @carljm on 2025-08-29 02:03_

---

_Comment by @github-actions[bot] on 2025-08-29 02:05_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-29 02:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-08-29 08:46_

> In `to_meta_type` -- all inhabitants of a `TypedDict` type must be `dict` instances at runtime (not instances of a subclass of `dict`). So the meta type of a `TypedDict` type should be the class `dict`.

Previous discussion on this:
- https://github.com/astral-sh/ruff/pull/19733#discussion_r2252598425
- https://github.com/astral-sh/ruff/pull/19758

If we want to go back on the decisions made there, we no longer need the `Type::dunder_class()` method added in #19758; we can go back to using `Type::to_meta_type()` for the type of `td.__class__` and `type(td)` (where `td` inhabits a `TypedDict` type)

---

_Comment by @sharkdp on 2025-08-29 08:51_

> In `to_meta_type` -- all inhabitants of a `TypedDict` type must be `dict` instances at runtime (not instances of a subclass of `dict`). So the meta type of a `TypedDict` type should be the class `dict`.

I explained [here](https://github.com/astral-sh/ruff/pull/19733#discussion_r2252598425) why I think it's necessary to include information about the actual `TypedDict` type in its meta type. Also note that we do already infer `<class 'dict[str, object]'>` for `type(some_typed_dict)` (see this [MD test](https://github.com/astral-sh/ruff/blob/04dc223710e9dfeb265603b9dcc04c08fd7a3601/crates/ty_python_semantic/resources/mdtest/typed_dict.md?plain=1#L470-L483)).

With the changes in https://github.com/astral-sh/ruff/pull/19758, `type(obj)` is not necessarily equivalent with calling `Type::to_meta_type` on the type of `obj`. See also [this doc comment](https://github.com/astral-sh/ruff/blob/04dc223710e9dfeb265603b9dcc04c08fd7a3601/crates/ty_python_semantic/src/types.rs#L5869-L5870).

(oops, Alex beat me to it)

---

_Comment by @carljm on 2025-08-29 16:10_

Makes sense, thanks for the clarification and sorry for the noise! I've updated the PR to exclude the meta-type change, and instead add a comment to avoid someone making the same mistake as me in future.

---

_Marked ready for review by @carljm on 2025-08-29 16:43_

---

_Review requested from @AlexWaygood by @carljm on 2025-08-29 16:43_

---

_Review requested from @sharkdp by @carljm on 2025-08-29 16:43_

---

_Review requested from @dcreager by @carljm on 2025-08-29 16:43_

---

_@AlexWaygood approved on 2025-08-29 16:45_

---

_Merged by @carljm on 2025-08-29 16:46_

---

_Closed by @carljm on 2025-08-29 16:46_

---

_Branch deleted on 2025-08-29 16:46_

---
