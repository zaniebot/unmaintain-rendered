```yaml
number: 18261
title: "[ty] Remove unnecessary lifetimes for `Task`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/remove-unnecessary-lifetimes
created_at: 2025-05-22T19:01:01Z
updated_at: 2025-05-26T12:44:45Z
url: https://github.com/astral-sh/ruff/pull/18261
synced_at: 2026-01-12T15:56:15Z
```

# [ty] Remove unnecessary lifetimes for `Task`

---

_@MichaReiser_

## Summary

We can simply require `'static`, this removes a handful of lifetimes.

## Test Plan

`cargo build`


---

_Review requested from @carljm by @MichaReiser on 2025-05-22 19:01_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-22 19:01_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-22 19:01_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-22 19:01_

---

_Label `internal` added by @MichaReiser on 2025-05-22 19:01_

---

_Label `ty` added by @MichaReiser on 2025-05-22 19:01_

---

_@carljm approved on 2025-05-22 19:46_

I don't know the server codebase, but this looks reasonable.

---

_Comment by @github-actions[bot] on 2025-05-26 12:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @MichaReiser on 2025-05-26 12:44_

---

_Closed by @MichaReiser on 2025-05-26 12:44_

---

_Branch deleted on 2025-05-26 12:44_

---
