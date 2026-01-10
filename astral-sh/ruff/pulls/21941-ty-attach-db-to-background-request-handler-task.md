```yaml
number: 21941
title: "[ty] Attach db to background request handler task"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/background-request-handler-db
created_at: 2025-12-12T11:06:43Z
updated_at: 2025-12-12T11:31:15Z
url: https://github.com/astral-sh/ruff/pull/21941
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Attach db to background request handler task

---

_Pull request opened by @MichaReiser on 2025-12-12 11:06_

## Summary

Attach the salsa db before calling a background request handler so that `dbg!(salsa_struct)` prints more than just the salsa id.




---

_Review requested from @carljm by @MichaReiser on 2025-12-12 11:06_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-12 11:06_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-12 11:06_

---

_Label `internal` added by @MichaReiser on 2025-12-12 11:06_

---

_Label `server` added by @MichaReiser on 2025-12-12 11:06_

---

_Label `ty` added by @MichaReiser on 2025-12-12 11:06_

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 11:08_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 11:10_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5133 diagnostics
+ Found 5132 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@dhruvmanila approved on 2025-12-12 11:27_

---

_Merged by @MichaReiser on 2025-12-12 11:31_

---

_Closed by @MichaReiser on 2025-12-12 11:31_

---

_Branch deleted on 2025-12-12 11:31_

---
