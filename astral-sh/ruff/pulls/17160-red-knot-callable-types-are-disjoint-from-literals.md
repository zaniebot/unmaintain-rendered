```yaml
number: 17160
title: "[red-knot] Callable types are disjoint from literals"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-disjoint-literal
created_at: 2025-04-02T21:21:13Z
updated_at: 2025-04-02T22:08:15Z
url: https://github.com/astral-sh/ruff/pull/17160
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Callable types are disjoint from literals

---

_Pull request opened by @dhruvmanila on 2025-04-02 21:21_

## Summary

A callable type is disjoint from other literal types. For example, `Type::StringLiteral` must be an instance of exactly `str`, not a subclass of `str`, and `str` is not callable. The same applies to other literal types.

This should hopefully fix #17144, I couldn't produce any failures after running property tests multiple times.

## Test Plan

Add test cases for disjointness check between callable and other literal types.

Run property tests multiple times.

---

_Label `bug` added by @dhruvmanila on 2025-04-02 21:21_

---

_Label `red-knot` added by @dhruvmanila on 2025-04-02 21:21_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-02 21:21_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-02 21:21_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-02 21:21_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-02 21:21_

---

_@dhruvmanila reviewed on 2025-04-02 21:21_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:1408 on 2025-04-02 21:21_

Other literals are handled directly with a catch-all combination:

* `BooleanLiteral`: https://github.com/astral-sh/ruff/blob/c2bb5d52507621ec5905dbb72ce187d989dd934f/crates/red_knot_python_semantic/src/types.rs#L1311-L1311
* `IntLiteral`: https://github.com/astral-sh/ruff/blob/c2bb5d52507621ec5905dbb72ce187d989dd934f/crates/red_knot_python_semantic/src/types.rs#L1320-L1320
* `LiteralString`: https://github.com/astral-sh/ruff/blob/c2bb5d52507621ec5905dbb72ce187d989dd934f/crates/red_knot_python_semantic/src/types.rs#L1339-L1339

---

_Comment by @github-actions[bot] on 2025-04-02 21:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood approved on 2025-04-02 21:35_

Thank you! One interesting thing to note is that `Callable` types are _not_ necessarily disjoint from `ModuleLiteral` types, as it _is_ possible for a module to be a subclass of `types.ModuleType` (you have to manually inject it into `sys.modules` at runtime, but once it's imported in another module it _would_ be recognised by red-knot as a `ModuleLiteral` type). And that subclass of `types.ModuleType` could possibly be callable.

This isn't something you get wrong in this PR, just something you might find interesting!

---

_Merged by @dhruvmanila on 2025-04-02 22:08_

---

_Closed by @dhruvmanila on 2025-04-02 22:08_

---

_Branch deleted on 2025-04-02 22:08_

---
