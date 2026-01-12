```yaml
number: 20987
title: "[ty] Fix auto import for files with `from __future__` import"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/fix-future-import
created_at: 2025-10-20T09:41:01Z
updated_at: 2025-10-21T06:14:41Z
url: https://github.com/astral-sh/ruff/pull/20987
synced_at: 2026-01-12T15:57:13Z
```

# [ty] Fix auto import for files with `from __future__` import

---

_@MichaReiser_

## Summary

Ensure that new imports are added after any pre-existing `from __future__` import.

Fixes https://github.com/astral-sh/ty/issues/1382

## Test Plan

Added test


---

_Review requested from @carljm by @MichaReiser on 2025-10-20 09:41_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-10-20 09:41_

---

_Label `bug` added by @MichaReiser on 2025-10-20 09:41_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-20 09:41_

---

_Label `server` added by @MichaReiser on 2025-10-20 09:41_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-20 09:41_

---

_Label `ty` added by @MichaReiser on 2025-10-20 09:41_

---

_Label `bug` added by @MichaReiser on 2025-10-20 09:41_

---

_Label `server` added by @MichaReiser on 2025-10-20 09:41_

---

_Label `ty` added by @MichaReiser on 2025-10-20 09:41_

---

_@sharkdp approved on 2025-10-20 09:44_

Thank you!

---

_@AlexWaygood approved on 2025-10-20 09:46_

Could you also add a test where the first statement in the file is a docstring and the _second_ statement is a future import?

---

_Closed by @MichaReiser on 2025-10-20 11:32_

---

_Reopened by @MichaReiser on 2025-10-20 11:32_

---

_Comment by @github-actions[bot] on 2025-10-20 11:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-20 23:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @MichaReiser on 2025-10-21 06:14_

---

_Closed by @MichaReiser on 2025-10-21 06:14_

---

_Branch deleted on 2025-10-21 06:14_

---
