```yaml
number: 15052
title: "[red-knot] Add support for unpacking union types"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/unpack-union
created_at: 2024-12-19T07:15:20Z
updated_at: 2024-12-20T11:01:17Z
url: https://github.com/astral-sh/ruff/pull/15052
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Add support for unpacking union types

---

_Pull request opened by @dhruvmanila on 2024-12-19 07:15_

## Summary

Refer: https://github.com/astral-sh/ruff/issues/13773#issuecomment-2548020368

This PR adds support for unpacking union types. 

Unpacking a union type requires us to first distribute the types for all the targets that are involved in an unpacking. For example, if there are two targets and a union type that needs to be unpacked, each target will get a type from each element in the union type.

For example, if the type is `tuple[int, int] | tuple[int, str]` and the target has two elements `(a, b)`, then
* The type of `a` will be a union of `int` and `int` which are at index 0 in the first and second tuple respectively which resolves to an `int`.
* Similarly, the type of `b` will be a union of `int` and `str` which are at index 1 in the first and second tuple respectively which will be `int | str`.

### Refactors

There are couple of refactors that are added in this PR:
* Add a `debug_assertion` to validate that the unpack target is a list or a tuple
* Add a separate method to handle starred expression

## Test Plan

Update `unpacking.md` with additional test cases that uses union types. This is done using parameter type hints style.


---

_Label `red-knot` added by @dhruvmanila on 2024-12-19 07:15_

---

_Comment by @github-actions[bot] on 2024-12-19 07:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:45 on 2024-12-20 04:27_

Refactor 1: Add a debug assertion to make sure that the target is a list or a tuple.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:139 on 2024-12-20 04:28_

Refactor 2: Move the logic that handles starred expression to a separate method mainly to split the main unpacking function and to avoid being a distraction to the main logic.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:78 on 2024-12-20 04:29_

This is the main entry point that handles the union type. For a union type, we need to apply the logic for each subtype in it.

I want to spend some time to avoid the nested vectors but this is not a hot path so I'd be fine moving with this as well.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:114 on 2024-12-20 04:31_

The core logic for unpacking that
1. If it's a tuple type, handle starred expression and then add each type to the relevant target
2. Or, check if it's an iterable / literal string and push the type to each target

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:127 on 2024-12-20 04:32_

At the end, we'll union the types for each target and call the function recursively to handle any nested unpacking and update the targets mapping.

---

_@dhruvmanila reviewed on 2024-12-20 04:32_

---

_Marked ready for review by @dhruvmanila on 2024-12-20 04:32_

---

_Review requested from @carljm by @dhruvmanila on 2024-12-20 04:32_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-12-20 04:32_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-12-20 04:32_

---

_Review requested from @sharkdp by @dhruvmanila on 2024-12-20 04:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:317 on 2024-12-20 04:35_

I'm pretty sure this will immediately simplify to just `tuple[int, int]`, so it's not really testing unpacking of a union, just union simplification, but that's fine; it's still relevant to test the overall behavior.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:310 on 2024-12-20 04:36_

We should probably have some tests of what happens when one element of the union is the wrong length?

And some tests for unions like `tuple[int, str] | IterableOfBytes()` where `IterableOfBytes` is a type that defines `__iter__` etc so that iterating it returns some type.

And maybe some tests for nested unpacking involving unions (I mean like where we have `a, (b, c) = ...` and unions on the RHS.)

Unpacking unions when there are starred elements on the LHS?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/unpacker.rs`:119 on 2024-12-20 04:45_

Safety comment here to note that we initialized `target_types` based on len of `elts`, so this can't panic?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/unpacker.rs`:137 on 2024-12-20 04:47_

Would `targets` be a clearer name for this?

---

_@carljm approved on 2024-12-20 04:49_

Looks good, thank you!!

---

_@MichaReiser reviewed on 2024-12-20 06:57_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:210 on 2024-12-20 06:57_

Nit: It should be enough to only pass `scope` because you can get to the file by calling `scope.file(db)`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/unpacker.rs`:79 on 2024-12-20 07:09_


```suggestion
                        union_ty.elements(self.db())
                    }
                    _ => std::slice::from_ref(&value_ty),
                };

```

---

_@MichaReiser reviewed on 2024-12-20 07:09_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/unpacker.rs`:97 on 2024-12-20 07:11_

Will we run into duplicate diagnostic problems if we add diagnostics here (because unpack is called multiple times) or is it guaranteed that unpack is only called once?

---

_@MichaReiser reviewed on 2024-12-20 07:11_

---

_@MichaReiser approved on 2024-12-20 07:12_

---

_@dhruvmanila reviewed on 2024-12-20 10:08_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:137 on 2024-12-20 10:08_

Yeah, I think that's better, thanks.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:97 on 2024-12-20 10:11_

I don't think we should get duplicate diagnostics because of the `first` flag:

https://github.com/astral-sh/ruff/blob/92928560f8587995003eaf2ec0730f1deb802287/crates/red_knot_python_semantic/src/types/infer.rs#L1863-L1868

---

_@dhruvmanila reviewed on 2024-12-20 10:11_

---

_@dhruvmanila reviewed on 2024-12-20 10:52_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:310 on 2024-12-20 10:52_

Added these test cases:
* Nested tuple types
* Starred expressions
* Tuple size mismatch
* Union of custom iterator with a tuple type

---

_@dhruvmanila reviewed on 2024-12-20 10:58_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:78 on 2024-12-20 10:58_

I actually started with having a vector of `UnionBuilder` but the builder takes ownership of itself when adding the type which isn't possible without multiple pop and push to get the ownership of the builder.

---

_Merged by @dhruvmanila on 2024-12-20 11:01_

---

_Closed by @dhruvmanila on 2024-12-20 11:01_

---

_Branch deleted on 2024-12-20 11:01_

---
