```yaml
number: 21994
title: "[ty] Remove extra headings and split examples in the `overrides` configuration docs"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: zb/ty-overrides-config
created_at: 2025-12-16T02:46:27Z
updated_at: 2025-12-16T12:57:08Z
url: https://github.com/astral-sh/ruff/pull/21994
synced_at: 2026-01-12T15:57:38Z
```

# [ty] Remove extra headings and split examples in the `overrides` configuration docs

---

_@zanieb_

Having these as markdown headings ends up being weird in the reference documentation, e.g., before:

<img width="1071" height="779" alt="Screenshot 2025-12-15 at 8 45 25 PM" src="https://github.com/user-attachments/assets/2118d4f1-f557-46f3-a4b6-56c406cf9aca" />


---

_Label `documentation` added by @zanieb on 2025-12-16 02:46_

---

_Comment by @astral-sh-bot[bot] on 2025-12-16 02:48_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Renamed from "[ty] Remove extra headings from the `overrides` configuration docs" to "[ty] Remove extra headings and split examples in the `overrides` configuration docs" by @zanieb on 2025-12-16 02:49_

---

_Comment by @astral-sh-bot[bot] on 2025-12-16 02:54_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5123 diagnostics
+ Found 5124 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2025-12-16 09:30_

---

_Marked ready for review by @zanieb on 2025-12-16 12:51_

---

_Review requested from @carljm by @zanieb on 2025-12-16 12:51_

---

_Review requested from @MichaReiser by @zanieb on 2025-12-16 12:51_

---

_Review requested from @sharkdp by @zanieb on 2025-12-16 12:51_

---

_Review requested from @dcreager by @zanieb on 2025-12-16 12:51_

---

_@MichaReiser approved on 2025-12-16 12:52_

---

_Merged by @zanieb on 2025-12-16 12:57_

---

_Closed by @zanieb on 2025-12-16 12:57_

---

_Branch deleted on 2025-12-16 12:57_

---
