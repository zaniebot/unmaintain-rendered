```yaml
number: 21548
title: "[ty] More low-hanging fruit for inlay hint goto-definition"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: alex/even-more-details
created_at: 2025-11-20T20:40:44Z
updated_at: 2025-11-20T23:16:00Z
url: https://github.com/astral-sh/ruff/pull/21548
synced_at: 2026-01-10T16:48:02Z
```

# [ty] More low-hanging fruit for inlay hint goto-definition

---

_Pull request opened by @AlexWaygood on 2025-11-20 20:40_

## Summary

Enable users to jump to `int` from the inlay for a variable inferred as `Literal[42]`, and some other easy things

## Test Plan

snapshots


---

_Review requested from @Gankra by @AlexWaygood on 2025-11-20 20:40_

---

_Label `server` added by @AlexWaygood on 2025-11-20 20:40_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-20 20:40_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-20 20:40_

---

_Label `ty` added by @AlexWaygood on 2025-11-20 20:40_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-20 20:40_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-20 20:40_

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 20:43_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-20 20:44_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review request for @carljm removed by @carljm on 2025-11-20 21:55_

---

_@Gankra reviewed on 2025-11-20 23:09_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/display.rs`:869 on 2025-11-20 23:09_

Hmm I think I disagree? From what I've seen LiteralString only shows up in code that's explicitly declared e.g. a function argument as LiteralString, but maybe I've just been lucky? (not a strong opinion)

---

_@Gankra approved on 2025-11-20 23:11_

nice!

---

_@AlexWaygood reviewed on 2025-11-20 23:15_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:869 on 2025-11-20 23:15_

hmm, you could be hovering over `os.name` or some other constant that's annotated (but not by you) as `LiteralString`? I don't have a strong opinion either; we can easily change this in response to user feedback if we get some!

---

_Merged by @AlexWaygood on 2025-11-20 23:15_

---

_Closed by @AlexWaygood on 2025-11-20 23:15_

---

_Branch deleted on 2025-11-20 23:16_

---
