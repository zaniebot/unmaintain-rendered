```yaml
number: 21912
title: "[ty] fix missing heap_size on Salsa query"
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: cjm/heapsize
created_at: 2025-12-11T01:19:55Z
updated_at: 2025-12-11T02:34:01Z
url: https://github.com/astral-sh/ruff/pull/21912
synced_at: 2026-01-10T16:42:11Z
```

# [ty] fix missing heap_size on Salsa query

---

_Pull request opened by @carljm on 2025-12-11 01:19_

_No description provided._

---

_Review requested from @AlexWaygood by @carljm on 2025-12-11 01:19_

---

_Review requested from @sharkdp by @carljm on 2025-12-11 01:19_

---

_Review requested from @dcreager by @carljm on 2025-12-11 01:19_

---

_Label `internal` added by @carljm on 2025-12-11 01:19_

---

_Label `ty` added by @carljm on 2025-12-11 01:19_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 01:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-11 01:23_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 492 diagnostics
+ Found 490 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 41 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`

prefect (https://github.com/PrefectHQ/prefect)
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`


```

</details>




---

_@AlexWaygood approved on 2025-12-11 02:31_

---

_Merged by @carljm on 2025-12-11 02:34_

---

_Closed by @carljm on 2025-12-11 02:34_

---

_Branch deleted on 2025-12-11 02:34_

---
