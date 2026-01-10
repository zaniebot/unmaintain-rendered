```yaml
number: 21164
title: "[ty] Do not promote literals in contravariant position"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: david/fix-1463
created_at: 2025-10-31T14:26:54Z
updated_at: 2025-10-31T15:14:58Z
url: https://github.com/astral-sh/ruff/pull/21164
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Do not promote literals in contravariant position

---

_Pull request opened by @sharkdp on 2025-10-31 14:26_

## Summary

closes https://github.com/astral-sh/ty/issues/1463

## Test Plan

Regression tests


---

_Label `bug` added by @sharkdp on 2025-10-31 14:26_

---

_Label `ty` added by @sharkdp on 2025-10-31 14:26_

---

_Comment by @github-actions[bot] on 2025-10-31 14:29_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-31 14:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
setuptools (https://github.com/pypa/setuptools)
- setuptools/_distutils/archive_util.py:288:46: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `Unknown | bool`
+ setuptools/_distutils/archive_util.py:288:46: error[invalid-argument-type] Argument is incorrect: Expected `Literal["gzip", "bzip2", "xz"] | None`, found `Unknown | bool`

ibis (https://github.com/ibis-project/ibis)
- ibis/tests/expr/test_table.py:1936:9: error[invalid-argument-type] Argument to bound method `pivot_longer` is incorrect: Expected `((str, /) -> Value) | Mapping[str, (str, /) -> Value] | None`, found `dict[str, (Overload[(self: str) -> str, (self) -> str]) | <class 'int'>]`
+ ibis/tests/expr/test_table.py:1936:9: error[invalid-argument-type] Argument to bound method `pivot_longer` is incorrect: Expected `((str, /) -> Value) | Mapping[str, (str, /) -> Value] | None`, found `dict[str, (Overload[(self: LiteralString) -> str, (self) -> str]) | <class 'int'>]`
- ibis/tests/expr/test_table.py:1959:13: error[invalid-argument-type] Argument to bound method `pivot_longer` is incorrect: Expected `((str, /) -> Value) | Mapping[str, (str, /) -> Value] | None`, found `dict[str, (Overload[(self: str) -> str, (self) -> str]) | <class 'int'>]`
+ ibis/tests/expr/test_table.py:1959:13: error[invalid-argument-type] Argument to bound method `pivot_longer` is incorrect: Expected `((str, /) -> Value) | Mapping[str, (str, /) -> Value] | None`, found `dict[str, (Overload[(self: LiteralString) -> str, (self) -> str]) | <class 'int'>]`

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-10-31 14:41_

---

_Review requested from @carljm by @sharkdp on 2025-10-31 14:41_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-31 14:41_

---

_Review requested from @dcreager by @sharkdp on 2025-10-31 14:41_

---

_@AlexWaygood approved on 2025-10-31 14:59_

---

_Merged by @sharkdp on 2025-10-31 15:00_

---

_Closed by @sharkdp on 2025-10-31 15:00_

---

_Branch deleted on 2025-10-31 15:00_

---

_@carljm reviewed on 2025-10-31 15:14_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/literal_promotion.md`:21 on 2025-10-31 15:14_

I suspect that this should better be `list[Unknown | int]`. That is, if we are in a context where we are promoting literals, the contravariant version of "promotion" is just to eliminate the negative literal types. This will help with the same kinds of invariance problems that literal promotion is intended to help with in the first place.

---
