```yaml
number: 20917
title: "[ty] Fix panic 'missing root' when handling completion request"
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
head: micha/fix-missing-root-during-completion
created_at: 2025-10-16T12:03:24Z
updated_at: 2025-10-16T14:23:04Z
url: https://github.com/astral-sh/ruff/pull/20917
synced_at: 2026-01-12T15:57:12Z
```

# [ty] Fix panic 'missing root' when handling completion request

---

_@MichaReiser_

## Summary

Fixes a panic in `list_modules_in` (which is called by the completions request) when the file root for editable installs or a custom typeshed path was missing.

Funnily enough, there were existing tests covering this already. Unfortunately, they also used their custom logic to register the file roots, so that the test passed, but the same request failed in production. 

I used this PR as an opportunity to improve our file-root related logging and the information we include if a file root is missing.

Fixes https://github.com/astral-sh/ty/issues/1363


## Test Plan

Updated existing tests, added new test


---

_Review requested from @carljm by @MichaReiser on 2025-10-16 12:03_

---

_Label `bug` added by @MichaReiser on 2025-10-16 12:03_

---

_Label `server` added by @MichaReiser on 2025-10-16 12:03_

---

_Label `ty` added by @MichaReiser on 2025-10-16 12:03_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-10-16 12:03_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-16 12:03_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-16 12:03_

---

_Label `bug` added by @MichaReiser on 2025-10-16 12:03_

---

_Label `server` added by @MichaReiser on 2025-10-16 12:03_

---

_Label `ty` added by @MichaReiser on 2025-10-16 12:03_

---

_Comment by @github-actions[bot] on 2025-10-16 12:05_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Renamed from "[ty] Fix 'missing root' during completion" to "[ty] Fix panic 'missing root' when handling completion request" by @MichaReiser on 2025-10-16 12:06_

---

_Comment by @github-actions[bot] on 2025-10-16 12:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-16 12:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @Danielkonge on 2025-10-16 14:13_

Just a note: I checked out this branch and tried it out, and it indeed fixes the problem I kept encountering in the linked issue.

---

_@AlexWaygood approved on 2025-10-16 14:18_

With the heavy caveat that I don't know much about the `list_modules_in` function, this makes sense to me

---

_Merged by @MichaReiser on 2025-10-16 14:23_

---

_Closed by @MichaReiser on 2025-10-16 14:23_

---

_Branch deleted on 2025-10-16 14:23_

---
