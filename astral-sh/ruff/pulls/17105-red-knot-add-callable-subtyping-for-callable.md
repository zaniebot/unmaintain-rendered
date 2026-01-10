```yaml
number: 17105
title: "[red-knot] Add callable subtyping for callable instances and bound methods"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: bounded-method-subtype
created_at: 2025-03-31T23:23:23Z
updated_at: 2025-04-01T23:49:15Z
url: https://github.com/astral-sh/ruff/pull/17105
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Add callable subtyping for callable instances and bound methods

---

_Pull request opened by @MatthewMckee4 on 2025-03-31 23:23_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Trying to improve #17005
Partially fixes #16953

## Test Plan

Update is_subtype_of.md


---

_Comment by @github-actions[bot] on 2025-03-31 23:25_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
rich (https://github.com/Textualize/rich)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/tests/test_progress.py:292:60: Object of type `MockClock` cannot be assigned to parameter `get_time` of function `track`; expected type `(() -> int | float) | None`
- Found 578 diagnostics
+ Found 577 diagnostics

black (https://github.com/psf/black)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:1280:48: Object of type `<bound method `readline` of `BytesIO`> | @Todo(instance attribute on class with dynamic base)` cannot be assigned to parameter 1 (`readline`) of function `detect_encoding`; expected type `() -> bytes | bytearray`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:1280:48: Object of type `<bound method `readline` of `BytesIO`> | @Todo(instance attribute on class with dynamic base)` cannot be assigned to parameter 1 (`readline`) of function `detect_encoding`; expected type `() -> bytes | bytearray`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:1280:48: Object of type `<bound method `readline` of `BytesIO`> | @Todo(instance attribute on class with dynamic base)` cannot be assigned to parameter 1 (`readline`) of function `detect_encoding`; expected type `() -> bytes | bytearray`
- Found 212 diagnostics
+ Found 209 diagnostics

```
</details>


---

_Label `red-knot` added by @AlexWaygood on 2025-03-31 23:42_

---

_@MatthewMckee4 reviewed on 2025-04-01 00:18_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:4341 on 2025-04-01 00:18_

I'm not sure this is how we will want to implement this , but i think its right to not check the name of the variable, and since staticmethod is not supported, this should be fine, but when that is implemented i think we will have to check decorators

---

_Marked ready for review by @MatthewMckee4 on 2025-04-01 00:20_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-04-01 00:20_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-04-01 00:20_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-04-01 00:20_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-04-01 00:20_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:4335 on 2025-04-01 07:13_

Is the `clone` call here necessary, considering that we already clone each parameter on the line below

---

_@MichaReiser reviewed on 2025-04-01 07:14_

---

_Comment by @MichaReiser on 2025-04-01 07:14_

Not sure, but this might be a good one for @dcreager to review?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1077 on 2025-04-01 20:27_

"Bounded" and "bound" have different meanings; the former means "has a bound", as in a boundary or limit. The latter means "is bound to something" (as in the verb "to bind"). Typevars can be "bounded" if they have a bound; methods are "bound" to an instance.

```suggestion
### Bound methods
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1141 on 2025-04-01 20:29_

maybe add some negative tests in here as well, to show that we are actually checking the signature?

Would also be interesting in future to test that `A.f` is a subtype of `Callable[[A, int], int]`, but this requires self types, so not for this PR. But a test with a TODO comment would be useful here.

---

_@carljm approved on 2025-04-01 20:34_

This looks reasonable to me! The tests are good, and we can always change the implementation of `BoundMethodType::into_callable_type` if we decide we need to preserve the first argument.

Thank you!

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1141 on 2025-04-01 20:53_

Sure, will add that soon

---

_@MatthewMckee4 reviewed on 2025-04-01 20:53_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:4341 on 2025-04-01 21:05_

Can you move the part that manipulates the `Signature` into a new `Signature::bind_self` method? That will make it a bit more readable here:

```suggestion
        Type::Callable(CallableType::General(GeneralCallableType::new(
            db,
            self.function(db).signature(db).bind_self(),
        )))
```

---

_@dcreager approved on 2025-04-01 21:06_

---

_@MatthewMckee4 reviewed on 2025-04-01 21:13_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:4341 on 2025-04-01 21:13_

Sure

---

_Renamed from "[red-knot] Add callable subtyping for bounded methods" to "[red-knot] Add callable subtyping for callable instances and bound methods" by @carljm on 2025-04-01 23:31_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1120 on 2025-04-01 23:33_

Nit: underscore is a good name for functions we are never calling or referencing, we just want a scope with some type-annotated names in it. It's not a good name for functions we actually reference.

```suggestion
def f(fn: Callable[[int], int]) -> None: ...

f(a)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1146 on 2025-04-01 23:36_

```suggestion
# TODO: This assertion should be true
```

---

_@carljm reviewed on 2025-04-01 23:37_

---

_Merged by @carljm on 2025-04-01 23:40_

---

_Closed by @carljm on 2025-04-01 23:40_

---

_@MatthewMckee4 reviewed on 2025-04-01 23:43_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1146 on 2025-04-01 23:43_

Thanks, not sure why i did that

---

_Branch deleted on 2025-04-01 23:49_

---
