```yaml
number: 21555
title: "[ty] Improve debug messages when imports fail"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/import-debug-info
created_at: 2025-11-21T12:32:53Z
updated_at: 2025-11-21T13:45:58Z
url: https://github.com/astral-sh/ruff/pull/21555
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Improve debug messages when imports fail

---

_Pull request opened by @AlexWaygood on 2025-11-21 12:32_

## Summary

- Improve a debug message (which is meant to be user-facing but wasn't written in a way that it would help users).
- Add a subdiagnostic if an unresolved relative module name could be resolved were the user to decrease the number of leading dots.

---

_Review requested from @carljm by @AlexWaygood on 2025-11-21 12:32_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-21 12:32_

---

_Label `ty` added by @AlexWaygood on 2025-11-21 12:32_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-21 12:32_

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 12:34_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-21 12:36_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dask/prefect_dask/__init__.py:3:7: error[unresolved-import] Cannot resolve imported module `.client`
+ src/integrations/prefect-dask/prefect_dask/__init__.py:3:7: error[unresolved-import] Cannot resolve imported module `.client` - did you mean `client`?


```

</details>


No memory usage changes detected ✅



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:6029 on 2025-11-21 12:42_

I'm not sure I find this useful; it might even be misleading because it's as likely that the user just used too many dots. What would be useful here is to test if the module can be resolved by reducing the number of dots (when we generate the diagnostic) and make a suggestion based on that.

---

_@MichaReiser reviewed on 2025-11-21 12:42_

---

_@AlexWaygood reviewed on 2025-11-21 12:46_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6029 on 2025-11-21 12:46_

fair

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-21 13:33_

---

_@AlexWaygood reviewed on 2025-11-21 13:33_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6029 on 2025-11-21 13:33_

Pushed a change implementing this

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-21 13:34_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/unresolved_import.md_-_Unresolved_import_di…_-_Using_`from`_with_to…_(4b8ba6ee48180cdd).snap`:43 on 2025-11-21 13:36_

Nice :) 

I would remove the *can*. I think the question mark is enough to indicate that we aren't sure

---

_@MichaReiser approved on 2025-11-21 13:36_

---

_Merged by @AlexWaygood on 2025-11-21 13:45_

---

_Closed by @AlexWaygood on 2025-11-21 13:45_

---

_Branch deleted on 2025-11-21 13:45_

---
