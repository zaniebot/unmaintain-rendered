```yaml
number: 22164
title: Render the entire diagnostic message in all output formats
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: brent/diagnostic-body
created_at: 2025-12-23T17:11:08Z
updated_at: 2025-12-24T15:28:25Z
url: https://github.com/astral-sh/ruff/pull/22164
synced_at: 2026-01-10T16:36:18Z
```

# Render the entire diagnostic message in all output formats

---

_Pull request opened by @ntBre on 2025-12-23 17:11_

Summary
--

This PR fixes https://github.com/astral-sh/ty/issues/2186 by replacing uses of
`Diagnostic::body` with [`Diagnostic::concise_message`][d]. The initial report
was only about ty's GitHub and GitLab output formats, but I think it makes sense
to prefer `concise_message` in the other output formats too. Ruff currently only
sets the primary message on its diagnostics, which is why this has no effect on
Ruff, and ty currently only supports the GitHub and GitLab formats that used
`body`, but removing `body` should help to avoid this problem as Ruff starts to
use more diagnostic features or ty gets new output formats.

Test Plan
--

Updated existing GitLab and GitHub CLI tests to have `reveal_type` diagnostics

[d]: https://github.com/astral-sh/ruff/blob/395bf106ab/crates/ruff_db/src/diagnostic/mod.rs#L185
[t]: https://github.com/astral-sh/ruff/blob/395bf106ab/crates/ruff/tests/cli/lint.rs#L3509


---

_Label `bug` added by @ntBre on 2025-12-23 17:11_

---

_Label `diagnostics` added by @ntBre on 2025-12-23 17:11_

---

_Label `ty` added by @ntBre on 2025-12-23 17:11_

---

_Comment by @astral-sh-bot[bot] on 2025-12-23 17:12_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-23 17:13_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 42 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2803 diagnostics
+ Found 2806 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- Found 1848 diagnostics
+ Found 1849 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@ntBre reviewed on 2025-12-23 17:22_

---

_Review comment by @ntBre on `crates/ty/tests/cli/main.rs`:815 on 2025-12-23 17:22_

I probably should have looked around a little harder. I forgot that I added separate tests for these in earlier PRs. We could probably just combine the separate [`gitlab_diagnostics`](https://github.com/astral-sh/ruff/blob/ed64c4d943d5a29c017550a6fc015922666002ac/crates/ty/tests/cli/main.rs#L656) and [`github_diagnostics`](https://github.com/astral-sh/ruff/blob/ed64c4d943d5a29c017550a6fc015922666002ac/crates/ty/tests/cli/main.rs#L721) tests with this one? And maybe also [`concise_diagnostics`](https://github.com/astral-sh/ruff/blob/ed64c4d943d5a29c017550a6fc015922666002ac/crates/ty/tests/cli/main.rs#L632) and [`concise_revealed_type`](https://github.com/astral-sh/ruff/blob/ed64c4d943d5a29c017550a6fc015922666002ac/crates/ty/tests/cli/main.rs#L752). Or I can add the `reveal_type` call back to the pre-existing tests.

---

_Comment by @astral-sh-bot[bot] on 2025-12-23 17:24_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Marked ready for review by @ntBre on 2025-12-23 17:26_

---

_Review requested from @carljm by @ntBre on 2025-12-23 17:26_

---

_Review requested from @MichaReiser by @ntBre on 2025-12-23 17:26_

---

_Review requested from @sharkdp by @ntBre on 2025-12-23 17:26_

---

_Review requested from @dcreager by @ntBre on 2025-12-23 17:26_

---

_Review requested from @BurntSushi by @carljm on 2025-12-23 19:49_

---

_@carljm approved on 2025-12-23 19:50_

Looks reasonable to me!

---

_Review comment by @MichaReiser on `crates/ty/tests/cli/main.rs`:814 on 2025-12-24 07:34_

Nit: I would probably create a helper function and call that from separate `concise_output`, `full_output` etc. functions over using test case, given that the test cases are very limited (has better IDE support)

---

_Review comment by @MichaReiser on `crates/ty/tests/cli/main.rs`:815 on 2025-12-24 07:35_

I agree that the tests now feel somewhat redundant. I'm not sure what your concern/question about `reveal_type` is

---

_Review comment by @MichaReiser on `crates/ty/tests/cli/snapshots/cli__output_format_gitlab.snap`:20 on 2025-12-24 07:36_

You have to redact the `fingerprint`, I think, because it might be different on different platforms? At least, that's what you did in https://github.com/astral-sh/ruff/blob/ed64c4d943d5a29c017550a6fc015922666002ac/crates/ty/tests/cli/main.rs#L656

---

_@MichaReiser approved on 2025-12-24 07:39_

> The only real downside is needing to call ConciseMessage::to_string in a
couple of the output formats.

If that's a concern, you could consider adding a `to_str` method on `ConciseMessage` that returns a `Cow<str>` which avoids allocation in the `Custom` and `MainDiagnostic` cases


I think we should merge some of the tests before landing this change

---

_@ntBre reviewed on 2025-12-24 14:03_

---

_Review comment by @ntBre on `crates/ty/tests/cli/main.rs`:815 on 2025-12-24 14:03_

I just meant that I could either add a `reveal_type` call to the existing GitHub and GitLab tests and remove this new test, or I could remove the four old tests I mentioned in favor of this one. 

I think I'll just update the existing GitHub and GitLab tests and revert the `test_case` stuff.

---

_Review requested from @AlexWaygood by @ntBre on 2025-12-24 14:42_

---

_Review request for @dcreager removed by @ntBre on 2025-12-24 14:43_

---

_Review request for @sharkdp removed by @ntBre on 2025-12-24 14:43_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-12-24 14:43_

---

_Merged by @ntBre on 2025-12-24 15:28_

---

_Closed by @ntBre on 2025-12-24 15:28_

---

_Branch deleted on 2025-12-24 15:28_

---
