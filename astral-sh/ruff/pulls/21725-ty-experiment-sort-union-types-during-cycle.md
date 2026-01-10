```yaml
number: 21725
title: "[ty] experiment: sort union types during cycle recovery to stabilize output (for comparison with #21722)"
type: pull_request
state: closed
author: mtshiba
labels:
  - ty
assignees: []
draft: true
base: main
head: sort-recursive-union2
created_at: 2025-12-01T12:00:56Z
updated_at: 2025-12-12T08:36:58Z
url: https://github.com/astral-sh/ruff/pull/21725
synced_at: 2026-01-10T16:42:11Z
```

# [ty] experiment: sort union types during cycle recovery to stabilize output (for comparison with #21722)

---

_Pull request opened by @mtshiba on 2025-12-01 12:00_

## Summary

This is a PR just to compare whether the output has stabilized. The content is the same as https://github.com/astral-sh/ruff/pull/21722, and the base of mypy_primer points to 56d3173da5c4eaa597efb010d20130069985e7e2.

## Test Plan

<!-- How was it tested? -->


---

_Comment by @astral-sh-bot[bot] on 2025-12-01 12:03_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 12:04_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 492 diagnostics
+ Found 494 diagnostics

poetry (https://github.com/python-poetry/poetry)
- src/poetry/utils/env/env_manager.py:260:48: error[invalid-type-form] Invalid subscript of object of type `def list(self, name: None | str = None) -> Unknown` in type expression
+ src/poetry/utils/env/env_manager.py:260:48: error[invalid-type-form] Invalid subscript of object of type `def list(self, name: str | None = None) -> Unknown` in type expression

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/linear_model/tests/test_bayes.py:31:12: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `list[Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
+ sklearn/linear_model/tests/test_bayes.py:31:12: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `Unknown | list[Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]]`
- sklearn/linear_model/tests/test_logistic.py:535:39: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `Unknown | dict[Unknown, Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ sklearn/linear_model/tests/test_logistic.py:535:39: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `dict[Unknown, Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/tests/test_logistic.py:540:16: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `Unknown | dict[Unknown, Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ sklearn/linear_model/tests/test_logistic.py:540:16: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `dict[Unknown, Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/tests/test_logistic.py:693:35: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `Unknown | dict[Unknown, Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ sklearn/linear_model/tests/test_logistic.py:693:35: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `dict[Unknown, Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/tests/test_logistic.py:727:43: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `Unknown | dict[Unknown, Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ sklearn/linear_model/tests/test_logistic.py:727:43: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `dict[Unknown, Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/tests/test_logistic.py:748:20: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `Unknown | dict[Unknown, Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ sklearn/linear_model/tests/test_logistic.py:748:20: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `dict[Unknown, Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/tests/test_logistic.py:760:31: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `Unknown | dict[Unknown, Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ sklearn/linear_model/tests/test_logistic.py:760:31: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `dict[Unknown, Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/tests/test_logistic.py:1977:12: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `Unknown | list[Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]] | float`
+ sklearn/linear_model/tests/test_logistic.py:1977:12: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `float | list[Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/tests/test_logistic.py:2014:35: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `Unknown | dict[Unknown, Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ sklearn/linear_model/tests/test_logistic.py:2014:35: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `dict[Unknown, Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`


```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2025-12-01 12:16_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 12:19_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Closed by @mtshiba on 2025-12-01 14:02_

---

_Reopened by @mtshiba on 2025-12-01 14:02_

---

_Closed by @mtshiba on 2025-12-12 08:36_

---

_Branch deleted on 2025-12-12 08:36_

---
