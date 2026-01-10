```yaml
number: 16970
title: "[red-knot] A `FunctionType` can be a subtype of `Callable`  (but never the other way around)"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: function-type-subtype-callable
created_at: 2025-03-25T16:26:37Z
updated_at: 2025-03-27T00:21:59Z
url: https://github.com/astral-sh/ruff/pull/16970
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] A `FunctionType` can be a subtype of `Callable`  (but never the other way around)

---

_Pull request opened by @MatthewMckee4 on 2025-03-25 16:26_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Partially fixes #16953

## Test Plan

Update is_subtype_of.md

Im unsure how thoroughly this needs to be tested, im not sure if it does for many more function types?

---

_Review requested from @carljm by @MatthewMckee4 on 2025-03-25 16:26_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-25 16:26_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-03-25 16:26_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-03-25 16:26_

---

_Comment by @github-actions[bot] on 2025-03-25 16:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pybind11 (https://github.com/pybind/pybind11)
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/pybind11/pybind11/setup_helpers.py:415:9: Default value of type `Literal[no_recompile]` is not assignable to annotated parameter type `(str, str, /) -> bool`
- Found 276 diagnostics
+ Found 275 diagnostics

rich (https://github.com/Textualize/rich)
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/rich/rich/console.py:1881:9: Default value of type `Literal[currentframe]` is not assignable to annotated parameter type `() -> FrameType | None`
- Found 586 diagnostics
+ Found 585 diagnostics

black (https://github.com/psf/black)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/black/src/black/trans.py:2509:12: Object of type `Literal[is_valid_index]` is not assignable to return type `(int, /) -> bool`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/lines.py:684:56: Object of type `Literal[is_import]` cannot be assigned to parameter `first_leaf_matches` of bound method `is_fmt_pass_converted`; expected type `((Leaf, /) -> bool) | None`
- Found 235 diagnostics
+ Found 233 diagnostics

```
</details>


---

_Comment by @MatthewMckee4 on 2025-03-25 17:41_

Ive just added the test that is failing, I'm not sure why it is failing though, any help is appreciated.

---

_Comment by @MatthewMckee4 on 2025-03-25 21:32_

Maybe an interesting point, and a question i have, should:
```rs
is_subtype_of(CallableTypeFromFunction[f], CallableTypeFromFunction[g]))
=>
is_subtype_of(TypeOf[f], TypeOf[g])
```

I guess this just depends on how we define FunctionLiteral arms in is_subtype_of. I would think that this equality should hold, but interested to see if there are any points against this

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:522 on 2025-03-25 21:52_

For good measure, we could assert this is not true the other way around.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:520 on 2025-03-25 21:56_

This should not be true. The `TypeOf[required_return_type]` or `TypeOf[optional_return_type]` evaluate to a function-literal type, which is a singleton type consisting of exactly one runtime object (that very function). No function-literal type has any subtype (other than itself and `Never`).

So it is correct for this test to fail, and the fix is to reverse the sense of the test.

And it's not surprising that it fails, since we don't have (and don't add in this PR), any case for `FunctionLiteral` vs `FunctionLiteral`.

```suggestion
# TypeOf[some_function] is a singleton function-literal type,  not a general callable type
static_assert(not is_subtype_of(TypeOf[required_return_type], TypeOf[optional_return_type]))
```

---

_Label `red-knot` added by @MichaReiser on 2025-03-25 21:56_

---

_@carljm reviewed on 2025-03-25 21:57_

Barring the one test that should be changed, this looks good to me, and reasonably well tested!

---

_Comment by @carljm on 2025-03-25 21:58_

> Maybe an interesting point, and a question i have, should:
> 
> ```rust
> is_subtype_of(CallableTypeFromFunction[f], CallableTypeFromFunction[g]))
> =>
> is_subtype_of(TypeOf[f], TypeOf[g])
> ```

No, this should not hold, for the same reason mentioned in my review above. `TypeOf[some_function]` is a singleton function literal type, which has no subtypes besides itself and `Never`. It is not a general callable type whose inhabitants are all functions with a substitutable signature (that's what we get from `CallableTypeFromFunction[some_function]`).

---

_Comment by @MatthewMckee4 on 2025-03-25 22:00_

> > Maybe an interesting point, and a question i have, should:
> > ```rust
> > is_subtype_of(CallableTypeFromFunction[f], CallableTypeFromFunction[g]))
> > =>
> > is_subtype_of(TypeOf[f], TypeOf[g])
> > ```
> 
> No, this should not hold, for the same reason mentioned in my review above. `TypeOf[some_function]` is a singleton function literal type, which has no subtypes besides itself and `Never`. It is not a general callable type whose inhabitants are all functions with a substitutable signature (that's what we get from `CallableTypeFromFunction[some_function]`).

Okay sounds good, thank you for the response and review. I think i thought `TypeOf[some_function]` was more similar to `CallableTypeFromFunction[some_function]` than it is.

---

_Review requested from @MichaReiser by @MichaReiser on 2025-03-25 22:01_

---

_Review request for @MichaReiser removed by @MichaReiser on 2025-03-25 22:01_

---

_@MatthewMckee4 reviewed on 2025-03-25 22:04_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:522 on 2025-03-25 22:04_

Which one?

---

_Merged by @carljm on 2025-03-25 22:04_

---

_Closed by @carljm on 2025-03-25 22:04_

---

_@carljm reviewed on 2025-03-26 11:38_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:522 on 2025-03-26 11:38_

I meant both, but after reviewing the rest of the PR I think this is already adequately tested.

---

_Branch deleted on 2025-03-27 00:21_

---
