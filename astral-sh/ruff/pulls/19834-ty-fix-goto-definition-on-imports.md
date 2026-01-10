```yaml
number: 19834
title: "[ty] fix goto-definition on imports"
type: pull_request
state: merged
author: Gankra
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/hover-import
created_at: 2025-08-08T19:48:46Z
updated_at: 2025-08-08T20:46:30Z
url: https://github.com/astral-sh/ruff/pull/19834
synced_at: 2026-01-10T17:52:17Z
```

# [ty] fix goto-definition on imports

---

_Pull request opened by @Gankra on 2025-08-08 19:48_

The stub mapper wasn't being passed into this codepath. It is now being used. A previously messed up test result I intentionally checked in was subsequently fixed.

---

_Label `bug` added by @Gankra on 2025-08-08 19:48_

---

_Label `server` added by @Gankra on 2025-08-08 19:48_

---

_Label `ty` added by @Gankra on 2025-08-08 19:48_

---

_Review requested from @carljm by @Gankra on 2025-08-08 19:48_

---

_Review requested from @MichaReiser by @Gankra on 2025-08-08 19:48_

---

_Review requested from @AlexWaygood by @Gankra on 2025-08-08 19:48_

---

_Review requested from @sharkdp by @Gankra on 2025-08-08 19:48_

---

_Review requested from @dcreager by @Gankra on 2025-08-08 19:48_

---

_Comment by @github-actions[bot] on 2025-08-08 19:50_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-08 19:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@carljm approved on 2025-08-08 20:28_

---

_Merged by @Gankra on 2025-08-08 20:46_

---

_Closed by @Gankra on 2025-08-08 20:46_

---

_Branch deleted on 2025-08-08 20:46_

---
