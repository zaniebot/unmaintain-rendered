```yaml
number: 20924
title: "[ty] impl `VarianceInferable` for `KnownInstanceType`"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: type-alias-variance
created_at: 2025-10-16T16:57:35Z
updated_at: 2025-10-19T04:07:58Z
url: https://github.com/astral-sh/ruff/pull/20924
synced_at: 2026-01-10T17:34:34Z
```

# [ty] impl `VarianceInferable` for `KnownInstanceType`

---

_Pull request opened by @mtshiba on 2025-10-16 16:57_

## Summary

Derived from #20900

Implement `VarianceInferable` for `KnownInstanceType` (especially for `KnownInstanceType::TypeAliasType`).

The variance of a type alias matches its value type. In normal usage, type aliases are expanded to value types, so the variance of a type alias can be obtained without implementing this. However, for example, if we want to display the variance when hovering over a type alias, we need to be able to obtain the variance of the type alias itself (cf. #20900).

## Test Plan

I couldn't come up with a way to test this in mdtest, so I'm testing it in a test submodule at the end of `types.rs`.
I also added a test to `mdtest/generics/pep695/variance.md`, but it passes without the changes in this PR.

---

_Comment by @github-actions[bot] on 2025-10-16 16:59_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-16 17:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @mtshiba on 2025-10-16 17:14_

---

_Review requested from @carljm by @mtshiba on 2025-10-16 17:14_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-10-16 17:14_

---

_Review requested from @sharkdp by @mtshiba on 2025-10-16 17:14_

---

_Review requested from @dcreager by @mtshiba on 2025-10-16 17:14_

---

_Label `ty` added by @AlexWaygood on 2025-10-16 17:23_

---

_@sharkdp reviewed on 2025-10-17 11:05_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:10804 on 2025-10-17 11:05_

Can we please document what `raw_value_type` does compared to `value_type`?

More importantly, `value_type` has fixpoint handling, but `raw_value_type` does not. Does this lead to problems with recursive type aliases?

---

_@mtshiba reviewed on 2025-10-17 18:35_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types.rs`:10804 on 2025-10-17 18:35_

`raw_value_type` is called by `KnownInstanceType::variance_of` except within the queried `value_type`.

[As already explained](https://github.com/astral-sh/ruff/pull/20924#issue-3522711333), `KnownInstanceType::variance_of` is not normally called. In current usage, the only time it may be called is during a hover request.
Clearly, the type inference functions that `raw_value_type` depends on never call a hover request. Therefore, in current usage, there is no possibility of infinite recursion.

For safety, I added a note that `raw_value_type` is not tracked, so that anyone using it in the future knows to be careful.

---

_@sharkdp approved on 2025-10-17 19:12_

Thank you for the update and the explanations!

---

_Merged by @sharkdp on 2025-10-17 19:12_

---

_Closed by @sharkdp on 2025-10-17 19:12_

---

_Branch deleted on 2025-10-19 04:07_

---
