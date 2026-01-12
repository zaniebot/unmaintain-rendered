```yaml
number: 18049
title: Avoid initializing progress bars early
type: pull_request
state: merged
author: ibraheemdev
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: ibraheem/progress-late-init
created_at: 2025-05-12T16:27:26Z
updated_at: 2025-05-12T19:07:57Z
url: https://github.com/astral-sh/ruff/pull/18049
synced_at: 2026-01-12T15:56:11Z
```

# Avoid initializing progress bars early

---

_@ibraheemdev_

## Summary

Resolves https://github.com/astral-sh/ty/issues/324#issuecomment-2871032559.


---

_Review requested from @carljm by @ibraheemdev on 2025-05-12 16:27_

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-05-12 16:27_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-05-12 16:27_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-05-12 16:27_

---

_Review requested from @dcreager by @ibraheemdev on 2025-05-12 16:27_

---

_@charliermarsh approved on 2025-05-12 16:28_

---

_Comment by @github-actions[bot] on 2025-05-12 16:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-05-12 16:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-05-12 16:52_

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:358 on 2025-05-12 16:52_

The fact that all check functions now need to be generic (and need to specify the type as well) is somewhat unfortunate. 

I'd have a slight preference towards using an associated type or returning a `Box<dyn>` from `with_files` instead (where `check` could even take a `&dyn Reporter`). 

An alternative would be to initialize indicatif when `set_files` or `report_file` is called first (lazily)



---

_Label `cli` added by @MichaReiser on 2025-05-12 16:54_

---

_Label `ty` added by @MichaReiser on 2025-05-12 16:54_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-12 17:54_

---

_@MichaReiser approved on 2025-05-12 18:37_

---

_Merged by @ibraheemdev on 2025-05-12 19:07_

---

_Closed by @ibraheemdev on 2025-05-12 19:07_

---

_Branch deleted on 2025-05-12 19:07_

---
