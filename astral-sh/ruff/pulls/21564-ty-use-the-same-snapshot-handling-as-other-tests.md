```yaml
number: 21564
title: "[ty] Use the same snapshot handling as other tests"
type: pull_request
state: merged
author: Gankra
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: gankra/escape
created_at: 2025-11-21T17:43:18Z
updated_at: 2025-11-21T17:48:03Z
url: https://github.com/astral-sh/ruff/pull/21564
synced_at: 2026-01-12T15:57:28Z
```

# [ty] Use the same snapshot handling as other tests

---

_@Gankra_

Fixes https://github.com/astral-sh/ty/issues/1605

---

_Label `internal` added by @Gankra on 2025-11-21 17:43_

---

_Review requested from @carljm by @Gankra on 2025-11-21 17:43_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-21 17:43_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-21 17:43_

---

_Label `ty` added by @Gankra on 2025-11-21 17:43_

---

_Review requested from @sharkdp by @Gankra on 2025-11-21 17:43_

---

_Review requested from @dcreager by @Gankra on 2025-11-21 17:43_

---

_Comment by @Gankra on 2025-11-21 17:44_

(Yes it just makes it Worse but whatever it's consistent now)

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 17:45_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-21 17:47_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser approved on 2025-11-21 17:47_

I wished there were a better approach to this but yeah, this seems fine

---

_Merged by @Gankra on 2025-11-21 17:48_

---

_Closed by @Gankra on 2025-11-21 17:48_

---

_Branch deleted on 2025-11-21 17:48_

---
