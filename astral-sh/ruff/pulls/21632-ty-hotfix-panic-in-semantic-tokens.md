```yaml
number: 21632
title: "[ty] hotfix panic in semantic tokens"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/hotfix
created_at: 2025-11-25T22:00:09Z
updated_at: 2025-11-25T22:25:12Z
url: https://github.com/astral-sh/ruff/pull/21632
synced_at: 2026-01-10T16:48:02Z
```

# [ty] hotfix panic in semantic tokens

---

_Pull request opened by @Gankra on 2025-11-25 22:00_

Fixes https://github.com/astral-sh/ty/issues/1637


---

_Review requested from @carljm by @Gankra on 2025-11-25 22:00_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-25 22:00_

---

_Review requested from @sharkdp by @Gankra on 2025-11-25 22:00_

---

_Review requested from @dcreager by @Gankra on 2025-11-25 22:00_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-25 22:00_

---

_Renamed from "hotfix panic in semantic tokens" to "[ty] hotfix panic in semantic tokens" by @Gankra on 2025-11-25 22:00_

---

_Label `server` added by @Gankra on 2025-11-25 22:00_

---

_Label `ty` added by @Gankra on 2025-11-25 22:00_

---

_Comment by @astral-sh-bot[bot] on 2025-11-25 22:02_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-25 22:04_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/appsec/_handlers.py:404:13: error[invalid-argument-type] Argument to bound method `add_configurations` is incorrect: Expected `list[tuple[str, str, str]]`, found `list[Unknown | tuple[str, int, Unknown]] & list[tuple[str, str, str] | tuple[str, int, Unknown]]`
+ ddtrace/appsec/_handlers.py:404:13: error[invalid-argument-type] Argument to bound method `add_configurations` is incorrect: Expected `list[tuple[str, str, str]]`, found `list[tuple[str, str, str] | tuple[str, int, Unknown]] & list[Unknown | tuple[str, int, Unknown]]`


```

</details>


No memory usage changes detected ✅



---

_Merged by @Gankra on 2025-11-25 22:09_

---

_Closed by @Gankra on 2025-11-25 22:09_

---

_Branch deleted on 2025-11-25 22:09_

---

_Comment by @oconnor663 on 2025-11-25 22:25_

https://github.com/astral-sh/ty/pull/1638/commits/e1ebb3182e50f66fd26169b7792fdd99d7b5e5b4 pulls this into https://github.com/astral-sh/ty/pull/1638.

---
