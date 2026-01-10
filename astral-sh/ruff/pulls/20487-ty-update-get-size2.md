```yaml
number: 20487
title: "[ty] Update `get-size2`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: ibraheem/get-size-tracker
created_at: 2025-09-19T20:56:52Z
updated_at: 2025-09-22T21:37:47Z
url: https://github.com/astral-sh/ruff/pull/20487
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Update `get-size2`

---

_Pull request opened by @ibraheemdev on 2025-09-19 20:56_

## Summary

Let's see if https://github.com/bircni/get-size2/pull/34 gives us more accurate memory usage numbers.

---

_Label `internal` added by @ibraheemdev on 2025-09-19 20:56_

---

_Label `ty` added by @ibraheemdev on 2025-09-19 20:56_

---

_Comment by @github-actions[bot] on 2025-09-19 20:58_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-19 21:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
- TOTAL MEMORY USAGE: ~69MB
+ TOTAL MEMORY USAGE: ~66MB
-     memo fields = ~57MB
+     memo fields = ~52MB

trio (https://github.com/python-trio/trio)
- TOTAL MEMORY USAGE: ~167MB
+ TOTAL MEMORY USAGE: ~159MB
-     memo fields = ~125MB
+     memo fields = ~113MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~273MB
+ TOTAL MEMORY USAGE: ~260MB
-     memo fields = ~204MB
+     memo fields = ~185MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~596MB
+ TOTAL MEMORY USAGE: ~568MB
-     memo fields = ~445MB
+     memo fields = ~403MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-09-19 21:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @ibraheemdev on 2025-09-22 18:31_

---

_Review requested from @carljm by @ibraheemdev on 2025-09-22 18:31_

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-09-22 18:31_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-09-22 18:31_

---

_Review requested from @dcreager by @ibraheemdev on 2025-09-22 18:31_

---

_@carljm approved on 2025-09-22 19:45_

Looks good, assuming the cancelled walltime benchmarks was something flaky and not a problem related to this PR. (Would recommend getting a clean run to be sure before merging this.)

---

_Merged by @ibraheemdev on 2025-09-22 21:37_

---

_Closed by @ibraheemdev on 2025-09-22 21:37_

---

_Branch deleted on 2025-09-22 21:37_

---
