```yaml
number: 21928
title: "[ty] Ignore `__all__` for document and workspace symbol requests"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/ignore-all-in-symbol-visitor
created_at: 2025-12-11T19:42:20Z
updated_at: 2025-12-12T12:29:31Z
url: https://github.com/astral-sh/ruff/pull/21928
synced_at: 2026-01-12T15:57:36Z
```

# [ty] Ignore `__all__` for document and workspace symbol requests

---

_@BurntSushi_

We also ignore names introduced by import statements, which seems to
match pylance behavior.

Fixes astral-sh/ty#1856


---

_Review requested from @carljm by @BurntSushi on 2025-12-11 19:42_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-12-11 19:42_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-12-11 19:42_

---

_Review requested from @sharkdp by @BurntSushi on 2025-12-11 19:42_

---

_Review requested from @dcreager by @BurntSushi on 2025-12-11 19:42_

---

_Review request for @dcreager removed by @BurntSushi on 2025-12-11 19:43_

---

_Review request for @carljm removed by @BurntSushi on 2025-12-11 19:43_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-12-11 19:43_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-12-11 19:43_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 19:44_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-11 19:46_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `server` added by @AlexWaygood on 2025-12-11 20:25_

---

_Label `ty` added by @AlexWaygood on 2025-12-11 20:25_

---

_@MichaReiser approved on 2025-12-11 21:40_

---

_Merged by @BurntSushi on 2025-12-12 12:29_

---

_Closed by @BurntSushi on 2025-12-12 12:29_

---

_Branch deleted on 2025-12-12 12:29_

---
