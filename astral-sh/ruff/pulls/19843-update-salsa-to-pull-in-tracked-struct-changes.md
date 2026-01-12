```yaml
number: 19843
title: Update salsa to pull in tracked struct changes
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/salsa-tracked-structs
created_at: 2025-08-09T16:30:53Z
updated_at: 2025-08-12T11:17:48Z
url: https://github.com/astral-sh/ruff/pull/19843
synced_at: 2026-01-12T15:56:48Z
```

# Update salsa to pull in tracked struct changes

---

_@MichaReiser_

## Summary
Pulls in https://github.com/salsa-rs/salsa/pull/969


## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2025-08-09 16:33_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-09 16:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo metadata = ~22MB
+     memo metadata = ~21MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     memo metadata = ~40MB
+     memo metadata = ~38MB

prefect (https://github.com/PrefectHQ/prefect)
-     memo metadata = ~80MB
+     memo metadata = ~76MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-08-09 16:44_

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

_Label `ty` added by @AlexWaygood on 2025-08-10 10:29_

---

_Label `internal` added by @MichaReiser on 2025-08-12 10:58_

---

_Marked ready for review by @MichaReiser on 2025-08-12 10:58_

---

_Merged by @MichaReiser on 2025-08-12 11:17_

---

_Closed by @MichaReiser on 2025-08-12 11:17_

---

_Branch deleted on 2025-08-12 11:17_

---
