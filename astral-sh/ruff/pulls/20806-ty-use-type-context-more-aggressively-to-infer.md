```yaml
number: 20806
title: "[ty] use type context more aggressively to infer values ​​when constructing a `TypedDict`"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: typed-dict-key-expr2
created_at: 2025-10-10T18:06:40Z
updated_at: 2025-10-10T23:51:16Z
url: https://github.com/astral-sh/ruff/pull/20806
synced_at: 2026-01-12T15:57:10Z
```

# [ty] use type context more aggressively to infer values ​​when constructing a `TypedDict`

---

_@mtshiba_

## Summary

Based on @ibraheemdev's comment on #20792:

> I think we can also update our bidirectional inference code, [which makes the same assumption](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types/infer/builder.rs?rgh-link-date=2025-10-09T21%3A30%3A31Z#L5860).

This PR also adds more test cases for how `TypedDict` annotations affect generic call inference.

## Test Plan

New tests in `typed_dict.md`


---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:128 on 2025-10-10 18:09_

This should be resolved by #20528.

---

_Comment by @github-actions[bot] on 2025-10-10 18:09_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@mtshiba reviewed on 2025-10-10 18:09_

---

_Comment by @github-actions[bot] on 2025-10-10 18:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 51 diagnostics
+ Found 52 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-10-10 18:17_

---

_Marked ready for review by @mtshiba on 2025-10-10 18:18_

---

_Review requested from @carljm by @mtshiba on 2025-10-10 18:18_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-10-10 18:18_

---

_Review requested from @sharkdp by @mtshiba on 2025-10-10 18:18_

---

_Review requested from @dcreager by @mtshiba on 2025-10-10 18:18_

---

_@ibraheemdev approved on 2025-10-10 20:49_

---

_@carljm approved on 2025-10-10 23:49_

---

_Merged by @carljm on 2025-10-10 23:51_

---

_Closed by @carljm on 2025-10-10 23:51_

---
