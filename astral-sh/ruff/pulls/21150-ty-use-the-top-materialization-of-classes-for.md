```yaml
number: 21150
title: "[ty] Use the top materialization of classes for narrowing in class-patterns for `match` statements"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/match-top-materialization
created_at: 2025-10-30T19:15:46Z
updated_at: 2025-10-30T20:44:52Z
url: https://github.com/astral-sh/ruff/pull/21150
synced_at: 2026-01-12T15:57:17Z
```

# [ty] Use the top materialization of classes for narrowing in class-patterns for `match` statements

---

_@AlexWaygood_

Fixes https://github.com/astral-sh/ty/issues/1458

---

_Review requested from @carljm by @AlexWaygood on 2025-10-30 19:15_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-30 19:15_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-30 19:15_

---

_Label `bug` added by @AlexWaygood on 2025-10-30 19:15_

---

_Label `ty` added by @AlexWaygood on 2025-10-30 19:15_

---

_Comment by @github-actions[bot] on 2025-10-30 19:17_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-30 19:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Expression (https://github.com/cognitedata/Expression)
- expression/core/result.py:85:73: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `_TSource@default_with | _TSourceOut@Result`
- expression/core/result.py:97:65: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TResult@map, _TErrorOut@Result]`
- expression/core/result.py:113:10: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TResult@map2, _TErrorOut@Result]`
- expression/core/result.py:125:70: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TSourceOut@Result, _TResult@map_error]`
- expression/core/result.py:137:86: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TResult@bind, _TErrorOut@Result]`
- expression/core/result.py:177:10: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TSourceOut@Result, _TErrorOut@Result]`
- expression/core/result.py:191:23: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `dict[str, _TSourceOut@Result | _TErrorOut@Result | Literal["ok", "error"]]`
- expression/core/result.py:205:23: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TErrorOut@Result, _TSourceOut@Result]`
- expression/core/result.py:269:26: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `str`
- expression/core/try_.py:24:26: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `str`
- expression/effect/async_result.py:32:10: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TResult@bind, _TError@AsyncResultBuilder]`
- expression/effect/async_result.py:79:94: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TSource@AsyncResultBuilder, _TError@AsyncResultBuilder]`
- Found 213 diagnostics
+ Found 201 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@carljm approved on 2025-10-30 19:30_

---

_@carljm approved on 2025-10-30 19:58_

---

_Comment by @MeGaGiGaGon on 2025-10-30 20:17_

That's fun in the primer seeing a bunch of Expression Result diagnostics going away, since my issue came from also running into this making my own Result type :)

---

_Closed by @AlexWaygood on 2025-10-30 20:31_

---

_Reopened by @AlexWaygood on 2025-10-30 20:31_

---

_Merged by @AlexWaygood on 2025-10-30 20:44_

---

_Closed by @AlexWaygood on 2025-10-30 20:44_

---

_Branch deleted on 2025-10-30 20:44_

---
