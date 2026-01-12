```yaml
number: 21848
title: "[ty] Supress inlay hints when assigning a trivial initializer call"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/quiet-ctor
created_at: 2025-12-08T15:35:01Z
updated_at: 2025-12-08T15:54:33Z
url: https://github.com/astral-sh/ruff/pull/21848
synced_at: 2026-01-12T15:57:35Z
```

# [ty] Supress inlay hints when assigning a trivial initializer call

---

_@Gankra_

## Summary

By taking a purely syntactic approach to the problem of trivial initializer calls we can supress `x: T = T()`, `x: T = x.y.T()` and `x: MyNewType = MyNewType(0)` but still display `x: T[U] = T()`.

The place where we drop a ball is this does not compose with our analysis for supressing `x = (0, "hello")` as `x = (0, T())` and `x = (T(), T())` will still get inlay hints (I don't think this is a huge deal).

* fixes https://github.com/astral-sh/ty/issues/1516

## Test Plan

Existing snapshots cover this well.


---

_Review requested from @carljm by @Gankra on 2025-12-08 15:35_

---

_Review requested from @MichaReiser by @Gankra on 2025-12-08 15:35_

---

_Review requested from @AlexWaygood by @Gankra on 2025-12-08 15:35_

---

_Review requested from @sharkdp by @Gankra on 2025-12-08 15:35_

---

_Review requested from @dcreager by @Gankra on 2025-12-08 15:35_

---

_Label `server` added by @Gankra on 2025-12-08 15:35_

---

_Label `ty` added by @Gankra on 2025-12-08 15:35_

---

_Renamed from "[ty] supress trivial initializer call inlay hints" to "[ty] supress inlay hints when assigning a trivial initializer call" by @Gankra on 2025-12-08 15:35_

---

_Renamed from "[ty] supress inlay hints when assigning a trivial initializer call" to "[ty] Supress inlay hints when assigning a trivial initializer call" by @Gankra on 2025-12-08 15:35_

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 15:36_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-08 15:38_


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

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5544 diagnostics
+ Found 5543 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/accounting/structures/processed_event.py:85:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/api/rest.py:1047:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2070 diagnostics
+ Found 2072 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@AlexWaygood approved on 2025-12-08 15:52_

Thanks, this looks great!

---

_Merged by @Gankra on 2025-12-08 15:54_

---

_Closed by @Gankra on 2025-12-08 15:54_

---

_Branch deleted on 2025-12-08 15:54_

---
