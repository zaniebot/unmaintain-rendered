```yaml
number: 18260
title: Fix instance vs callable subtyping/assignability
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/callable-instances
created_at: 2025-05-22T18:40:56Z
updated_at: 2025-05-27T19:04:47Z
url: https://github.com/astral-sh/ruff/pull/18260
synced_at: 2026-01-12T15:56:15Z
```

# Fix instance vs callable subtyping/assignability

---

_@carljm_

## Summary

Fix some issues with subtying/assignability for instances vs callables. We need to look up dunders on the class, not the instance, and we should limit our logic here to delegating to the type of `__call__`, so it doesn't get out of sync with the calls we allow.

Also, we were just entirely missing assignability handling for `__call__` implemented as anything other than a normal bound method (though we had it for subtyping.)

A first step towards considering what else we want to change in https://github.com/astral-sh/ty/issues/491

## Test Plan

mdtests


---

_Review requested from @AlexWaygood by @carljm on 2025-05-22 18:40_

---

_Review requested from @sharkdp by @carljm on 2025-05-22 18:40_

---

_Review requested from @dcreager by @carljm on 2025-05-22 18:40_

---

_Comment by @github-actions[bot] on 2025-05-22 18:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
strawberry (https://github.com/strawberry-graphql/strawberry)
- error[invalid-assignment] strawberry/federation/scalar.py:133:9: Object of type `_T | None` is not assignable to `((...) -> Unknown) | None`
- error[invalid-argument-type] strawberry/types/scalar.py:110:29: Argument to bound method `__init__` is incorrect: Expected `(Any, /) -> Any`, found `_T`
- error[invalid-assignment] strawberry/types/scalar.py:209:9: Object of type `_T | None` is not assignable to `((...) -> Unknown) | None`
- Found 436 diagnostics
+ Found 433 diagnostics

jinja (https://github.com/pallets/jinja)
- error[invalid-argument-type] src/jinja2/ext.py:168:27: Argument to bound method `call` is incorrect: Expected `(...) -> Any`, found `Any | Undefined`
- Found 234 diagnostics
+ Found 233 diagnostics

```
</details>


---

_Label `ty` added by @MichaReiser on 2025-05-22 18:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:677 on 2025-05-22 19:00_

```suggestion
An instance type is assignable to a compatible callable type if the instance type's class has a callable `__call__` attribute.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1162 on 2025-05-22 19:05_

```suggestion
An instance type can be a subtype of a compatible callable type if the instance type's class has a callable `__call__` attribute.
```

---

_@AlexWaygood approved on 2025-05-22 19:06_

---

_Merged by @carljm on 2025-05-22 19:47_

---

_Closed by @carljm on 2025-05-22 19:47_

---

_Branch deleted on 2025-05-22 19:47_

---

_@sharkdp reviewed on 2025-05-23 07:45_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/call/dunder.md`:243 on 2025-05-23 07:45_

Minor: This test here is very similar to the "Operating on instances" test above (search for `ThisFails`). Could we maybe add the "Class-level annotations with no value" part of this to the section above, instead?

---

_@carljm reviewed on 2025-05-27 19:04_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/dunder.md`:243 on 2025-05-27 19:04_

https://github.com/astral-sh/ruff/pull/18343

---
