```yaml
number: 19829
title: "[ty] Improve ability to solve TypeVars when they appear in unions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/tvar-solving-order
created_at: 2025-08-08T13:50:34Z
updated_at: 2025-08-08T16:50:39Z
url: https://github.com/astral-sh/ruff/pull/19829
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Improve ability to solve TypeVars when they appear in unions

---

_Pull request opened by @AlexWaygood on 2025-08-08 13:50_

## Summary

This PR moves the `Union` branch higher up in the `match` expression in `SpecializationBuilder::infer()`. Doing this improves our ability to solve TypeVars when they appear in unions (see the primer report!)

## Test Plan

Mdtests added. The first mdtest (TypeVars with an upper bound in a union) does not pass on `main`. The second one (constrained TypeVars in a union) _does_ pass on `main`, but was accidentally broken in https://github.com/astral-sh/ruff/pull/19811, and I only found out about the breakage from primer because we had some missing test coverage. So I'm adding that test coverage here!

---

_Label `ty` added by @AlexWaygood on 2025-08-08 13:50_

---

_Comment by @github-actions[bot] on 2025-08-08 13:52_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-08 13:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- src/trio/_core/_tests/test_run.py:2668:5: error[invalid-assignment] Object of type `<class 'RuntimeError'> | RaisesGroup[ExcT_1@__init__ | ExceptionGroup[ExcT_2@__init__]]` is not assignable to `type[RuntimeError] | RaisesGroup[RuntimeError]`
- src/trio/_tests/type_tests/raisesgroup.py:169:5: error[invalid-assignment] Object of type `RaisesGroup[ExcT_1@__init__ | ExceptionGroup[ExcT_2@__init__]]` is not assignable to `RaisesGroup[ValueError]`
- src/trio/_tests/type_tests/raisesgroup.py:170:5: error[invalid-assignment] Object of type `RaisesGroup[ExcT_1@__init__ | ExceptionGroup[ExcT_2@__init__]]` is not assignable to `RaisesGroup[ValueError]`
- src/trio/_tests/type_tests/raisesgroup.py:171:5: error[invalid-assignment] Object of type `RaisesGroup[ExcT_1@__init__ | ExceptionGroup[ExcT_2@__init__]]` is not assignable to `RaisesGroup[ValueError]`
- Found 732 diagnostics
+ Found 728 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/states.py:369:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[State[Any]]`, found `Collection[Unknown]`
+ src/prefect/states.py:369:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[State[Any]]`, found `Collection[R@return_value_to_state]`

sympy (https://github.com/sympy/sympy)
- sympy/polys/densebasic.py:861:28: error[invalid-argument-type] Argument to function `dmp_ground_p` is incorrect: Argument type `Er@dmp_ground_p | None` does not satisfy upper bound of type variable `Er`
- Found 12943 diagnostics
+ Found 12942 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Renamed from "[ty] [experiment] Move `Union` branch higher up in `SpecializationBuilder::infer()`" to "[ty] Improve ability to solve TypeVars when they appear in unions" by @AlexWaygood on 2025-08-08 16:38_

---

_Marked ready for review by @AlexWaygood on 2025-08-08 16:41_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-08 16:41_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-08 16:41_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-08 16:41_

---

_@dcreager approved on 2025-08-08 16:47_

Great catch!

---

_Merged by @AlexWaygood on 2025-08-08 16:50_

---

_Closed by @AlexWaygood on 2025-08-08 16:50_

---

_Branch deleted on 2025-08-08 16:50_

---
