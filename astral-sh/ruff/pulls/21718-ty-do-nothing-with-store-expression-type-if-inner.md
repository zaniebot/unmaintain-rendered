```yaml
number: 21718
title: "[ty] do nothing with `store_expression_type` if `inner_expression_inference_state` is `Get`"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-1688
created_at: 2025-12-01T03:20:24Z
updated_at: 2025-12-05T02:07:42Z
url: https://github.com/astral-sh/ruff/pull/21718
synced_at: 2026-01-12T15:57:31Z
```

# [ty] do nothing with `store_expression_type` if `inner_expression_inference_state` is `Get`

---

_@mtshiba_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1688

## Test Plan

N/A


---

_Comment by @astral-sh-bot[bot] on 2025-12-01 03:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 03:24_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1207:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5806 diagnostics
+ Found 5807 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2025-12-01 06:56_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:7097 on 2025-12-01 06:56_

Could we use `multi_inference_state::Ignore` instead of having two modes that represent "read-only" type inference?

---

_Label `ty` added by @AlexWaygood on 2025-12-01 08:54_

---

_@Gankra reviewed on 2025-12-01 14:42_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/infer/builder.rs`:7097 on 2025-12-01 14:42_

drive-by context: I am likely to modify this check for `in_string_annotation` to do the "store (Expr, Expr) for string annotation types" thing we've been discussing.

---

_@mtshiba reviewed on 2025-12-01 16:20_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer/builder.rs`:7097 on 2025-12-01 16:20_

> Could we use `multi_inference_state::Ignore` instead of having two modes that represent "read-only" type inference?

I understand that `MultiInferenceState::Ignore` is an option to perform the same calculation twice and discard the second result.
Since all type inference for expressions is skipped while in `InnerExpressionInferenceState::Get`, I think it should be used whenever possible (for example, when only diagnostics are needed, such as the handling in `infer_subscript_type_expression` for union types).
But I'm not sure whether this can be assumed in all cases where `MultiInferenceState::Ignore` is used.

---

_Marked ready for review by @mtshiba on 2025-12-04 10:19_

---

_Review requested from @carljm by @mtshiba on 2025-12-04 10:19_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-12-04 10:19_

---

_Review requested from @sharkdp by @mtshiba on 2025-12-04 10:19_

---

_Review requested from @dcreager by @mtshiba on 2025-12-04 10:19_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-04 11:12_

---

_@carljm reviewed on 2025-12-05 02:05_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:7097 on 2025-12-05 02:05_

I think it is necessary for `MultiInferenceState` to redo inference, since it is doing it with different type context. So I think we need both.

---

_@carljm approved on 2025-12-05 02:05_

This looks correct, thank you.

---

_Merged by @carljm on 2025-12-05 02:05_

---

_Closed by @carljm on 2025-12-05 02:05_

---

_Branch deleted on 2025-12-05 02:07_

---
