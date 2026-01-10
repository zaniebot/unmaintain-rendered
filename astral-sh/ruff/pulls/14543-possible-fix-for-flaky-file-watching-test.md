```yaml
number: 14543
title: Possible fix for flaky file watching test
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/fix-file-watching
created_at: 2024-11-22T21:28:44Z
updated_at: 2024-12-03T07:22:44Z
url: https://github.com/astral-sh/ruff/pull/14543
synced_at: 2026-01-10T20:42:26Z
```

# Possible fix for flaky file watching test

---

_Pull request opened by @MichaReiser on 2024-11-22 21:28_

## Summary

Instead of waiting for a maximum duration, wait for a specific change event and fail if it isn't seen in the specified timeout.
This is also not 100% bulletproof because the event order can differ between OS, and listing all seen events is fairly cumbersome. But hopefully, this gets us to something more predictable and the error message contain more useful information

Fixes https://github.com/astral-sh/ruff/issues/14473

## Test Plan

`cargo test` 

Let's see if it breaks for windows or unix


---

_Review requested from @carljm by @MichaReiser on 2024-11-22 21:28_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-22 21:28_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-22 21:28_

---

_Label `red-knot` added by @MichaReiser on 2024-11-22 21:29_

---

_Converted to draft by @MichaReiser on 2024-11-22 21:29_

---

_Comment by @github-actions[bot] on 2024-11-22 21:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2024-11-29 14:18_

---

_Label `testing` added by @MichaReiser on 2024-11-29 14:18_

---

_@carljm approved on 2024-12-03 03:00_

---

_Merged by @MichaReiser on 2024-12-03 07:22_

---

_Closed by @MichaReiser on 2024-12-03 07:22_

---

_Branch deleted on 2024-12-03 07:22_

---
