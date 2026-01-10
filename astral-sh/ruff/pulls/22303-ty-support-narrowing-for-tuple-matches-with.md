```yaml
number: 22303
title: "[ty] Support narrowing for tuple matches with literal elements"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charliermarsh/typeddict-negative
created_at: 2025-12-30T17:07:50Z
updated_at: 2025-12-30T18:45:09Z
url: https://github.com/astral-sh/ruff/pull/22303
synced_at: 2026-01-10T16:36:19Z
```

# [ty] Support narrowing for tuple matches with literal elements

---

_Pull request opened by @charliermarsh on 2025-12-30 17:07_

## Summary

See: https://github.com/astral-sh/ruff/pull/22299#issuecomment-3699913849.


---

_Label `ty` added by @charliermarsh on 2025-12-30 17:07_

---

_Comment by @astral-sh-bot[bot] on 2025-12-30 17:09_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-30 17:10_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/parsing.py:482:42: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[str, str | None]`, found `str | tuple[str, str | None]`
- tanjun/parsing.py:484:16: error[invalid-return-type] Return type does not match returned value: expected `str | None`, found `str | tuple[str, str | None] | None`
- tanjun/parsing.py:491:38: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `str`, found `str | tuple[str, str | None]`
- tanjun/parsing.py:493:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str | None] | None`, found `str | tuple[str, str | None] | None`
- Found 145 diagnostics
+ Found 141 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2804 diagnostics
+ Found 2801 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14447 diagnostics
+ Found 14448 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2025-12-30 17:42_

---

_Review requested from @carljm by @charliermarsh on 2025-12-30 17:42_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-30 17:42_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-30 17:42_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-30 17:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:1755 on 2025-12-30 17:44_

can we not just rename `is_supported_typeddict_tag_literal` to `is_supported_tag_literal` and use that in both places?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:1784 on 2025-12-30 17:48_

I think we probably want to return `true` here if it's a union like `tuple[Literal["a"], 1] | list[str]` and the narrowing predicate is `union[0] == "a"`. (You do this.)

But we probably want to return `false` if it's something like `tuple[int, Literal["a"]] | tuple[int]` and the narrowing predicate is `union[1] == "a"`. The index is out of bounds for the second tuple in the union, and that will cause us to emit a diagnostic elsewhere, so we probably shouldn't "do the narrowing anyway" (that would be a bit weird)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:1436 on 2025-12-30 17:50_

we can probably skip this entirely if we already added a typeddict tagged-union constraint immediately above

```suggestion
            else if let Some((place, constraint)) = self.narrow_tuple_subscript(
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/narrow/complex_target.md`:357 on 2025-12-30 17:55_

I don't think this TODO comment is correct. You can create `int` subclasses where the integers compare equal to `"tag1"`. It wouldn't be sound to do this narrowing.

```suggestion
        # A list of ints could have int subclasses in it,
        # and int subclasses could have custom `__eq__` methods such that they
        # compare equal to `"tag1"`, so `list[int]` cannot be narrowed out of this
        # union.
```

---

_@AlexWaygood approved on 2025-12-30 17:55_

---

_Merged by @charliermarsh on 2025-12-30 18:45_

---

_Closed by @charliermarsh on 2025-12-30 18:45_

---

_Branch deleted on 2025-12-30 18:45_

---
