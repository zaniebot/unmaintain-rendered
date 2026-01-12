```yaml
number: 19790
title: "[ty] Track heap usage of salsa structs"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: ibraheem/structs-memory-usage
created_at: 2025-08-06T19:41:20Z
updated_at: 2025-08-12T11:30:21Z
url: https://github.com/astral-sh/ruff/pull/19790
synced_at: 2026-01-12T15:56:47Z
```

# [ty] Track heap usage of salsa structs

---

_@ibraheemdev_

## Summary

This is the last big remaining item for our Salsa memory reports.

---

_Review requested from @carljm by @ibraheemdev on 2025-08-06 19:41_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-08-06 19:41_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-08-06 19:41_

---

_Review requested from @dcreager by @ibraheemdev on 2025-08-06 19:41_

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-08-06 19:41_

---

_Renamed from "Track heap usage of salsa structs" to "[ty] Track heap usage of salsa structs" by @ibraheemdev on 2025-08-06 19:41_

---

_Label `ty` added by @ibraheemdev on 2025-08-06 19:41_

---

_Label `internal` added by @ibraheemdev on 2025-08-06 19:41_

---

_@ibraheemdev reviewed on 2025-08-06 19:48_

---

_Review comment by @ibraheemdev on `crates/ty_project/src/db.rs`:332 on 2025-08-06 19:48_

Would be nice if there was a `total_size_of_fields` helper.

---

_Comment by @github-actions[bot] on 2025-08-06 19:49_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-06 19:57_

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

_Comment by @github-actions[bot] on 2025-08-06 20:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~10MB
+     struct fields = ~11MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     struct fields = ~15MB
+     struct fields = ~17MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct fields = ~33MB
+     struct fields = ~40MB

```
</details>


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-06 20:57_

---

_Review comment by @MichaReiser on `crates/ruff_memory_usage/src/lib.rs`:17 on 2025-08-07 07:40_

Could we add a simple implementation for now that simply iterates over all values and adds the size of those values. That would be better than just zero (even if not a 100% accurate)

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:152 on 2025-08-07 07:41_

I think I'd prefer a `tracing::warn` over a hard panic (with a fallback to 0). It makes me aware that the numbers are under reporting but it might not matter for the analysis I'm doing right now (so I don't have to fix it immediately)

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:1 on 2025-08-07 07:43_

Unrelated to your changes: Would it make sense for the CI job to show something closer to FULL (while still bucketing the numbers)? I find the current output a good indicator but it isn't actionable because all I know is that memory usage increased or decreased. It might be more helpful if we can pin point the change to specific queries.

---

_@MichaReiser approved on 2025-08-07 07:44_

Thank you, this is great. 

Did you run ty on any large project with memory reporting enabled? Any interesting (large structs) finds?

---

_Comment by @MichaReiser on 2025-08-12 11:10_

I'll merge this because I think it will make @AlexWaygood's life a bit easier and I think all review feedback is addressed too (I just did a merge and added a few more get size implementations)

---

_Merged by @MichaReiser on 2025-08-12 11:28_

---

_Closed by @MichaReiser on 2025-08-12 11:28_

---
