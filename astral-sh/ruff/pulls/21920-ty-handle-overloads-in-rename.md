```yaml
number: 21920
title: "[ty] Handle overloads in rename"
type: pull_request
state: open
author: MichaReiser
labels:
  - server
  - ty
assignees: []
draft: true
base: main
head: micha/rename-overload
created_at: 2025-12-11T15:55:21Z
updated_at: 2025-12-11T16:01:08Z
url: https://github.com/astral-sh/ruff/pull/21920
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Handle overloads in rename

---

_Pull request opened by @MichaReiser on 2025-12-11 15:55_

- **[ty] Reduce size of ty-ide snapshots**
- **[ty] Handle overloads in rename**


---

_Label `server` added by @MichaReiser on 2025-12-11 15:55_

---

_Label `ty` added by @MichaReiser on 2025-12-11 15:55_

---

_Label `server` added by @MichaReiser on 2025-12-11 15:55_

---

_Label `ty` added by @MichaReiser on 2025-12-11 15:55_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 15:59_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-11 16:01_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 41 diagnostics


```

</details>


No memory usage changes detected ✅



---
