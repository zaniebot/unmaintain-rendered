```yaml
number: 12170
title: "Replace `Mutex<RefCell>` with `Mutex` in vendored file system\""
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: vendored-remove-refcell
created_at: 2024-07-03T12:28:41Z
updated_at: 2024-07-03T13:12:14Z
url: https://github.com/astral-sh/ruff/pull/12170
synced_at: 2026-01-12T15:55:40Z
```

# Replace `Mutex<RefCell>` with `Mutex` in vendored file system"

---

_@MichaReiser_

## Summary

I should have noticed this during my review. A mutex is a thread safe `RefCell`. It's, therefore, not necessary to have a `Mutex<RefCell>`.

This PR removes the `RefCell`

I found this because `salsa::Cancelled::catch` requires the `Db` to be `UnwindSafe` and `RefCell`s are not unwind safe.

## Test Plan

`cargo test`.


---

_Label `red-knot` added by @MichaReiser on 2024-07-03 12:28_

---

_Comment by @github-actions[bot] on 2024-07-03 12:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-03 12:45_

---

_@AlexWaygood approved on 2024-07-03 13:09_

Ah, that's much nicer. Thanks!

> A mutex is a thread safe `RefCell`. It's, therefore, not necessary to have a `Mutex<RefCell>`.

TIL. But it makes sense that that's the way it would be implemented :-)

---

_Merged by @MichaReiser on 2024-07-03 13:12_

---

_Closed by @MichaReiser on 2024-07-03 13:12_

---

_Branch deleted on 2024-07-03 13:12_

---
