```yaml
number: 18238
title: "[ty] Fix multithreading related hangs and panics"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: micha-verify-hang
created_at: 2025-05-21T10:56:19Z
updated_at: 2025-06-01T09:34:53Z
url: https://github.com/astral-sh/ruff/pull/18238
synced_at: 2026-01-12T15:56:15Z
```

# [ty] Fix multithreading related hangs and panics

---

_@MichaReiser_

Pulls in https://github.com/salsa-rs/salsa/pull/882 which fixes multiple multithreaded fixpoint related hangs and panics

## Test plan

https://github.com/astral-sh/ruff/pull/18370


---

_Comment by @github-actions[bot] on 2025-05-21 11:04_

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

_Label `ty` added by @AlexWaygood on 2025-05-21 18:14_

---

_Comment by @github-actions[bot] on 2025-05-29 06:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Renamed from "Patch salsa" to "Pull in salsa multithreaded fix point bugfixes" by @MichaReiser on 2025-05-29 12:59_

---

_Renamed from "Pull in salsa multithreaded fix point bugfixes" to "Pull in salsa's multithreaded fix point bugfixes" by @MichaReiser on 2025-05-29 12:59_

---

_Label `bug` added by @MichaReiser on 2025-05-29 13:00_

---

_Renamed from "Pull in salsa's multithreaded fix point bugfixes" to "Fix multithreaded hangs" by @MichaReiser on 2025-05-29 13:00_

---

_Renamed from "Fix multithreaded hangs" to "[ty] Fix multithreaded hangs" by @MichaReiser on 2025-05-29 13:00_

---

_Renamed from "[ty] Fix multithreaded hangs" to "[ty] Fix multithreading related hangs and panics" by @MichaReiser on 2025-05-29 13:01_

---

_@MichaReiser reviewed on 2025-05-29 13:01_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/primer/bad.txt`:8 on 2025-05-29 13:01_

I'll move them to `good` in a separate commit

---

_@MichaReiser reviewed on 2025-06-01 09:06_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:9203 on 2025-06-01 09:06_

Nice @ibraheemdev some of your recent changes reduced the size of `Type` again!

---

_Comment by @MichaReiser on 2025-06-01 09:07_

I'll merge this. I can't see how this would be controversial 😆 but happy to address any feedback post merge

---

_Marked ready for review by @MichaReiser on 2025-06-01 09:07_

---

_Review requested from @carljm by @MichaReiser on 2025-06-01 09:07_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-01 09:07_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-01 09:07_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-01 09:07_

---

_Merged by @MichaReiser on 2025-06-01 09:07_

---

_Closed by @MichaReiser on 2025-06-01 09:07_

---

_Branch deleted on 2025-06-01 09:07_

---

_@ibraheemdev reviewed on 2025-06-01 09:34_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:9203 on 2025-06-01 09:34_

Huh, that's interesting. The only change to the stack types was that `Id` is now aligned to 4-bytes instead of 8.

---
