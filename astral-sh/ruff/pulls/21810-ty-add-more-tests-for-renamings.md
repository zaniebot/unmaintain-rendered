```yaml
number: 21810
title: "[ty] Add more tests for renamings"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - server
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/more-renamings
created_at: 2025-12-05T12:00:38Z
updated_at: 2025-12-05T12:41:32Z
url: https://github.com/astral-sh/ruff/pull/21810
synced_at: 2026-01-12T15:57:34Z
```

# [ty] Add more tests for renamings

---

_@AlexWaygood_

## Summary

Following on from #21809, this PR adds a few more renaming test cases: tests for `@property`s, `@singledispatch`-decorated functions, and `@singledispatchmethod`-decorated methods. The `@singledispatch(method)`-decorated functions all seem to work great, but we don't do as good a job as Pylance on properties with setters and/or deleters right now. (See the TODOs added in these tests.)

## Test Plan

N/A


---

_Label `server` added by @AlexWaygood on 2025-12-05 12:00_

---

_Review requested from @carljm by @AlexWaygood on 2025-12-05 12:00_

---

_Label `testing` added by @AlexWaygood on 2025-12-05 12:00_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-12-05 12:00_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-05 12:00_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-05 12:00_

---

_Label `ty` added by @AlexWaygood on 2025-12-05 12:00_

---

_Comment by @astral-sh-bot[bot] on 2025-12-05 12:02_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-05 12:04_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 38 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2025-12-05 12:13_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/rename.rs`:1488 on 2025-12-05 12:13_

You probably copy pasted my tests where I messed this up. Do we need the subpackage complexity here or could we just go with `main.py` and `lib.py`?

---

_@MichaReiser approved on 2025-12-05 12:14_

---

_@AlexWaygood reviewed on 2025-12-05 12:16_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/rename.rs`:1488 on 2025-12-05 12:16_

Me? Copy and paste? I'm mortally offended (yes, you're right)

---

_Merged by @AlexWaygood on 2025-12-05 12:41_

---

_Closed by @AlexWaygood on 2025-12-05 12:41_

---

_Branch deleted on 2025-12-05 12:41_

---
