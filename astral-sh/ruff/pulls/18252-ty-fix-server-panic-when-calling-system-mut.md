```yaml
number: 18252
title: "[ty] Fix server panic when calling `system_mut`"
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
head: micha/fix-arc-get-mut-panic
created_at: 2025-05-22T11:56:55Z
updated_at: 2025-05-22T14:10:09Z
url: https://github.com/astral-sh/ruff/pull/18252
synced_at: 2026-01-12T15:56:15Z
```

# [ty] Fix server panic when calling `system_mut`

---

_@MichaReiser_

## Summary

Drop order matters... See inline comment

Fixes https://github.com/astral-sh/ty/issues/469

## Test Plan

I added a fake sleep of 5s to the diagnostics handler. I then typed a ton and wasn't able to reproduce the error where I was before the fix.


---

_Review requested from @carljm by @MichaReiser on 2025-05-22 11:56_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-22 11:56_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-22 11:56_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-22 11:56_

---

_Label `bug` added by @MichaReiser on 2025-05-22 11:57_

---

_Label `server` added by @MichaReiser on 2025-05-22 11:57_

---

_Label `ty` added by @MichaReiser on 2025-05-22 11:57_

---

_Comment by @github-actions[bot] on 2025-05-22 12:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp approved on 2025-05-22 13:41_

Uff — thank you!

---

_Merged by @MichaReiser on 2025-05-22 14:10_

---

_Closed by @MichaReiser on 2025-05-22 14:10_

---

_Branch deleted on 2025-05-22 14:10_

---
