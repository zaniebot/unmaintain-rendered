```yaml
number: 18201
title: "[ty] Small LSP cleanups"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/lsp-refactor
created_at: 2025-05-19T17:06:02Z
updated_at: 2025-05-19T19:02:14Z
url: https://github.com/astral-sh/ruff/pull/18201
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Small LSP cleanups

---

_Pull request opened by @MichaReiser on 2025-05-19 17:06_

## Summary

* Add logging when the LSP ignores request
* Return an error response when the method isn't supported (instead of silently ignoring the message). r-a does the same
* Make `db` the first argument for request handlers
* Pass `db` by reference. This avoids the annoying `&db` in request handlers

## Test Plan

`cargo build`


---

_Review requested from @carljm by @MichaReiser on 2025-05-19 17:06_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-19 17:06_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-19 17:06_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-19 17:06_

---

_Label `server` added by @MichaReiser on 2025-05-19 17:06_

---

_Label `ty` added by @MichaReiser on 2025-05-19 17:06_

---

_Label `internal` added by @MichaReiser on 2025-05-19 17:06_

---

_Comment by @github-actions[bot] on 2025-05-19 17:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @MichaReiser on 2025-05-19 17:08_

---

_Closed by @MichaReiser on 2025-05-19 17:08_

---

_Branch deleted on 2025-05-19 17:09_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-19 17:09_

---

_Comment by @MichaReiser on 2025-05-19 19:02_

Using a db ref is also more correct. We otherwise allow request handlers to call mutable db methods which we don't want to

---
