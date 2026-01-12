```yaml
number: 19824
title: "[ty] Improve performance of subtyping and assignability checks for protocols"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: micha/cyclic-small-cleanup
created_at: 2025-08-08T09:55:41Z
updated_at: 2025-08-08T11:05:14Z
url: https://github.com/astral-sh/ruff/pull/19824
synced_at: 2026-01-12T15:56:48Z
```

# [ty] Improve performance of subtyping and assignability checks for protocols

---

_@MichaReiser_

## Summary

It seems unnecessary to write to `seen` if we have a cache entry (in which case we pop the seen immediately).

I doubt this has any meaningful performance impact but it makes it clearer that `cache` is a superset of `seen`.

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2025-08-08 09:55_

---

_Label `ty` added by @MichaReiser on 2025-08-08 09:55_

---

_Review requested from @carljm by @MichaReiser on 2025-08-08 09:55_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-08-08 09:55_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-08 09:55_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-08 09:55_

---

_Comment by @github-actions[bot] on 2025-08-08 09:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-08-08 10:23_

> I doubt this has any meaningful performance impact

it actually has a 3% perf improvement on `DateType`, a codebase where typechecking necessitates a large number of expensive assignability checks between protocol types! https://codspeed.io/astral-sh/ruff/branches/micha%2Fcyclic-small-cleanup?runnerMode=Instrumentation

---

_Comment by @github-actions[bot] on 2025-08-08 10:24_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@AlexWaygood approved on 2025-08-08 10:25_

---

_Label `internal` removed by @AlexWaygood on 2025-08-08 10:25_

---

_Label `performance` added by @AlexWaygood on 2025-08-08 10:25_

---

_Renamed from "[ty] Small cleanup in `CycleDetector`" to "[ty] Improve performance of subtyping and assignability checks for protocols" by @AlexWaygood on 2025-08-08 10:26_

---

_Merged by @MichaReiser on 2025-08-08 11:05_

---

_Closed by @MichaReiser on 2025-08-08 11:05_

---

_Branch deleted on 2025-08-08 11:05_

---
