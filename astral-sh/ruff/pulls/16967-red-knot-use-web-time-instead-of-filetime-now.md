```yaml
number: 16967
title: "[red-knot] Use `web-time` instead of `FileTime::now`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/file-time-now-wasm
created_at: 2025-03-25T12:55:44Z
updated_at: 2025-03-25T13:05:11Z
url: https://github.com/astral-sh/ruff/pull/16967
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Use `web-time` instead of `FileTime::now`

---

_Pull request opened by @MichaReiser on 2025-03-25 12:55_

## Summary

`std::time::now` isn't available on `wasm32-unknown-unknown` but it is used by `FileTime::now`. 

This PR replaces the usages of `FileTime::now` with a target specific helper function that we already had in the memory file system.
Fixes https://github.com/astral-sh/ruff/issues/16966

## Test Plan

Tested that the playground no longer crash when adding an extra-path



---

_Review requested from @carljm by @MichaReiser on 2025-03-25 12:55_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-25 12:55_

---

_Review requested from @sharkdp by @MichaReiser on 2025-03-25 12:55_

---

_Review requested from @dcreager by @MichaReiser on 2025-03-25 12:55_

---

_Label `playground` added by @MichaReiser on 2025-03-25 12:55_

---

_Label `red-knot` added by @MichaReiser on 2025-03-25 12:55_

---

_Merged by @MichaReiser on 2025-03-25 13:03_

---

_Closed by @MichaReiser on 2025-03-25 13:03_

---

_Branch deleted on 2025-03-25 13:03_

---

_Comment by @github-actions[bot] on 2025-03-25 13:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---
