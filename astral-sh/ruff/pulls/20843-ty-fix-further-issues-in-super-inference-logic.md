```yaml
number: 20843
title: "[ty] Fix further issues in `super()` inference logic"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/more-super-fixes
created_at: 2025-10-13T12:33:48Z
updated_at: 2025-10-14T12:48:49Z
url: https://github.com/astral-sh/ruff/pull/20843
synced_at: 2026-01-12T15:57:11Z
```

# [ty] Fix further issues in `super()` inference logic

---

_@AlexWaygood_

## Summary

Some small followups to https://github.com/astral-sh/ruff/pull/20814, after studying the latest ecosystem report on https://github.com/astral-sh/ruff/pull/20723 and studying the code some more:
- Allow `type[]` types as the first argument to `super()`: there are several ecosystem projects that do that
- Disallow `GenericAlias` instances as the first argument to `super()`: this fails at runtime
- Add a hint if the user has annotated `self` with a `TypeVar` that doesn't have an upper bound (they probably want to add an upper bound to the `TypeVar`)

## Test Plan

Snapshots and mdtests


---

_Review requested from @carljm by @AlexWaygood on 2025-10-13 12:33_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-13 12:33_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-13 12:33_

---

_Label `bug` added by @AlexWaygood on 2025-10-13 12:33_

---

_Label `ty` added by @AlexWaygood on 2025-10-13 12:33_

---

_@AlexWaygood reviewed on 2025-10-13 12:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/class/super.md`:130 on 2025-10-13 12:34_

```suggestion
`super(pivot_class, owner)` can be called from inside methods, just like single-argument `super()`:
```

---

_Comment by @github-actions[bot] on 2025-10-13 12:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-13 12:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scipy (https://github.com/scipy/scipy)
- scipy/stats/tests/test_distributions.py:3262:21: error[invalid-super-argument] `type[invgauss_gen]` is not a valid class
- scipy/stats/tests/test_distributions.py:3467:30: error[invalid-super-argument] `type[laplace_gen]` is not a valid class
- Found 6856 diagnostics
+ Found 6854 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@dcreager approved on 2025-10-14 00:41_

---

_Merged by @AlexWaygood on 2025-10-14 12:48_

---

_Closed by @AlexWaygood on 2025-10-14 12:48_

---

_Branch deleted on 2025-10-14 12:48_

---
