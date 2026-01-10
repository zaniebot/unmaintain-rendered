```yaml
number: 20966
title: "[ty] fix non-deterministic overload function inference"
type: pull_request
state: merged
author: mtshiba
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: fix-1389
created_at: 2025-10-19T07:20:11Z
updated_at: 2025-10-19T10:18:44Z
url: https://github.com/astral-sh/ruff/pull/20966
synced_at: 2026-01-10T17:34:34Z
```

# [ty] fix non-deterministic overload function inference

---

_Pull request opened by @mtshiba on 2025-10-19 07:20_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1389.

I'm not yet convinced that the bug has been fixed completely, but assuming that the cause of the non-determinism is `FxHash{Set, Map}`, I think this part is the most suspicious. As far as I can see, this is the only place where a non-deterministic operation is performed on `FxHash{Set, Map}`, i.e., where it is used as an iterator.

## Test Plan

I'll run the tests multiple times and check the results.





---

_Comment by @github-actions[bot] on 2025-10-19 07:22_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-19 07:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Closed by @mtshiba on 2025-10-19 07:41_

---

_Reopened by @mtshiba on 2025-10-19 07:41_

---

_Closed by @mtshiba on 2025-10-19 07:59_

---

_Reopened by @mtshiba on 2025-10-19 07:59_

---

_@MichaReiser reviewed on 2025-10-19 08:26_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:261 on 2025-10-19 08:26_

Nit: Could we use an `FxIndexSet`. I believe insertion order should be "good enough."

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:263 on 2025-10-19 09:55_

Can you add a note to the doc-comment that says we use an `IndexSet` here because we need a deterministic iteration order?

---

_@AlexWaygood reviewed on 2025-10-19 09:55_

---

_Label `bug` added by @AlexWaygood on 2025-10-19 10:04_

---

_Label `ty` added by @AlexWaygood on 2025-10-19 10:04_

---

_Renamed from "WIP: [ty] fix non-deterministic overload function inference" to "[ty] fix non-deterministic overload function inference" by @mtshiba on 2025-10-19 10:05_

---

_Marked ready for review by @mtshiba on 2025-10-19 10:06_

---

_Review requested from @carljm by @mtshiba on 2025-10-19 10:06_

---

_Review requested from @sharkdp by @mtshiba on 2025-10-19 10:06_

---

_Review requested from @dcreager by @mtshiba on 2025-10-19 10:06_

---

_@MichaReiser approved on 2025-10-19 10:13_

Thank you

---

_Merged by @MichaReiser on 2025-10-19 10:13_

---

_Closed by @MichaReiser on 2025-10-19 10:13_

---

_Branch deleted on 2025-10-19 10:18_

---
